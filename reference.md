# Reference
<details><summary><code>client.<a href="/src/Client.ts">searchBrands</a>({ ...params }) -> spyglass.SearchBrandsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Find brands.
Search semantically with `query`, or look them up by name, id, or
social handle. Either way, you can also narrow the results with
filters such as product taxonomy, location, and follower count.

Send one search field. Each field is its own mode:

- **Query** (`query`). A natural-language brief. Runs a language model and
  ranks by relevance, so `sortBy` is ignored. Bills at least one row even
  when nothing matches.
- **Name** (`names`). Fuzzy-matches account names (a case-insensitive contains
  match). Pass every name in one array.
- **ID** (`brandIds`). Looks up known ids. Missing ids are omitted and listed
  in `unmatchedIds`. `limit` and `offset` are ignored.
- **Instagram** (`igUsername`). Case-insensitive contains match on the
  handle. A leading `@` is ignored.
- **TikTok** (`tiktokUsername`). Same match on the TikTok handle.

If a request includes more than one of these fields, only the highest
priority field is used. The rest are ignored:

1. `brandIds`
2. `igUsername` or `tiktokUsername` (if both are set, either handle can match)
3. `names`
4. `query`

One exception: `query` together with `names` uses both. `names` narrows the
set, and `query` ranks inside it.

`limit` (default 20, max 100) caps the rows returned, and the cost, for
every mode except ID.

Limited to 120 requests per minute per user.
Costs 2,000 microdollars ($0.0020) per brand returned, charged against your plan's monthly credits. The count field is what you were charged for.
A `query` search runs a language model, so it bills a minimum of one
row even when nothing matches.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.searchBrands({
    names: ["Nike"],
    limit: 3
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.SearchBrandsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Spyglass.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">searchCreators</a>({ ...params }) -> spyglass.SearchCreatorsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Find creators.
Search semantically with `query`, or look them up by name, id, or
social handle. Either way, you can also narrow the results with
filters such as audience profile, location, and follower count.

Send one search field. Each field is its own mode:

- **Query** (`query`). A natural-language brief. Runs a language model and
  ranks by relevance, so `sortBy` is ignored. Bills at least one row even
  when nothing matches.
- **Name** (`names`). Fuzzy-matches account names (a case-insensitive contains
  match). Pass every name in one array.
- **ID** (`brandIds`). Looks up known ids. Missing ids are omitted and listed
  in `unmatchedIds`. `limit` and `offset` are ignored.
- **Instagram** (`igUsername`). Case-insensitive contains match on the
  handle. A leading `@` is ignored.
- **TikTok** (`tiktokUsername`). Same match on the TikTok handle.

If a request includes more than one of these fields, only the highest
priority field is used. The rest are ignored:

1. `brandIds`
2. `igUsername` or `tiktokUsername` (if both are set, either handle can match)
3. `names`
4. `query`

One exception: `query` together with `names` uses both. `names` narrows the
set, and `query` ranks inside it.

`limit` (default 20, max 100) caps the rows returned, and the cost, for
every mode except ID.

Limited to 120 requests per minute per user.
Costs 2,000 microdollars ($0.0020) per creator returned, charged against your plan's monthly credits. The count field is what you were charged for.
A `query` search runs a language model, so it bills a minimum of one
row even when nothing matches.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.searchCreators({
    igUsername: "charlidamelio",
    limit: 3,
    gender: "female"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.SearchCreatorsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Spyglass.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">searchTaxonomies</a>({ ...params }) -> spyglass.SearchTaxonomiesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Find taxonomy paths you can pass to POST /media as `taxonomy`.
`query` is a product or category phrase. Each path is a string
such as `Apparel > Footwear > Sneakers`.

Limited to 120 requests per minute per user.
This call is free and does not use credits.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.searchTaxonomies({
    query: "running shoes"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.SearchTaxonomiesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Spyglass.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">searchMedia</a>({ ...params }) -> spyglass.SearchMediaResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return creatives. Pass `brandId` to scope to one brand or creator,
or omit it to search the whole corpus. Without `brandId`,
`query` is required.

Limited to 120 requests per minute per user.
Costs 1,000 microdollars ($0.0010) per creative returned, charged against your plan's monthly credits. The count field is what you were charged for.
The call bills a minimum of one row even when nothing matches. In that
case `count` is 0 but you are charged for one row.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.searchMedia({
    query: "comfort-focused video ads",
    limit: 20
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.SearchMediaRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Spyglass.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">getCreditUsage</a>() -> spyglass.GetCreditUsageResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Credits left on the API key's team, the credits included in the
plan, and the current billing period. Granted credits count toward
`remainingCredits` and not `planCredits`.

Limited to 120 requests per minute per user.
This call is free and does not use credits. It still responds when
the team is out of credits.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.getCreditUsage();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `Spyglass.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">getComments</a>({ ...params }) -> spyglass.GetCommentsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return public comments for specific Instagram or TikTok posts.
Pass post URLs or the platform id. At least one of `instagramIds` or
`tiktokIds` is required (max 20 each). `limit` is comment pages
per post (default 1, max 20).

Instagram accepts a shortcode or a post URL:

- `C2xY9abcD1e`
- `https://www.instagram.com/p/C2xY9abcD1e/`
- `https://www.instagram.com/reel/C2xY9abcD1e/`

TikTok accepts a video id or a post URL:

- `7234567890123456789`
- `https://www.tiktok.com/@nike/video/7234567890123456789`

Recent posts may be scraped live; older posts with stored comments
are served from cache.

Limited to 20 requests per minute per user.
Costs 10,000 microdollars ($0.0100) per successful page of comments returned, charged against your plan's monthly credits.
Each comment page fetched live (`limit`, default 1) is one billed
unit. Cached posts do not scrape and are not billed.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.getComments({
    instagramIds: ["https://www.instagram.com/p/C2xY9abcD1e/"],
    tiktokIds: ["https://www.tiktok.com/@nike/video/7234567890123456789"],
    limit: 1
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.GetCommentsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `Spyglass.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Account
<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">get</a>({ ...params }) -> spyglass.GetAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Fetch a single brand or creator by id. The `type` field tells you
which one you got: `BRAND` or `CREATOR`, and the creator-only fields
(`gender`, `ageRange`, ...) are present only on `CREATOR`.

Limited to 120 requests per minute per user.
Costs 2,000 microdollars ($0.0020) per account returned, charged against your plan's monthly credits.
An id that matches nothing returns 404 and is free.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.account.get({
    id: "id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.GetAccountRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AccountClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">insights</a>({ ...params }) -> spyglass.InsightsAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return clustered creative themes for an account. `MEDIA_MIX` returns a
breakdown by media type; the other types return weekly buckets plus
ranked totals, highest first.

`maxDaysBack` filters both the weekly bucket series and ranked
`totals` to creatives that started within the requested window.

Limited to 120 requests per minute per user.
Costs 1,000 microdollars ($0.0010) per cluster returned, charged against your plan's monthly credits. The count field is what you were charged for.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.account.insights({
    id: "id",
    type: "HOOK"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.InsightsAccountRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AccountClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">landingPages</a>({ ...params }) -> spyglass.LandingPagesAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return the landing-page URLs an account sends ad traffic to, ranked by
how often they appear across its ads.

Limited to 120 requests per minute per user.
Costs 5,000 microdollars ($0.0050) per landing page returned, charged against your plan's monthly credits. The count field is what you were charged for.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.account.landingPages({
    id: "id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.LandingPagesAccountRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AccountClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">index</a>({ ...params }) -> spyglass.IndexAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a brand by website URL, or start indexing it if we do not
have it yet. Pass a single `websiteUrl`. Instagram and TikTok
profile URLs are accepted and resolved as creators.

This endpoint does not consume credits. An existing brand returns
`status: "already_exists"`. A new brand is created and sync is
queued, returning `status: "indexing"` and the new `brandId`.

Limited to 50 requests per minute per user.
This call is free.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.account.index({
    websiteUrl: "https://www.nike.com"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.IndexAccountRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AccountClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">indexStatus</a>({ ...params }) -> spyglass.IndexStatusAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Poll whether a brand finished indexing after POST /account/index.

Limited to 300 requests per minute per user.
This call is free.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.account.indexStatus({
    id: "id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `spyglass.IndexStatusAccountRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AccountClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

