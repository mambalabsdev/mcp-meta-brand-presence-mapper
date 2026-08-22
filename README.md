# Instagram Threads Facebook Brand Presence Mapper MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-meta-brand-presence-mapper)](https://smithery.ai/servers/mambabuilt/mcp-meta-brand-presence-mapper) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-meta-brand-presence-mapper/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-meta-brand-presence-mapper) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-meta-brand-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-meta-brand-presence-mapper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-meta-brand-presence-mapper)](https://www.npmjs.com/package/@mambalabsdev/mcp-meta-brand-presence-mapper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-meta-brand-presence-mapper)](https://github.com/mambalabsdev/mcp-meta-brand-presence-mapper/blob/main/LICENSE)

MCP server for the Mamba Labs **Instagram Threads Facebook Brand Presence Mapper** actor on Apify.

Resolve a company domain to its Instagram, Threads and Facebook accounts with follower and post counts.

## What it does

Resolve a company domain to its Instagram, Threads and Facebook accounts with follower and post counts, as one flat Clay ready row. The Threads handle is derived from the resolved Instagram handle at no extra discovery cost. Instagram and Threads counts are rounded by Meta and the row carries both the rounded integer and the platform's own display string. Facebook is best effort: Meta serves a login wall to anonymous clients, so blocked is a normal answer there and never a zero. Read only; requires an APIFY_TOKEN and consumes Apify credits per call.

## Quick start

Add this to your MCP client configuration:

```json
{
  "mcpServers": {
    "mamba-meta-brand-presence-mapper": {
      "command": "npx",
      "args": ["-y", "@mambalabsdev/mcp-meta-brand-presence-mapper"],
      "env": { "APIFY_TOKEN": "your-apify-token" }
    }
  }
}
```

## Prerequisites

- Node.js 18 or newer
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations)

The actor is pay per event and consumes Apify credits per call. Pricing is on the
[actor page](https://apify.com/mambalabs/meta-brand-presence-mapper).

## Example prompts

- "Get the Instagram and Threads follower counts for glossier.com."
- "Has oatly.com adopted Threads yet, and how many posts do they have?"
- "Find the Facebook Page for everlane.com."

## Tool and inputs

Tool: `map_meta_brand_presence`

| Input | Type | Meaning |
|---|---|---|
| `company_domain` | string | Bare company domain, for example shopify.com. Supply this or a handle. With a domain the actor runs full discovery; with a handle it skips straight to |
| `company_name` | string | Optional. Improves search accuracy and is what the identity gate checks a discovered profile against, so supplying it reduces wrong matches. |
| `handle` | string | Optional. The Instagram handle with or without the leading @. Supplying it skips Instagram discovery AND gives Threads its handle for free, because Th |
| `platforms` | array | Which of the three Meta surfaces to map. Default is all three. Dropping Facebook is the common choice: it is the least reliable of the three and it co |
| `includeFollowerCounts` | boolean | When "true" (default) the profile page is fetched and the counts are extracted. Set "false" to resolve the profile URL only, which is cheaper and need |
| `skipCache` | boolean | When "false" (default) a successful lookup is cached for seven days and reused. Set "true" to force a fresh fetch. Sent as a string for Clay compatibi |

## Reading the output

Every row carries a per platform `_status` field, and it is the field to read
first. The vocabulary is the same across the whole Mamba Labs social family:

| Status | Meaning |
|---|---|
| `ok` | fetched and parsed, the value is there |
| `not_found` | we looked and there is no such profile |
| `not_extractable` | the profile exists and the value is not on the wire to us |
| `blocked` | the platform refused us, worth retrying later |
| `identity_mismatch` | we found a real profile and it belongs to someone else |
| `skipped` | you did not ask for this platform |

**`false` and `null` are never interchangeable.** `false` means we looked and the
answer is no. `null` means we could not look. If you filter for companies with no
presence, filter on `false`, because `null` rows are unknown rather than absent.

## Full actor documentation

[apify.com/mambalabs/meta-brand-presence-mapper](https://apify.com/mambalabs/meta-brand-presence-mapper)

## Mamba Labs GTM Suite

Mamba Labs builds a fleet of GTM enrichment actors that share one flat, Clay
ready output convention, so their rows join on `company_domain` with no cleaning
step. Full fleet: [apify.com/mambalabs](https://apify.com/mambalabs)

## License

MIT
