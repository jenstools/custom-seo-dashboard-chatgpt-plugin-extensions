---
name: onboarding
description: First-run setup for SEO Dashboard. Runs once after the plugin is installed.
---

# SEO Dashboard onboarding

1. Welcome the user in one sentence: SEO Dashboard tracks their Google rankings every day, next to their chats, like a small SEO tool. ChatGPT can explain what changed and what to work on.
2. Ask for their domain, the Google country and language to check (default `us`/`en`), five to twenty keywords they care about, and up to five competitor domains. Create the project with `seo.setupProject`. If they'd rather type it in a form, call `seo.setupProject` with no arguments, or `seo.open` to show the setup screen.
3. Explain the two ways rankings refresh, in two sentences:
   - Through ChatGPT's Apify app (the default): the Refresh button asks you to run the Apify Actor `apify/google-search-scraper`, and the results are recorded with `seo.ingest`. The Apify app must be connected in ChatGPT.
   - Directly every day: the user pastes an Apify API token in the dashboard (Settings, never in the chat), and it runs once a day after a set hour while ChatGPT is open (catching up if a day was missed).
   Mention the cost: about $0.01 per keyword per refresh at top 20 on Apify's prices.
4. Mention in one sentence that they can add Google Search Console clicks and impressions per keyword for free under Settings → Google Search Console in the dashboard (a read-only service-account key, pasted there, never in the chat).
5. Offer the first refresh: call `seo.plan`, and once the user agrees, run the Actor with the Apify app using exactly that input, then pass the dataset items to `seo.ingest`. Finish with `seo.summary`.
