<div align="center">
  <img src="./assets/wordmark.svg" alt="Spyglass" width="360">

  <h3>The API layer for the social internet</h3>
  <p>Official TypeScript & Node.js SDK for the Spyglass API — search brands, creators, and the ads they run.</p>

  <a href="https://app.spyglass.so">
    <img src="https://img.shields.io/badge/Get_Started-111111?style=for-the-badge" alt="Get Started">
  </a>
  <a href="https://docs.spyglass.so">
    <img src="https://img.shields.io/badge/Documentation-4A4A4A?style=for-the-badge" alt="Documentation">
  </a>
  <a href="https://spyglass.so">
    <img src="https://img.shields.io/badge/spyglass.so-0A0A0A?style=for-the-badge" alt="spyglass.so">
  </a>
</div>

<br>

<div align="center">
  <a href="./LICENSE">
    <img src="https://img.shields.io/github/license/spyglass-so/spyglass-typescript" alt="License">
  </a>
  <a href="https://github.com/spyglass-so/spyglass-typescript/stargazers">
    <img src="https://img.shields.io/github/stars/spyglass-so/spyglass-typescript?style=social" alt="Stars">
  </a>
  <a href="https://www.npmjs.com/package/@spyglasshq/sdk">
    <img src="https://img.shields.io/npm/v/@spyglasshq/sdk" alt="npm">
  </a>
  <a href="https://github.com/spyglass-so/spyglass-python">
    <img src="https://img.shields.io/badge/Python_SDK-spyglass--sdk-3776AB?logo=python&logoColor=white" alt="Python SDK">
  </a>
</div>

---

Official TypeScript client for the [Spyglass API](https://docs.spyglass.so). Server-side only. Looking for Python? See [`spyglass-so/spyglass-python`](https://github.com/spyglass-so/spyglass-python).

## Installation

```bash
npm install @spyglasshq/sdk
# or
pnpm add @spyglasshq/sdk
```

## Quickstart

Create an API key in [Settings → API](https://app.spyglass.so). Keys start with `sg_live_` and are shown once.

```ts
import { SpyglassClient } from "@spyglasshq/sdk";

const client = new SpyglassClient({
  token: process.env.SPYGLASS_API_KEY!,
});

// 1. Resolve a brand
const { brands } = await client.search.searchBrandsV2({
  names: ["Nike"],
  limit: 1,
});

const brand = brands[0];

if (brand) {
  // 2. Pull the creatives that brand is running
  const { media } = await client.search.getMediaV2({
    brandId: brand.id,
    query: "running shoes comfort",
    limit: 20,
  });

  for (const ad of media) {
    console.log(ad.id, ad.hook, ad.landingPageUrl);
  }
}
```

## What You Can Query

| Method | Endpoint | What it answers |
| --- | --- | --- |
| `client.search.searchBrandsV2(...)` | `POST /brands` | Which brand is this? Who else is in this space? |
| `client.search.creators(...)` | `POST /creators` | Which creator is this? Who matches this audience profile? |
| `client.search.getAccount(...)` | `GET /account/{id}` | What do we know about a brand or creator ID you already have? |
| `client.search.getMediaV2(...)` | `POST /media` | What creatives is this account running, or what's out there for this theme? |
| `client.search.taxonomies(...)` | `GET /taxonomies` | Which product-category paths match this phrase? |
| `client.search.getBrandMediaInsights(...)` | `GET /account/{id}/insights` | What hooks, USPs, personas, and formats does it lean on? |
| `client.search.getBrandLandingPages(...)` | `GET /account/{id}/landing-pages` | Where does its ad traffic go? |
| `client.search.getComments(...)` | `POST /comments` | What are people saying on this Instagram or TikTok post? |
| `client.indexing.indexBrand(...)` | `POST /account/index` | Index a brand we don't have yet by website or social URL. |
| `client.indexing.indexBrandStatus(...)` | `GET /account/{id}/index-status` | Has that brand finished indexing? |
| `client.account.getCreditUsage()` | `GET /team/credit-usage` | How many monthly credits does this team have left? |

Full parameters and response schemas are documented in [`reference.md`](./reference.md) and at [docs.spyglass.so](https://docs.spyglass.so).

## Indexing a New Brand

If Spyglass doesn't have a brand yet, queue it by website or social profile URL and poll until `status` is `"ready"`:

```ts
const indexed = await client.indexing.indexBrand({
  websiteUrl: "https://www.nike.com",
});

if (indexed.status === "indexing") {
  let status = indexed.status;
  while (status === "indexing") {
    await new Promise((r) => setTimeout(r, 3000));
    const check = await client.indexing.indexBrandStatus({ id: indexed.brandId });
    status = check.status;
  }
}
```

## Request & Response Types

All request and response types are exported under the `Spyglass` namespace:

```ts
import { Spyglass } from "@spyglasshq/sdk";

const request: Spyglass.SearchBrandsV2Request = {
  names: ["Nike"],
  limit: 5,
};
```

## Error Handling

Non-2xx responses throw a `SpyglassError` with status code, parsed body, and raw response metadata:

```ts
import { SpyglassError } from "@spyglasshq/sdk";

try {
  await client.search.searchBrandsV2({ names: ["Nike"], limit: 5 });
} catch (err) {
  if (err instanceof SpyglassError) {
    console.error(err.statusCode, err.message, err.body);
  }
}
```

## Advanced Options

### Retries & Timeouts

Requests default to a 60-second timeout and up to 2 automatic retries with exponential backoff on `408`, `429`, and `5xx` responses. Override per request:

```ts
const response = await client.search.searchBrandsV2(
  { names: ["Nike"], limit: 5 },
  {
    timeoutInSeconds: 30,
    maxRetries: 3,
    abortSignal: controller.signal,
  },
);
```

### Raw Response & Headers

Use `.withRawResponse()` to inspect HTTP headers (such as rate-limit or credit headers):

```ts
const { data, rawResponse } = await client.search
  .searchBrandsV2({ names: ["Nike"], limit: 1 })
  .withRawResponse();

console.log(data.brands);
console.log(rawResponse.headers);
```

### Subpackage Imports

Import individual resource clients directly for smaller bundle sizes:

```ts
import { SearchClient } from "@spyglasshq/sdk/search";
```

### Runtime Compatibility

- Node.js 18+
- Bun 1.0+
- Deno v1.25+
- Vercel / Cloudflare Workers

## Billing & Conventions

- **Credits:** Most calls spend monthly credits per row returned (`count` in the response). Always bound requests with `limit`. See [Billing docs](https://docs.spyglass.so/documentation/get-started/billing).
- **Pagination:** Page through results with `offset` and `limit`. `count` is the number of rows in the current response.
- **Server-to-server:** The API does not enable browser CORS; call it from your backend or agent runtime.

## Support

- **Documentation:** [docs.spyglass.so](https://docs.spyglass.so)
- **Issues:** [GitHub Issues](https://github.com/spyglass-so/spyglass-typescript/issues)
- **Email:** [support@spyglass.so](mailto:support@spyglass.so)

## License

[MIT](./LICENSE)
