# SEO Dashboard 0.1.7

Free, public release for manual installation in the desktop app, published by snipKI GmbH.

- Adds a downloadable marketplace ZIP, a square PNG icon, snipKI GmbH publisher metadata and a shorter listing subtitle.
- Makes setup with your own local Apify key and optional Google Search Console service-account JSON the recommended onboarding flow.
- Documents installation, costs, local credential storage, disconnect behavior and support.
- Preserves the existing ranking dashboard, Search Console reports and Plugin Extensions integration.

Requires desktop plugin support, Node.js 22+ and the Codex CLI. Credentials are entered in dashboard Settings and remain on the user's computer. Apify charges each user's own account for scraping. The plugin itself is free.

Distribution: public GitHub marketplace. OpenAI Plugin Directory submission: not submitted. The directory requires a hosted HTTPS MCP server; this release preserves the chosen local architecture.

Validation: the existing end-to-end suite passed for 37 tools using local mock Apify and Google services. It covers setup, refresh, reports, daily scheduling, owner-only credential files and credential deletion. No Apify key, Google private key or access token appeared in the 104 tested returned results or tested logs. This does not claim live testing against users' production provider accounts.
