# Custom SEO Dashboard for ChatGPT

**A custom SEO dashboard that runs inside ChatGPT, built as a plugin on OpenAI's new [Plugin Extensions](https://developers.openai.com/plugins/build/extensions) (MCP Extensions).**

Track Google rankings for your keywords and up to five competitors, see what moved, and ask ChatGPT what to do about it. It isn't a chat bot that pastes tables: the dashboard is a real app in the ChatGPT sidebar, and ChatGPT can read and update it while you talk.

- **Rankings** for each keyword, with movers over 1, 7 and 30 days, CTR-weighted visibility and share of voice against your competitors
- **Google Search Console** (optional, free): clicks, impressions, CTR and Google's average position for every keyword, 90 days of history, and the queries you get found with but don't track yet
- **Search Console report tab**, like Google's Performance report: clicks, impressions, CTR and position over 7, 28 or 90 days vs the period before, top queries and pages with their change, brand vs other searches, countries, devices, striking-distance queries, and a CSV export
- **Keyword detail:** position history chart (with Search Console's average position) and today's Google top 10, People Also Ask questions, related searches and the AI overview
- **Site check** of your own pages once a day: missing titles, noindex, redirects, slow responses (free, no Apify needed)
- **Ask ChatGPT** to explain today's changes, plan your SEO week or turn the questions people ask into content ideas
- Opens as an app in the ChatGPT sidebar. Everything stays on your computer.

Try in chat: *"Refresh my SEO dashboard"* · *"Explain today's ranking changes"* · *"What should I work on for SEO this week?"*

## Built on ChatGPT Plugin Extensions

Plugin Extensions let a plugin hook into ChatGPT itself, not just answer in the chat. SEO Dashboard uses them like this:

| Extension | In SEO Dashboard |
| --- | --- |
| Sidebar app | The full dashboard opens from the ChatGPT sidebar and works fullscreen |
| Conversation panel | Opens the dashboard next to a chat, so you can talk about the rankings you're looking at |
| Composer @mentions | @mention a project or keyword to hand its report or history to ChatGPT |
| Deep links | Links (and ChatGPT) open the dashboard straight at a keyword or tab |
| Rich forms | Set up a project in a native ChatGPT form |
| Plugin settings | Refresh mode and hour, results per keyword, AI overview tracking, cost cap and notifications in ChatGPT's plugin settings |
| Plugin onboarding | A guided first-run setup in the chat |

Under the hood it's a local [MCP](https://modelcontextprotocol.io) server with an [MCP App](https://github.com/modelcontextprotocol/ext-apps) UI, using OpenAI's `@openai/mcp-extensions` SDK. It's a custom, independent plugin, not an official OpenAI or Apify product.

## Install

You need:

- the **ChatGPT desktop app** with plugins (tested on macOS)
- **[Node.js 22](https://nodejs.org) or newer**
- the **Codex CLI** in your terminal: `npm install -g @openai/codex`

Then run:

```sh
codex plugin marketplace add jenstools/custom-seo-dashboard-chatgpt-plugin-extensions
codex plugin add seo-dashboard@seo-dashboard
```

**Restart ChatGPT.** On a Mac: `osascript -e 'tell application id "com.openai.codex" to quit'`, then open it again. SEO Dashboard now shows up in the sidebar and as an @mention. ChatGPT walks you through setup the first time: your domain, the Google country and language, your keywords and competitors.

## How rankings refresh

Rankings come from Google results scraped by the Apify Actor [`apify/google-search-scraper`](https://apify.com/apify/google-search-scraper). Apify bills these runs to your own Apify account: about **$0.01 per keyword per refresh** at the default depth (top 20). Choose one of two ways:

1. **Through ChatGPT's Apify app** (default, no key stored). Connect the Apify app in ChatGPT. **Refresh** asks ChatGPT to run the Actor with your keyword list and hands the results to the dashboard.
2. **Daily with your own Apify API key.** Paste a key under *Settings → Apify API key* in the dashboard (never in the chat). The refresh then runs once a day after the hour you choose while ChatGPT is open, and catches up when you open ChatGPT after a missed day. Each run's cost is recorded, and *Max cost per run* is passed to Apify as the run's charge limit. The key is stored only on your computer in `~/.workbench/seo-dashboard/credentials.json` (readable only by you). ChatGPT never sees it. Setting the `APIFY_TOKEN` environment variable overrides it.

Search results come from the web. The plugin tells ChatGPT to treat them as data, never as instructions.

## Google Search Console (optional)

Add Google's own numbers to every keyword: clicks, impressions, CTR and average position over the last 28 days, a 90-day history, and the top queries people find your site with that you don't track yet (one click to track them). It's free and read-only. You need a Google Cloud service account:

1. In [Google Cloud](https://console.cloud.google.com/apis/library/searchconsole.googleapis.com), turn on the **Google Search Console API** for a project, then create a service account under *IAM & Admin → Service accounts*.
2. Open the service account → *Keys* → *Add key* → *Create new key* → *JSON*. A key file downloads.
3. In the dashboard, open *Settings → Google Search Console* and paste the whole file (never in the chat).
4. In [Search Console](https://search.google.com/search-console), open your property → *Settings* → *Users and permissions* → *Add user*, and add the service account's email (the dashboard shows it, with a copy button). *Restricted* is enough.

The dashboard then picks the property matching your domain (a domain property first), fetches the last 90 days and syncs once a day while ChatGPT is open. You can choose another property in Settings. Keywords are matched by exact query (case doesn't matter), so "running shoes" doesn't include "running shoes for kids". Search Console data lags two to three days.

The **Search Console** tab is a report for the whole site: pick 7 days, 28 days or 3 months and compare with the period before. Click a metric to chart it, switch the table between queries, pages, countries and devices, sort and filter it, or download it as CSV. *Brand vs other searches* splits clicks by queries containing your brand (your domain name by default; *Edit* to set your own terms). *Striking distance* lists queries at positions 4–20 with the clicks they could bring at #3. Ask ChatGPT to write the report, find quick wins or explain a change. Queries Google anonymizes count in the totals but not in brand vs other.

The key is stored only on your computer in `~/.workbench/seo-dashboard/gsc-credentials.json` (readable only by you) and only used to sign in to Google with read-only access. ChatGPT never sees it. *Disconnect* deletes the key and the synced data. Setting `SEO_DASHBOARD_GSC_KEY_FILE` to the path of a key file overrides the saved one.

## Your data

Projects, keywords and ranking history are stored in `~/.workbench/seo-dashboard/` on your computer. The plugin itself only contacts Apify (for the Google searches), Google's Search Console API if you connect it, and your own site (for the site check). What you ask about in a chat goes to ChatGPT like any other conversation.

## Update

```sh
codex plugin marketplace upgrade seo-dashboard
codex plugin add seo-dashboard@seo-dashboard
```

Then restart ChatGPT.

## Remove

```sh
codex plugin remove seo-dashboard@seo-dashboard
```

Your data in `~/.workbench/seo-dashboard/` stays until you delete that folder.

## License

MIT
