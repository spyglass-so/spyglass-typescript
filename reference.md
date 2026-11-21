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
Filter by taxonomy, location, or followers.

Leave the search fields empty to list by `sortBy` (default `MOST_FOLLOWERS`).
Or pass one of:

- `query` — semantic search. Ordered by relevance, so `sortBy` is ignored.
  Bills at least one row even when nothing matches.
- `names` — name contains this text.
- `ids` — UUID, handle, domain, or Ad Library id. Misses are in
  `unmatchedIds`. Ignores `limit` and `offset`.
- `igUsername` or `tiktokUsername` — handle contains this text. A leading
  `@` is ignored.

When more than one is set:

`ids` > `igUsername` / `tiktokUsername` > `names` > `query`

`names` + `query` uses both.

`limit` is 20 by default, 100 max, and caps the charge except for `ids`.

Limited to 120 requests per minute per user.
Costs $0.0010 per brand returned, charged against your plan's monthly credits. The count field is what you were charged for.
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
Filter by category, audience, location, or followers.

Leave the search fields empty to list by `sortBy` (default `MOST_FOLLOWERS`).
Or pass one of:

- `query` — semantic search. Ordered by relevance, so `sortBy` is ignored.
  Bills at least one row even when nothing matches.
- `names` — name contains this text.
- `ids` — UUID, handle, domain, or Ad Library id. Misses are in
  `unmatchedIds`. Ignores `limit` and `offset`.
- `igUsername` or `tiktokUsername` — handle contains this text. A leading
  `@` is ignored.

When more than one is set:

`ids` > `igUsername` / `tiktokUsername` > `names` > `query`

`names` + `query` uses both.

`limit` is 20 by default, 100 max, and caps the charge except for `ids`.

Limited to 120 requests per minute per user.
Costs $0.0010 per creator returned, charged against your plan's monthly credits. The count field is what you were charged for.
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
This call is free and does not consume credits.
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

Return media posts. A call usually takes under 10 seconds.

**1. Pick what to search**

| You want | Send |
| --- | --- |
| All media | Send neither `accountId` nor `query` to list media across the index, ordered by `sortBy` |
| One account's media | `accountId` |
| Media matching a description | `query`, which matches posts by what they show or say |
| One account's media matching a description | `accountId` and `query` |
| A broader category | `taxonomy`, a path from GET /taxonomies |

**2. Pick a sort order**

`sortBy` defaults to `MOST_RECENT`. Some orders only work with certain channels:

| `sortBy` | Allowed `channel` |
| --- | --- |
| `MOST_RECENT`, `OLDEST` | Any |
| `MOST_IMPRESSIONS`, `LONGEST_RUN` | Exactly `META_AD` |
| `MOST_VIEWS`, `MOST_LIKES`, `MOST_COMMENTS`, `HIGHEST_ENGAGEMENT`, `HIGHEST_OUTLIER` | Anything except `META_AD` |

**3. Optionally add a transcript or caption**

Send `includeTranscript: true` to get each video transcript.
Send `includeCaption: true` to get each post caption.
Both default to false and do not change the price.

**Cost and limits**

Costs $0.0010 per creative returned, charged against your plan's monthly credits. The count field is what you were charged for.
You are always charged for at least one row. If nothing matches, `count` is 0 but you pay for one row.
Limited to 120 requests per minute per user.
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

Limited to 60 requests per minute per user.
This call is free and does not consume credits. It still responds when
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
Costs $0.0050 per successful page of comments returned, charged against your plan's monthly credits.
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

<details><summary><code>client.<a href="/src/Client.ts">listAgentRuns</a>({ ...params }) -> spyglass.ListAgentRunsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists runs for this team, newest first.
Page with `offset`. The next page is `offset` plus the `limit` you sent.
`count` is the number of runs in this response. Stop when `count` is less than `limit`.

Limited to 120 requests per minute per user.
This call is free and does not consume credits.
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
await client.listAgentRuns();

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

**request:** `spyglass.ListAgentRunsRequest` 
    
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

<details><summary><code>client.<a href="/src/Client.ts">createAgentRun</a>({ ...params }) -> spyglass.CreateAgentRunResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Start a Spyglass agent run. The run id is the chat thread id.
Pass `outputSchema` to get that JSON on `structured` when the run completes.
The root `type` must be `"object"`. Each property can be a string, number, integer, boolean, array, or object.
`model` is `default` or `pro`.
Poll GET /agent/runs/{id} until `status` is `completed` or `failed`.

Limited to 20 requests per minute per user.
The create call is free. The run is billed from model usage when it finishes.
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
await client.createAgentRun({
    query: "query"
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

**request:** `spyglass.CreateAgentRunRequest` 
    
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

<details><summary><code>client.<a href="/src/Client.ts">getAgentRun</a>({ ...params }) -> Record&lt;string, unknown&gt;</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Poll a run until `status` is `completed` or `failed`.
Completed runs include `text` and, when you passed a schema, `structured`.
Failed runs include `error` with a `code` and `message`.

Limited to 120 requests per minute per user.
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
await client.getAgentRun({
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

**request:** `spyglass.GetAgentRunRequest` 
    
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

Fetch a single brand or creator. `id` is any one of:

- an account id (a UUID, or the numeric id from POST /brands or POST /creators)
- a website URL or bare domain, such as `https://www.nike.com` or `nike.com`
- an Instagram handle, such as `nike` or `@nike`
- a TikTok handle, such as `nike` or `@nike`

A website matches the account domain. A handle matches `igUsername` or
`tiktokUsername`. When both match, the `igUsername` match is returned.
Encode `/` and `?` in a URL. The `type` field tells you which
one you got: `BRAND` or `CREATOR`, and the creator-only fields
(`gender`, `ageRange`, ...) are present only on `CREATOR`.

Limited to 120 requests per minute per user.
Costs $0.0010 per account returned, charged against your plan's monthly credits.
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
    id: "e9cf96ee-7455-4b00-b692-80e2f923f79d"
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

Return creative insights for an account. `MEDIA_MIX` counts creatives by
media type, `CATEGORY` by creative category, and `FUNNEL` by funnel
stage. `HOOK`, `TARGET_PERSONA`, and `AD_ANGLE` return ranked totals
(`id`, `label`, `tags`, `total`), highest first.

If a hook, angle, or persona clustering run is missing, older than 30
days, or has no clusters, clustering starts and the response includes
`status: "analyzing"` with empty totals. Retry for the finished clusters.
Analyzing responses are not charged.

`datePreset`, or `startDate` and `endDate` as `YYYY-MM-DD`, filter
by creative start date. Omit both for the last 30 days.

Limited to 120 requests per minute per user.
Costs $0.0010 per cluster returned, charged against your plan's monthly credits. The count field is what you were charged for.
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
    id: "e9cf96ee-7455-4b00-b692-80e2f923f79d",
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

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">campaigns</a>({ ...params }) -> spyglass.CampaignsAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return an account’s campaigns or offers, newest start date first.
`datePreset`, or `startDate` and `endDate` as `YYYY-MM-DD`, filter
by campaign start date. Omit both for all time.

Limited to 120 requests per minute per user.
Costs $0.0010 per campaign returned, charged against your plan's monthly credits. The count field is what you were charged for.
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
await client.account.campaigns({
    id: "e9cf96ee-7455-4b00-b692-80e2f923f79d",
    type: "CAMPAIGN"
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

**request:** `spyglass.CampaignsAccountRequest` 
    
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

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">collabs</a>({ ...params }) -> spyglass.CollabsAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return the creators an account has collaborated with, ordered from
most media to least. Works for both brands and creators.
mediaCount is how many of that account’s media items tag the
creator. startedAt is the earliest of those media start times, and
lastPostedAt is the latest. `datePreset`, or `startDate` and
`endDate` as `YYYY-MM-DD`, filter by startedAt. Omit both for all time.

Limited to 120 requests per minute per user.
Costs $0.0010 per collab returned, charged against your plan's monthly credits. The count field is what you were charged for.
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
await client.account.collabs({
    id: "e9cf96ee-7455-4b00-b692-80e2f923f79d"
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

**request:** `spyglass.CollabsAccountRequest` 
    
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
Costs $0.0010 per landing page returned, charged against your plan's monthly credits. The count field is what you were charged for.
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
    id: "e9cf96ee-7455-4b00-b692-80e2f923f79d"
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

<details><summary><code>client.account.<a href="/src/api/resources/account/client/Client.ts">posts</a>({ ...params }) -> spyglass.PostsAccountResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Return one account's posts, fetched live. The account does not need to be indexed.
A call usually takes 10–30 seconds and starts indexing the rest of the brand's posts.
Use `POST /media` first when you already have an account id. See the Finding media guide.

Limited to 120 requests per minute per user.
Costs $0.0010 per creative returned, charged against your plan's monthly credits. The count field is what you were charged for.
The call bills a minimum of one row even when nothing matches. In that
case `count` is 0 but you are charged for one row.

Send exactly one identifier. The fields you can send depend on which
one you pick.

**`igUsername`**

| Field | Details |
| --- | --- |
| `igUsername` | Instagram handle. A leading `@` is ignored. |
| `tagged` | Optional. `true` returns posts the account is tagged in. Omit it for the account's own posts. Cannot be combined with `postType=reels`. |
| `postType` | Optional, Instagram only. `posts` (default) or `reels`. Cannot be combined with `tagged`. |
| `includeTranscript` | Optional. `true` transcribes each video and returns `transcript`. Omit it or send `false` to skip transcription. The call takes longer. |
| `limit` | Optional. Default 20, max 120. |
| `sortBy` | Ignored. Posts are always most recent. |

**`tiktokUsername`**

| Field | Details |
| --- | --- |
| `tiktokUsername` | TikTok handle. A leading `@` is ignored. |
| `includeTranscript` | Optional. `true` transcribes each video and returns `transcript`. Omit it or send `false` to skip transcription. The call takes longer. |
| `sortBy` | Optional. `most_recent` (default) or `popular`. |
| `limit` | Optional. Default 20, max 120. |

**`facebookAdLibraryId`**

| Field | Details |
| --- | --- |
| `facebookAdLibraryId` | Facebook Ad Library page id. |
| `includeTranscript` | Optional. `true` transcribes each video and returns `transcript`. Omit it or send `false` to skip transcription. The call takes longer. |
| `sortBy` | Optional. `most_recent` (default) or `most_impressions`. |
| `limit` | Optional. Default 20, max 120. |
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
await client.account.posts({
    igUsername: "nike",
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

**request:** `spyglass.PostsAccountRequest` 
    
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

An existing brand returns
`status: "already_exists"`. A new brand is created and sync is
queued, returning `status: "indexing"` and the new `brandId`.

Limited to 50 requests per minute per user.
This call is free and does not consume credits.
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
This call is free and does not consume credits.
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
    id: "e9cf96ee-7455-4b00-b692-80e2f923f79d"
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

