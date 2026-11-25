# Korea Data MCP — 데이터와통찰

[한국어](README.md) | **English**

Remote [MCP](https://modelcontextprotocol.io) connectors that let an AI assistant query Korean public data: government procurement, defense procurement, local businesses, real-estate transactions, land parcels, official land prices, companies, corporate filings, trade statistics and startup support programs.

Operated by Data & Insight (㈜데이터와통찰) — <https://datawatongchal.com>

- **Nothing to install.** Remote servers over Streamable HTTP.
- **No sign-up, no API key.** Add the URL and start asking.
- **Read-only.** Every tool only looks data up.
- **Free** to use.

This repository is documentation only. It contains no server source code and does not redistribute any dataset — the connectors are a lookup service on top of data published by Korean public agencies, and every answer names its source.

## Quick start

Use the all-in-one connector unless you only need one field:

```
https://datawatongchal.com/mcp
```

**Claude (claude.ai / Claude Desktop)**
Settings → Connectors → Add custom connector → enter a name and the URL above. There is no login screen.

**Claude Code**

```bash
claude mcp add --transport http datawatongchal https://datawatongchal.com/mcp
```

**ChatGPT**
Custom servers can be added on the web (chatgpt.com) only, not in the mobile apps. The steps below follow OpenAI's guidance as of November 2026.

1. Go to [chatgpt.com/plugins](https://chatgpt.com/plugins) (Plugins).
2. Select the plus button → **Add custom MCP server**.
3. Enter a name (description optional) and, under **Connection**, the server URL `https://datawatongchal.com/mcp`.
4. For authentication choose **No authentication**.
5. Read the notice, tick **I understand and want to continue**, then select **Create as a plugin**.
6. In a new conversation, type `@` in the prompt box, pick the plugin you just created and ask your question.

According to OpenAI, custom MCP server connections are available on Business, Enterprise and Edu plans (a workspace may require admin permission), and Pro can connect read-only tools. Every tool in these connectors is read-only. Plans and menus change, so check the [OpenAI help article](https://help.openai.com/en/articles/12584461-developer-mode-apps-and-full-mcp-connectors-in-chatgpt-beta) as well.

**Other clients**
Any client that supports remote Streamable HTTP MCP servers should work with the URL alone. Check your client's documentation for the exact syntax.

If tools do not show up, refresh the tool list in the connector settings and start a new conversation.

## Connectors

One all-in-one connector and ten single-field connectors. Tool-name prefixes tell you the field.

| Field | URL | Tools | What it covers |
|---|---|---|---|
| **All-in-one** | `https://datawatongchal.com/mcp` | 44 | Everything below in a single connector. New fields are added automatically. |
| Procurement | `https://datawatongchal.com/mcp/g2b` | 8 | KONEPS (나라장터): order plans → pre-specifications → bid notices → awards → contracts |
| Defense procurement | `https://datawatongchal.com/mcp/d2b` | 3 | DAPA defense e-procurement (a separate system from KONEPS) |
| Local businesses | `https://datawatongchal.com/mcp/store` | 4 | About 2.7 million operating shops by administrative district and industry |
| Real estate | `https://datawatongchal.com/mcp/realestate` | 4 | About 24 million actual transactions since 2020, plus commercial rent and vacancy |
| Land (Pointology) | `https://datawatongchal.com/mcp/pointology` | 3 | Everything about one coordinate: land category, official land price, zoning, satellite observations |
| Land prices | `https://datawatongchal.com/mcp/landprice` | 3 | 2026 official individual land prices for about 35.9 million parcels nationwide: most and least expensive land, regional distribution, single parcels |
| Companies | `https://datawatongchal.com/mcp/corp` | 4 | About 550,000 workplaces from National Pension data, plus Data Voucher suppliers |
| Filings | `https://datawatongchal.com/mcp/dart` | 4 | DART disclosure filings and listed-company financials |
| Trade | `https://datawatongchal.com/mcp/trade` | 4 | Customs export/import statistics since 2015 by item (HS 10-digit), country and month |
| Startup support | `https://datawatongchal.com/mcp/startup` | 7 | Support program notices, incubator space, free courses, location data |

Counts are approximate, as of November 2026. An introduction page for each field is at `https://datawatongchal.com/connectors/{all|g2b|d2b|store|realestate|pointology|landprice|corp|dart|trade|startup}`.

## Tools

### Procurement — `g2b_`

| Tool | What it returns |
|---|---|
| `g2b_search_notices` | Bid notices: counts, monthly trend, work type, median estimated price, plus a list with deadlines |
| `g2b_prespecs` | Pre-specifications: allocated budget, comment deadline, software-project flag, spec document link |
| `g2b_order_plans` | Order plans: planned month, amount, contract method |
| `g2b_award_stats` | Awards: award-rate distribution, number of bidders, frequent winners |
| `g2b_contracts` | Contracts: amount, method, period, vendor shares, ordering agency |
| `g2b_breakdown` | Counts and amounts by group (month, field, agency, region, price band, or your own segments) |
| `g2b_bid_timeline` | The history of one project from order plan to contract |

### Defense procurement — `d2b_`

| Tool | What it returns |
|---|---|
| `d2b_search` | Search and summary of procurement plans, bid notices, open negotiated contracts, bid results and contracts |
| `d2b_vendors` | Vendor ranking and share by contract amount |
| `d2b_detail` | Every original field of one item, with its cancellation and change history |

### Local businesses — `store_`

| Tool | What it returns |
|---|---|
| `store_count` | Number of shops and rankings by industry, sub-region or administrative district |
| `store_trend` | Quarterly shop counts and shops newly listed or dropped |
| `store_brand` | Shop counts and regional spread for a brand, matched by shop name |
| `store_editions` | Available editions and the caveats of each |

### Real estate — `re_`

| Tool | What it returns |
|---|---|
| `re_stats` | Transaction counts, median price trend and regional rankings |
| `re_deals` | Individual transaction records |
| `re_complex` | One apartment complex: price by floor area, jeonse-to-price ratio, recent deals |
| `re_commercial` | Retail and office rent per ㎡, vacancy rate and investment return |

### Land (Pointology) — `point_`

| Tool | What it returns |
|---|---|
| `point_find` | Address or place name → coordinate candidates |
| `point_info` | For one coordinate: address, parcel, land cover (7-year change), land category, official land price, zoning, buildings, terrain, soil, vegetation (NDVI), radar, surface temperature, night lights, agricultural weather |
| `point_geo` | Twelve national spatial themes: administrative area, cadastre, zoning, regulations, buildings, roads, school districts, soil, geology, watershed, aviation restrictions, nearby facilities |

### Companies — `corp_`

| Tool | What it returns |
|---|---|
| `corp_search` | Companies matching conditions, ordered by change in subscribers |
| `corp_profile` | One company: monthly subscribers, joiners, leavers, estimated average reported income per person |
| `corp_changes` | Changes in a period: new, closed, industry, address or name changes, incorporation |
| `corp_stats` | Company counts by industry, region and size |

### Land prices — `lp_`

| Tool | What it tells you |
|---|---|
| `lp_rank` | The most and least expensive parcels (official price per m²) nationwide or in a region |
| `lp_stats` | A region's price distribution (min, percentiles, max, mean) and a ranking of the regions directly below it |
| `lp_parcel` | One parcel's official price and its rank within its neighborhood and district |

### Filings — `dart_`

| Tool | What it returns |
|---|---|
| `dart_filings` | Filing list with counts by type and links to the originals |
| `dart_company` | Company overview, recent financials, filings and National Pension headcount |
| `dart_financials` | Annual and quarterly trend of key accounts (full statements on request) |
| `dart_rank` | Listed-company rankings by account value or growth rate |

### Trade — `trade_`

| Tool | What it returns |
|---|---|
| `trade_items` | Item name → HS code candidates |
| `trade_trend` | Monthly and yearly trend by item and country, with year-over-year change |
| `trade_flash` | Preliminary export figures for the latest 10-day periods |
| `trade_rank` | Item and country rankings |

### Startup support — `startup_`

| Tool | What it returns |
|---|---|
| `startup_search` | Search support program notices |
| `startup_match` | Notices open now that fit a company's region and age |
| `startup_cert` | Look up a startup confirmation certificate by business registration number |
| `startup_space` | Incubator rooms and rent |
| `startup_edu` | Free startup courses |
| `startup_notice` | Full text of one notice |
| `startup_location` | Location data for a region: rent, vacancy, key money, shop-count trend |

### Shared

| Tool | What it does |
|---|---|
| `request_data` | Sends a request for data that is not available yet. Works only when the user agrees and provides an email address. Available on the all-in-one and procurement connectors. |

## Data sources and freshness

| Field | Source | Coverage | Updated |
|---|---|---|---|
| Procurement | Public Procurement Service, KONEPS (data.go.kr OpenAPI) | 2023-01-01 – today | Notices hourly; pre-specifications every 3 h; awards and order plans every 6 h; contracts daily |
| Defense procurement | DAPA defense e-procurement (data.go.kr) | 2023-01 – | Daily |
| Local businesses | Small Enterprise and Market Service, shop information (data.go.kr) | 2023-03 – | One edition per quarter |
| Real estate | Ministry of Land, Infrastructure and Transport, actual transaction prices (data.go.kr OpenAPI); Korea Real Estate Board R-ONE commercial rental survey | 2020-01 – | Daily (last 3 months re-fetched for late filings and cancellations); rental survey quarterly |
| Land | Data & Insight's own Sentinel-1/Sentinel-2 archive; MOLIT V-World; Ministry of Environment land cover map; MAFRA Farm Map; others | Korean Peninsula; NDVI since 2019 | Each value carries its own source and date |
| Land prices | MOLIT official individual land prices (V-World spatial data download) | 2026 (as of January 1) | Yearly |
| Companies | National Pension Service workplace data; K-Data Data Voucher supplier list (data.go.kr) | Monthly editions from 2026-01 | Monthly |
| Filings | Financial Supervisory Service, DART OpenAPI | Filings: last 3 years; financials: 2015 – (incl. quarterly) | Daily |
| Trade | Korea Customs Service trade statistics (data.go.kr) | 2015 – ; about 245 countries; USD; 10-day flash since 2016 | Monthly |
| Startup support | KISED K-Startup, startup space platform and startup education; MSS Bizinfo; SME public purchasing portal | Current notices; about 420 spaces and 340 courses | Daily |

## Things to know before you trust a number

The connectors return organized data and notes on how to read it. The judgment is left to you and your AI.

- **Procurement** — Anything before 2023 is *outside the coverage period*, not "nothing happened". 1–5 January 2025 has no notices, awards or pre-specifications in the original source because of the KONEPS system migration. Compare award rates within the same contract method. Unit-price contracts for "each ordering agency" are a shared ceiling and should not be summed.
- **Defense procurement** — A separate system from KONEPS. Cancelled notices and amended contracts are folded into one project and counted once.
- **Local businesses** — "New" and "gone" mean shops that entered or left the list, not actual openings and closures; the jump in December 2024 reflects wider collection. Brand counts are matched by shop name and are not official store counts.
- **Real estate** — Prices are transaction medians, not averages or an index, so they can move against the market when the mix of deals changes. Cancelled deals are excluded. The latest one to two months look low because filings are allowed up to 30 days.
- **Companies** — Average income per person is an estimate derived from National Pension contributions. Linking a company across months relies on partial registration numbers, names and addresses and is not perfect.
- **Land prices** — Official land prices are set by the government for taxes and charges. They are not market prices and are usually lower. The cheapest parcels are usually roads and forest land in remote mountains.
- **Filings** — Filings are written by the filers; the Financial Supervisory Service does not guarantee their accuracy.
- **Trade** — Amounts are in USD. 10-day flash figures are preliminary.
- **Everything** — For investment or business decisions, check the original source and the situation on the ground. The connectors do not give buy or sell opinions.

## Example questions

Questions work best in Korean, since place names and agency names are Korean. More in [EXAMPLES.md](EXAMPLES.md).

- "올해 나라장터에서 가장 많이 수주한 기업 순위" — top vendors on KONEPS this year
- "카페가 가장 많은 행정동" — the district with the most cafés
- "은마아파트 84㎡ 시세와 전세가율" — price and jeonse ratio for one apartment complex
- "최근 2년 반도체(8542) 월별 수출과 나라별 비중" — monthly semiconductor exports and country shares
- "성수동 아파트값, 새로 생긴 가게, 구청 발주 사업을 한 번에 정리해줘" — home prices, new shops and public projects for one neighborhood, across three fields

## Privacy

Lookup tools do not ask for anything that identifies you, and your conversation with the assistant is not sent to the server. See the [privacy policy](https://datawatongchal.com/mcp/privacy).

## Links and contact

- Website: <https://datawatongchal.com>
- Guide: <https://datawatongchal.com/mcp/docs>
- Summary for AI: <https://datawatongchal.com/llms.txt>
- Questions, bug reports and security reports: <dkc@datawatongchal.com>, or open an issue in this repository
