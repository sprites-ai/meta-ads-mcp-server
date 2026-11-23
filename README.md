# Meta Ads MCP Server

Connect Facebook and Instagram Ads to Claude, ChatGPT, Cursor and any MCP client. Read ROAS and creative fatigue, draft new ads, and launch or change campaigns, with every change approved by you first.

![License: MIT](https://img.shields.io/badge/license-MIT-blue) ![MCP: remote](https://img.shields.io/badge/MCP-remote-6E56CF) ![Transport: streamable HTTP](https://img.shields.io/badge/transport-streamable%20HTTP-informational) ![Auth: OAuth](https://img.shields.io/badge/auth-OAuth-success) ![Hosted by Sprites](https://img.shields.io/badge/hosted%20by-Sprites-111111)

**Server URL:** `https://mcp.sprites.ai/mcp` · streamable HTTP · OAuth sign-in · 6 read tools · 14 write tools · every write approved by you

> This repository holds the documentation, client configs and MCP Registry metadata for the hosted
> Meta Ads MCP server run by [Sprites](https://www.sprites.ai). The server itself is hosted by Sprites and its source is not
> in this repository. There is nothing to install or run.

## What's new

| Date | Change |
|---|---|
| 2026-11-23 | Initial public release of connector docs and configs |

## Works with

- **Claude**: claude.ai and Claude Desktop, custom connector (renders approval cards)
- **Claude Code**: `claude mcp add`
- **ChatGPT**: developer mode app (Plus, Pro, Business, Enterprise, Edu) (renders approval cards)
- **Codex**: `codex mcp add` or `config.toml`
- **Cursor**: `mcp.json`
- **Gemini CLI**: `settings.json`
- **GitHub Copilot (VS Code)**: `.vscode/mcp.json`, agent mode
- **Google Antigravity**: `mcp_config.json`
- **Windsurf**: `mcp_config.json`
- **Lovable**: Connectors, Custom MCP
- **Grok**: grok.com/connectors, Custom
- **Cline**: Remote Servers, Streamable HTTP
- **Devin**: Customize, MCPs
- **Replit Agent**: Integrations, MCP Servers

Any other client that speaks remote MCP over streamable HTTP with OAuth should work with the same URL.

## Quick start

1. **Connect Meta Ads in Sprites.** Sign up at [sprites.ai](https://www.sprites.ai) and connect Meta Ads once in the Sprites integrations menu. Plans start at $29 a month with a 7-day trial for $1. There is no free plan.
2. **Add the server to your client** with the URL `https://mcp.sprites.ai/mcp` (one endpoint for every client and every channel).
3. **Sign in.** Your client opens the Sprites consent screen. Sign in with the account your channels are connected to and approve.
4. **Ask.** Try: "With Sprites, which Meta creatives are fatiguing and what should replace them?"

## Setup by client

### Claude (claude.ai and Claude Desktop)

Works on Free (one custom connector), Pro, Max, Team and Enterprise.

1. Settings, then Customize, then Connectors. On Team and Enterprise an Owner adds it once under Organization settings, Connectors.
2. Click **+**, then **Add custom connector**. Name it `Sprites` and paste `https://mcp.sprites.ai/mcp`. Leave Advanced settings empty.
3. Click **Connect**, sign in to Sprites and approve.

Claude draws the Sprites approval card in the chat, so you can edit a budget or a headline in place and approve it.

### Claude Code

```bash
claude mcp add --transport http sprites https://mcp.sprites.ai/mcp
```

Then type `/mcp` inside Claude Code to finish the browser sign-in.

### ChatGPT

Plus, Pro, Business, Enterprise and Edu, on the web app. Not available on Free.

1. Settings, Security and login: turn on **Developer mode**. On Business and Enterprise an admin has to allow it first.
2. Settings, **Plugins** (formerly Connectors), then **+** to create an app.
3. Name `Sprites`, MCP server URL `https://mcp.sprites.ai/mcp`, Authentication **OAuth**. Tick the trust box and click Create.
4. Connect the app and sign in to Sprites.

### Cursor

`~/.cursor/mcp.json` (every project) or `.cursor/mcp.json` (one repo):

```json
{
  "mcpServers": {
    "sprites": {
      "url": "https://mcp.sprites.ai/mcp"
    }
  }
}
```

Cursor discovers the Sprites authorization server from the endpoint and opens a browser to sign in.

### Codex

```bash
codex mcp add sprites --url https://mcp.sprites.ai/mcp
codex mcp login sprites
```

Or by hand in `~/.codex/config.toml`:

```toml
[mcp_servers.sprites]
url = "https://mcp.sprites.ai/mcp"
auth = "oauth"
```

### VS Code and GitHub Copilot

`.vscode/mcp.json`. The top-level key is `servers`, not `mcpServers`, and a remote server needs `"type": "http"`:

```json
{
  "servers": {
    "sprites": {
      "type": "http",
      "url": "https://mcp.sprites.ai/mcp"
    }
  }
}
```

Run **Auth** above the entry, sign in, then switch Copilot Chat to **Agent** mode. Ask mode does not call MCP tools.

### Gemini CLI

`~/.gemini/settings.json`. Use `httpUrl`; a `url` entry is treated as SSE:

```json
{
  "mcpServers": {
    "sprites": {
      "httpUrl": "https://mcp.sprites.ai/mcp"
    }
  }
}
```

Restart the CLI; the first call opens the sign-in. Check with `gemini mcp list`.

### Other clients

| Client | How |
|---|---|
| Windsurf | `~/.codeium/windsurf/mcp_config.json` with `"serverUrl": "https://mcp.sprites.ai/mcp"` |
| Google Antigravity | `~/.gemini/config/mcp_config.json` with `"serverUrl": "https://mcp.sprites.ai/mcp"` (`url` and `httpUrl` are skipped) |
| Cline | Remote Servers tab, transport Streamable HTTP; in the file, `"type": "streamableHttp"` |
| Lovable | Connectors, Custom MCP, paste the URL |
| Grok | grok.com/connectors, New Connector, Custom (paid tiers) |
| Devin | Customize, MCPs, Add custom MCP, transport HTTP, authentication OAuth |
| Replit Agent | Integrations, MCP Servers, Add MCP server, Test & Save |

Copy-ready files for each are in [`configs/`](configs/).

## What it can read

- Campaigns, ad sets, ads, audiences and their status
- Spend, ROAS, CPA, CTR, frequency and placement breakdowns
- Creative assets and per-creative performance
- Learning-phase status and delivery issues per ad set
- Public competitor ads from the Meta Ad Library

## What it can change (with your approval)

- Create campaigns, ad sets, audiences and ads
- Budgets, schedules, statuses and targeting
- New creative, previewed in placement before anything ships
- New campaigns are created paused

Every change arrives as an approval with the before and after. In Claude and ChatGPT it is a card you can edit
in place. In clients without cards, Sprites returns the change written out with a gate id, and applies it only
after you confirm (`approve_change`). Nothing is applied by the model on its own.

## Permissions

- **OAuth.** You connect Meta once inside Sprites with Facebook Login for Business. The Sprites Meta app asks for `ads_read`, `ads_management` and `business_management`, plus `pages_show_list`, `pages_manage_ads` and `pages_read_engagement` so ads can run from your Page.
- **No developer app.** You do not create a Meta developer app, request App Review or paste a system-user token. Sprites holds the connection.
- **Account access.** Sprites sees the ad accounts your Facebook login can manage. For a client account, have their Business add yours as a partner first.
- **Rate limits.** Calls are paced per ad account, so a busy agent does not spend the account's Marketing API quota in one go (Meta error `(#17) User request limit reached`).
- **Your AI client never holds a Meta credential.** It holds a Sprites OAuth token bound to `https://mcp.sprites.ai/mcp`.

## Tools

The server exposes 20 tools to the model: 6 read tools and 14 write tools. Write tools that touch an ad account or a site only propose; nothing reaches either until you approve it. The tools that do the Meta Ads work:

| Tool | What it does on Meta Ads |
|---|---|
| `analyze` | ROAS, CPA, frequency and creative-level performance, with the period comparison. |
| `create_creative` | New hooks, headlines and primary text, and written image or video concepts. |
| `optimize` | Fatigued creatives, ad sets stuck in learning, budget reallocation. |
| `launch` | Campaign, ad sets, audiences and ads as one editable proposal. |
| `research` | What competitors are running right now, from the Meta Ad Library. |
| `change` | A budget, a status, an audience, one card at a time. |
| `schedule_task` | A weekly Meta review or a daily fatigue check that runs inside Sprites. |

Meta Ads requests use `channel: meta_ads`. Name another channel, or `all`, to compare.

Every task tool takes the same input: `goal` (the request written to stand on its own: Sprites sees only this
string, not the chat), `title` (a short label), `channel`, and `today` (the user's local date, so "last 30 days"
resolves correctly). Long tasks return a run id; `task_status` collects the result and `cancel_task` stops it.

<details>
<summary>Full tool list (20 tools, plus <code>approve_change</code> on clients without cards)</summary>

| Tool | Type | What it does |
|---|---|---|
| `analyze` | read | Performance, reports, spend, revenue and Shopify sales questions, period comparisons, diagnosing a drop. Reads only. |
| `research` | read | Competitors and their ads, keyword research (paid or SEO), market and audience research, AI visibility. |
| `optimize` | write | Audits, wasted spend, scaling winners, moving budget. Reads, reports, and queues the recommended changes as approval cards. |
| `create_creative` | write | Ad copy, headlines and descriptions, and written concepts for image or video ads. Writes creative; does not generate images, video or audio. |
| `launch` | write | New campaigns, ad sets or ad groups, audiences, retargeting, duplicating a campaign, A/B tests, proposed as approval cards. |
| `change` | write | One named edit: budget, pause or enable, keywords or negatives, targeting, rename. Opens one approval card. |
| `schedule_task` | write | Recurring work that runs inside Sprites: weekly briefs, monitors, alerts, recurring content. |
| `write_seo_blog_post` | write | A researched blog post for your site, drafted in your CMS and published once approved. |
| `create_seo_page` | write | Create or edit a landing or service page, meta titles and descriptions, schema; a diff card before publishing. |
| `create_aeo_content` | write | Content meant to be cited by ChatGPT, Perplexity, Gemini or AI Overviews: answer, FAQ and comparison pages, llms.txt. |
| `run_task` | write | Anything else: setup, capability questions, cross-channel and multi-step jobs. |
| `linkedin_ads_report` | read | LinkedIn Ads performance: spend, leads, CPL, CTR, reach, period comparisons. |
| `linkedin_forecast` | read | LinkedIn's own forecast for up to 5 audiences: members, cost per lead, CPC and CTR over 30 days. Creates nothing. |
| `linkedin_ads_campaign` | write | Create a LinkedIn campaign: Sponsored Content, lead gen forms, message ads, retargeting. |
| `linkedin_abm_campaign` | write | LinkedIn account-based marketing: target account lists, company matched audiences. |
| `linkedin_ads` | write | Any other LinkedIn Ads task, routed by intent (audit, budget, pause, targeting, creative). |
| `account_status` | read | Which channels are connected in Sprites and how many credits are left. |
| `choose_business` | write | Select which Sprites business the tools work on, by name, or list your businesses. |
| `task_status` | read | Status and result of a Sprites task by its run id. |
| `cancel_task` | write | Stop a running task. |
| `approve_change` | write | Only on clients that do not render the approval card (Codex, Cursor, Claude Code and others): apply or decline a pending change by its gate id, after you confirm it. |

</details>

## Example prompts

1. With Sprites, which Meta creatives are fatiguing and what should replace them?
2. Ask Sprites for 5 new hooks for our best-performing Instagram ad.
3. Use Sprites to compare Meta ROAS this month against last month by ad set.
4. Have Sprites check which of our Meta ad sets are stuck in learning.
5. With Sprites, show me what our top 3 competitors are running on Meta right now.
6. Use Sprites to launch a Meta campaign for our fall sale at $80 a day.
7. Ask Sprites to move $50/day from the weakest prospecting ad set to the best one.
8. With Sprites, pause every Meta ad with frequency over 5 and CTR under 0.5% this week.
9. Have Sprites draft Reels ad concepts from our three best-selling products.
10. Use Sprites to send me a Meta performance summary every Monday morning.

More in [`examples/prompts.md`](examples/prompts.md). Saying "with Sprites" or "use Sprites" helps clients that
rank their own built-in tools first (Cursor, Grok).

## Official server vs this one

**Meta Ads MCP server (hosted by Meta).** Meta hosts its own Ads MCP server as part of its ads AI connectors. You add it as a custom connector and sign in with your Facebook Business login. It covers reporting, creating and editing campaigns, ad sets and ads, catalogs, datasets, A/B tests and lift studies, on Meta only. New campaigns are created paused.

| | Official / self-hosted | Sprites (this server) |
|---|---|---|
| Endpoint | Hosted by Meta | `https://mcp.sprites.ai/mcp` |
| Channels | Facebook and Instagram | Meta plus Google Ads, LinkedIn, TikTok, Reddit, Microsoft, GA4, SEO and Shopify |
| Writes | Campaigns, ad sets and ads created paused; activation confirmed in your AI client | Proposed as an editable card with before and after; applied only on approve |
| New creative | Builds ads from creative you already have | Drafts hooks, headlines and concepts |
| Competitor ads | Your own ad accounts only | Live ads from any advertiser in the Meta Ad Library |
| Catalogs, datasets, lift studies | Yes, its strongest ground | Not the focus |

If you only run Meta, or you manage catalogs and lift studies, Meta's own server may be enough. Use Sprites when you run more than Meta, want new creative drafted, want competitor ads in the same thread, or need every write staged as an approval your team can see.

## FAQ

**Is this a server I have to host?**
No. It is a hosted remote MCP server. Paste `https://mcp.sprites.ai/mcp` into your client and sign in.

**Is this a Facebook Ads MCP server?**
Yes. Facebook and Instagram ads both run on a Meta ad account, so this covers both, with placements read and set per ad set.

**Do I need a Meta developer app or a system-user token?**
No. You connect Meta once in Sprites with OAuth. Your AI client never sees a Meta token.

**Will it publish an ad without me seeing it?**
No. Creative comes back as an editable preview and ships only when you approve it. New campaigns start paused.

**What happens if the Meta connection expires?**
The task parks instead of failing. Sprites tells your agent the channel needs reconnecting, and the same task continues once you reconnect.

**Will Sprites push me to spend more on Meta?**
Sprites is priced per plan, not as a share of ad spend, and reads your other channels too, so it can just as well propose cutting a Meta budget.

**Does it cover other channels?**
Yes. The same URL reaches Google Ads, Meta Ads, LinkedIn Ads, TikTok Ads, Reddit Ads, Microsoft Advertising,
Apple Ads, GA4, Search Console, Shopify, SEO and AI visibility, and Google Business Profile, for whatever is
connected in Sprites. See [ads-mcp-servers](https://github.com/sprites-ai/ads-mcp-servers).

**How much does it cost?**
The connector is part of your Sprites plan: Build is $29 to $99 a month, Launch is $300 to $500 a month, and
larger companies get a custom plan. Every plan starts with a 7-day trial for $1. Meta Ads charges its own media
cost as usual.

**The client says it has no tools, or every call is unauthorized.**
Remove the server, re-add it with exactly `https://mcp.sprites.ai/mcp`, and sign in again. Grants are bound to that one URL.

**How do I revoke access?**
Disconnect the connector in your client, or remove the connection in Sprites. The next tool call fails closed.

## Maintained by Sprites

This server is built and run by [Sprites](https://www.sprites.ai), an AI marketing agent for paid ads, SEO and AI visibility.

- Product page: [sprites.ai/mcp/meta-ads](https://www.sprites.ai/mcp/meta-ads)
- Setup guide: [sprites.ai/meta-ads-mcp](https://www.sprites.ai/meta-ads-mcp)
- All Sprites MCP servers: [github.com/sprites-ai](https://github.com/sprites-ai)
- Issues with these docs or configs: open an issue here. Security reports: see [SECURITY.md](SECURITY.md).

Licensed under the [MIT License](LICENSE). The license covers the documentation and configuration files in this repository.
