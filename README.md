# SEO Dashboard for ChatGPT

A free SEO dashboard by **snipKI GmbH**, built with OpenAI's new [Plugin Extensions](https://developers.openai.com/plugins/build/extensions). Track Google rankings, compare competitors, read Search Console reports and ask ChatGPT what changed.

This release is publicly available through a GitHub plugin marketplace for **manual installation in the desktop app**. It has not been submitted to or approved for OpenAI's public Plugin Directory. It is an independent plugin, not an official OpenAI, Google or Apify product.

## Install

You need the desktop app with plugin support, [Node.js 22 or newer](https://nodejs.org), and the [Codex CLI](https://developers.openai.com/codex/cli/).

Install the CLI once if needed:

```sh
npm install -g @openai/codex
```

Then install SEO Dashboard:

```sh
codex plugin marketplace add jenstools/custom-seo-dashboard-chatgpt-plugin-extensions
codex plugin add seo-dashboard@seo-dashboard
```

Fully quit and reopen the desktop app. SEO Dashboard appears in the sidebar. Ask: **"Set up my SEO dashboard."** Enter your domain, Google country/language, keywords and up to five competitors.

### Install from the release ZIP

Download the ZIP from [Releases](https://github.com/jenstools/custom-seo-dashboard-chatgpt-plugin-extensions/releases), extract it, and register the extracted marketplace folder:

```sh
codex plugin marketplace add /absolute/path/to/seo-dashboard
codex plugin add seo-dashboard@seo-dashboard
```

The extracted folder contains .agents/plugins/marketplace.json and plugins/seo-dashboard/. Use one installation source for this plugin to avoid duplicates.

## Connect your credentials

Enter credentials only in **dashboard Settings**, never in a conversation.

1. **Apify:** open Settings → Apify API key and paste your own key. This enables direct daily refreshes while the local plugin is running, with catch-up after missed days. Set the refresh hour and maximum cost per run in Settings. Apify bills your own account for [Google Search Scraper](https://apify.com/apify/google-search-scraper) runs; the plugin itself is free. Estimates depend on keywords, depth and current Apify pricing.
2. **Google Search Console, optional:** enable the [Search Console API](https://console.cloud.google.com/apis/library/searchconsole.googleapis.com) in your Google Cloud project. Create a service account and download its JSON key. Paste the JSON in Settings → Google Search Console. Add that service-account email to your property under Search Console → Settings → Users and permissions; Restricted access is sufficient for reporting. Choose the matching property in the dashboard. The plugin uses read-only API access.
3. Click **Refresh** in the dashboard. After the run completes, ask **"Explain today's ranking changes."** Search Console syncs daily; its data normally lags behind the current date.

If you prefer not to save an Apify key, connect the Apify app and use the dashboard's ChatGPT refresh mode. ChatGPT plans the Actor run, obtains permission for the charge, and imports its results.

## What you get

- Up to 200 keywords per project and five competitor domains, with positions, daily/weekly/monthly movers, visibility and share of voice.
- Keyword history, Google results, People Also Ask, related queries and AI overviews when the source provides them.
- Search Console clicks, impressions, CTR and average position. Compare 7, 28 or 90 days with the previous period; inspect queries, pages, countries, devices, brand searches and queries near the top positions; export CSV.
- A daily check of your own pages for missing titles, noindex, redirects and slow responses.
- A sidebar dashboard, a panel beside conversations, project and keyword mentions, setup forms and plugin settings using OpenAI Plugin Extensions.

Search Console position is an average across impressions. Daily scraped rankings are observations for the configured country, language and depth. The dashboard distinguishes them; no ranking improvement is guaranteed.

## Your data

The plugin stores projects, keywords and history in `~/.workbench/seo-dashboard/`. Apify and Google keys are stored in separate local files with owner-only file permissions; they are not encrypted by the plugin. The Apify key is used to contact Apify. The Google private key signs a read-only access request to Google's token endpoint. Site checks contact your configured website. Direct mode does not send these credentials to a snipKI server.

The local files are `credentials.json` for Apify and `gsc-credentials.json` for Search Console. Credentials are excluded from returned tool results. Data you ask ChatGPT to analyze is shared with ChatGPT as part of that conversation. Web results and Search Console queries are treated as untrusted data.

**Disconnect Apify** deletes the saved Apify key. **Disconnect Search Console** deletes its saved key and synced data. Removing the plugin leaves its local data folder in place until you delete it. Environment overrides `APIFY_TOKEN` and `SEO_DASHBOARD_GSC_KEY_FILE` take precedence over saved credentials; disconnecting a saved key does not remove an environment override.

## Update or remove

```sh
codex plugin marketplace upgrade seo-dashboard
codex plugin add seo-dashboard@seo-dashboard
```

Fully quit and reopen the desktop app after updating. To remove:

```sh
codex plugin remove seo-dashboard@seo-dashboard
```

## Support and license

Report installation or plugin issues through [GitHub Issues](https://github.com/jenstools/custom-seo-dashboard-chatgpt-plugin-extensions/issues). Include your app version, Node version and the error message after removing keys, tokens and private site data. Never attach credentials.

[snipKI](https://snipki.de) · [Publisher details](https://snipki.de/impressum/) · MIT license.
