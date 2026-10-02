# Custom SEO Dashboard for ChatGPT

**A custom SEO dashboard that runs inside ChatGPT, built as a plugin on OpenAI's new [Plugin Extensions](https://developers.openai.com/plugins/build/extensions) (MCP Extensions).**

Track Google rankings for your keywords and up to five competitors, see what moved, and ask ChatGPT what to do about it. It isn't a chat bot that pastes tables: the dashboard is a real app in the ChatGPT sidebar, and ChatGPT can read and update it while you talk.

- **Rankings** for each keyword, with movers over 1, 7 and 30 days, CTR-weighted visibility and share of voice against your competitors
- **Keyword detail:** position history chart and today's Google top 10, People Also Ask questions, related searches and the AI overview
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

## Your data

Projects, keywords and ranking history are stored in `~/.workbench/seo-dashboard/` on your computer. The plugin itself only contacts Apify (for the Google searches) and your own site (for the site check). What you ask about in a chat goes to ChatGPT like any other conversation.

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
