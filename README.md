<div align="center">
  <img src="https://raw.githubusercontent.com/spyglass-so/spyglass-typescript/main/assets/wordmark.svg" alt="Spyglass" width="360">

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
  <a href="https://github.com/spyglass-so/spyglass-typescript/blob/main/LICENSE">
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

**`@spyglasshq/sdk`** is the official TypeScript and Node.js client for the [Spyglass API](https://docs.spyglass.so). Resolve brands and creators, search across millions of Meta, Instagram, and TikTok creatives with natural-language briefs or structured filters, and extract hooks, personas, landing pages, and public comments.

> Looking for Python? See [`spyglass-so/spyglass-python`](https://github.com/spyglass-so/spyglass-python) (`pip install spyglass-sdk`).

## Table of Contents

- [Installation](#installation)
- [Quickstart](#quickstart)
- [Core Workflows](#core-workflows)
  - [Search Brands](#1-search-brands)
  - [Search Ad Creatives & Social Media](#2-search-ad-creatives--social-media)
  - [Discover Creators](#3-discover-creators)
  - [Creative Insights & Landing Pages](#4-creative-insights--landing-pages)
  - [Post Comments](#5-post-comments)
  - [Index a New Brand](#6-index-a-new-brand)
  - [Check Team Credit Usage](#7-check-team-credit-usage)
- [API Reference Overview](#api-reference-overview)
- [TypeScript Types](#typescript-types)
- [Error Handling](#error-handling)
- [Advanced Configuration](#advanced-configuration)
  - [Retries, Timeouts & Abort Signals](#retries-timeouts--abort-signals)
  - [Raw Response & Headers](#raw-response--headers)
  - [Tree-Shakeable Subpackage Imports](#tree-shakeable-subpackage-imports)
  - [Logging](#logging)
  - [Runtime Compatibility](#runtime-compatibility)
- [Billing & Conventions](#billing--conventions)

## Installation

```bash
npm install @spyglasshq/sdk
# or
pnpm add @spyglasshq/sdk
# or
yarn add @spyglasshq/sdk
# or
bun add @spyglasshq/sdk
```

## Quickstart

1. Create an API key in **[Settings → API](https://app.spyglass.so)**. Keys start with `sg_live_` and are shown once.
2. Initialize `Spyglass` and make your first call:

```ts
import { Spyglass } from "@spyglasshq/sdk";

const client = new Spyglass({
  token: process.env.SPYGLASS_API_KEY!,
});

// 1. Resolve a brand by name
const { brands } = await client.search.brands({
  names: ["Nike"],
  limit: 1,
});

const brand = brands[0];

if (brand) {
  // 2. Search the creatives that brand is running
  const { media } = await client.search.getMedia({
    brandId: brand.id,
    query: "marathon running shoes comfort",
    limit: 10,
  });

  for (const ad of media) {
    console.log({
      id: ad.id,
      hook: ad.hook,
      coreAngle: ad.coreAngle,
      targetPersona: ad.targetPersona,
      daysRun: ad.daysRun,
      url: ad.url,
    });
  }
}
```

---

## Core Workflows

### 1. Search Brands

Search advertisers by name (`names`), social handle (`igUsername`, `tiktokUsername`), known IDs (`brandIds`), or a natural-language brief (`query`). You can also combine `names` + `query` to rank a specific set of brands against a brief.

```ts
// Fuzzy name lookup
const byName = await client.search.brands({
  names: ["Athletic Greens", "AG1"],
  limit: 5,
});

// Natural-language discovery
const byBrief = await client.search.brands({
  query: "DTC electrolyte and hydration powder brands",
  limit: 10,
});
```

### 2. Search Ad Creatives & Social Media

Pass `brandId` to inspect a specific brand or creator's creatives, or omit `brandId` and pass `query` to search across the entire Spyglass index. Combine semantic search with structured filters:

```ts
// Optional: look up valid product taxonomy paths first (free call)
const { taxonomies } = await client.search.taxonomies({
  query: "running shoes",
});

const { media, count } = await client.search.getMedia({
  query: "UGC founder story explaining product benefits",
  channel: ["META_AD", "TIKTOK"],
  format: ["VIDEO"],
  status: "ACTIVE",
  taxonomy: taxonomies.slice(0, 2),
  minDaysRun: 14,
  limit: 20,
  offset: 0,
});
```

Each item in `media` includes creative intelligence extracted by Spyglass:
- **Copy & Strategy:** `hook`, `summary`, `coreAngle`, `targetPersona`, `painPoints`, `sellingPoints`
- **Creative Metadata:** `type` (`VIDEO`, `IMAGE`, `CAROUSEL`, `DPA`, `DCO`), `durationInSeconds`, `url`, `urls`, `thumbnailUrl`, `ugcCharacter`
- **Distribution & Performance:** `platforms`, `startAt`, `endAt`, `daysRun`, `viewCount`, `likeCount`, `commentCount`, `impressionPercentile`

### 3. Discover Creators

Search creators by handle, name, or natural-language brief, and filter by demographic and profile attributes (`gender`, `ageRange`, `ethnicity`, `location`, `language`):

```ts
const { creators } = await client.search.creators({
  query: "skincare routine and dermatology tips",
  gender: "female",
  ageRange: ["25-34"],
  location: ["US"],
  language: ["en"],
  limit: 20,
});
```

### 4. Creative Insights & Landing Pages

Pull clustered creative themes (`HOOK`, `USP`, `PERSONA`, `MEDIA_MIX`) and ranked destination landing pages for any brand:

```ts
// Ranked hooks + weekly volume breakdown over the last 90 days
const insights = await client.search.getBrandMediaInsights({
  id: brand.id,
  type: "HOOK",
  maxDaysBack: 90,
  limit: 10,
});

// Top landing pages where the brand sends ad traffic
const { landingPages } = await client.search.getBrandLandingPages({
  id: brand.id,
  showInactive: "false",
  limit: 20,
});
```

### 5. Post Comments

Fetch public comments from Instagram posts/reels (by shortcode or URL) and TikTok videos (by video ID or URL):

```ts
const { posts } = await client.search.getComments({
  instagramIds: ["https://www.instagram.com/reel/C2xY9abcD1e/"],
  tiktokIds: ["https://www.tiktok.com/@nike/video/7234567890123456789"],
  limit: 1, // comment pages per post
});
```

### 6. Index a New Brand

If Spyglass doesn't have a brand yet, submit its website or social profile URL (`POST /account/index` is free) and poll until `status` is `"ready"`:

```ts
const indexed = await client.indexing.indexBrand({
  websiteUrl: "https://www.on.com",
});

// "already_exists" can be queried immediately; "indexing" queues a background sync
let status = indexed.status;
while (status === "indexing") {
  await new Promise((resolve) => setTimeout(resolve, 3000));
  const check = await client.indexing.indexBrandStatus({ id: indexed.brandId });
  status = check.status;
}

if (status === "ready" || status === "already_exists") {
  const account = await client.search.getAccount({ id: indexed.brandId });
  console.log("Indexed account:", account.name);
}
```

### 7. Check Team Credit Usage

Check your team's remaining monthly credits and current billing window (free call):

```ts
const usage = await client.account.getCreditUsage();
console.log(`Remaining: ${usage.remainingCredits} / ${usage.planCredits}`);
```

---

## API Reference Overview

| SDK Method | HTTP Endpoint | Cost | Rate Limit |
| --- | --- | --- | --- |
| `client.search.brands(req)` | `POST /brands` | $0.0020 / brand returned | 120 / min |
| `client.search.creators(req)` | `POST /creators` | $0.0020 / creator returned | 120 / min |
| `client.search.getAccount({ id })` | `GET /account/{id}` | $0.0020 / account returned | 120 / min |
| `client.search.getMedia(req)` | `POST /media` | $0.0010 / creative returned | 120 / min |
| `client.search.taxonomies({ query })` | `GET /taxonomies` | Free | 120 / min |
| `client.search.getBrandMediaInsights(req)` | `GET /account/{id}/insights` | $0.0010 / cluster returned | 120 / min |
| `client.search.getBrandLandingPages(req)` | `GET /account/{id}/landing-pages` | $0.0050 / landing page returned | 120 / min |
| `client.search.getComments(req)` | `POST /comments` | $0.0100 / live page fetched | 20 / min |
| `client.indexing.indexBrand(req)` | `POST /account/index` | Free | 50 / min |
| `client.indexing.indexBrandStatus({ id })` | `GET /account/{id}/index-status` | Free | 300 / min |
| `client.account.getCreditUsage()` | `GET /team/credit-usage` | Free | 120 / min |

Full method signatures and parameter types are available in [`reference.md`](https://github.com/spyglass-so/spyglass-typescript/blob/main/reference.md) and at [docs.spyglass.so](https://docs.spyglass.so).

---

## TypeScript Types

All request and response types are exported under the `spyglass` namespace:

```ts
import { spyglass } from "@spyglasshq/sdk";

const request: spyglass.GetMediaRequest = {
  query: "summer sale",
  channel: ["META_AD"],
  limit: 25,
};

type MediaItem = spyglass.GetMediaResponse.Media.Item;
```

## Error Handling

When the API returns a non-2xx status code (`400`, `401`, `402` out of credits, `404`, `429` rate limit, or `5xx`), the SDK throws a `SpyglassError` (or a status-specific subclass such as `PaymentRequiredError` or `TooManyRequestsError`):

```ts
import { SpyglassError } from "@spyglasshq/sdk";

try {
  await client.search.brands({ names: ["Nike"], limit: 5 });
} catch (err) {
  if (err instanceof SpyglassError) {
    console.error("Status:", err.statusCode);
    console.error("Message:", err.message);
    console.error("Body:", err.body);
  }
}
```

---

## Advanced Configuration

### Retries, Timeouts & Abort Signals

The SDK defaults to a **60-second timeout** and **2 automatic retries** with exponential backoff on `408`, `429`, and `5xx` responses. You can configure these on the client or override them per request:

```ts
const controller = new AbortController();

const response = await client.search.getMedia(
  { query: "protein bar", limit: 10 },
  {
    timeoutInSeconds: 30,
    maxRetries: 3,
    abortSignal: controller.signal,
    headers: { "X-Custom-Header": "value" },
  },
);
```

### Raw Response & Headers

Chain `.withRawResponse()` on any request promise to inspect HTTP status codes and headers alongside parsed data:

```ts
const { data, rawResponse } = await client.search
  .brands({ names: ["Nike"], limit: 1 })
  .withRawResponse();

console.log(data.brands);
console.log(rawResponse.headers.get("retry-after"));
```

### Tree-Shakeable Subpackage Imports

Import individual resource clients directly for minimal bundle sizes:

```ts
import { SearchClient } from "@spyglasshq/sdk/search";
import { IndexingClient } from "@spyglasshq/sdk/indexing";
import { AccountClient } from "@spyglasshq/sdk/account";
```

### Logging

Enable debug or info logging on the client:

```ts
import { Spyglass, logging } from "@spyglasshq/sdk";

const client = new Spyglass({
  token: process.env.SPYGLASS_API_KEY!,
  logging: {
    level: logging.LogLevel.Debug,
    logger: new logging.ConsoleLogger(),
    silent: false,
  },
});
```

### Runtime Compatibility

Zero external runtime dependencies. Works out of the box in:
- **Node.js** 18+
- **Bun** 1.0+
- **Deno** v1.25+
- **Vercel Functions** & **Cloudflare Workers**

## Billing & Conventions

- **Credits per row:** Most endpoints charge against your monthly plan credits per item returned (`count` in the response). Always bound requests with `limit`. Semantic `query` searches on `/media`, `/brands`, and `/creators` run a language model and bill a minimum of 1 row even when 0 rows match. See [Billing docs](https://docs.spyglass.so/documentation/get-started/billing).
- **Pagination:** Page through results with `offset` and `limit`. `count` is the number of rows in *this* response (not the total across all pages). Stop paging when `count < limit`.
- **Server-side only:** The Spyglass API is designed for backend services, data pipelines, and AI agents. Do not expose your `sg_live_` key in browser client code.

## Support

- **Documentation:** [docs.spyglass.so](https://docs.spyglass.so)
- **API Playground:** [app.spyglass.so/playground](https://app.spyglass.so/playground)
- **Issues:** [github.com/spyglass-so/spyglass-typescript/issues](https://github.com/spyglass-so/spyglass-typescript/issues)
- **Email:** [support@spyglass.so](mailto:support@spyglass.so)

## License

[MIT](https://github.com/spyglass-so/spyglass-typescript/blob/main/LICENSE)
