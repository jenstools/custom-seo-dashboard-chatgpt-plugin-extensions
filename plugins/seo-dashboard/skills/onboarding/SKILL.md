---
name: onboarding
description: First-run setup for SEO Dashboard. Runs once after the plugin is installed.
---

# SEO Dashboard onboarding

1. Welcome the user in one sentence: SEO Dashboard tracks Google rankings and Search Console performance beside their chats in the desktop app. Explain that the plugin is free and Apify bills their own account for ranking refreshes.
2. Ask for their domain, Google country and language (default us/en), five to twenty keywords, and up to five competitor domains. Create the project with seo.setupProject. If they prefer a form, call seo.setupProject with no arguments, or seo.open to show the setup screen.
3. Open seo.open and guide them to Settings → Apify API key for daily direct refreshes. Credentials must be entered only in the dashboard's Settings. Never ask for, read back, or include an API key in the chat. The key stays on their computer. Refreshes run while the local plugin is running and catch up after a missed day. Explain the estimated cost and cost cap shown by the dashboard; actual charges depend on Apify's current pricing.
4. Guide them to Settings → Google Search Console for optional clicks, impressions and performance reports. They paste the service-account JSON only there, then add its service-account email as a user of their Search Console property. Never ask them to paste the JSON in chat. The key stays on their computer and Search Console access is read-only. The dashboard's setup instructions cover the Google Cloud API and property permissions.
5. Offer a first refresh using the dashboard's Refresh button. Explain that it charges their own Apify account; the user initiates it in the dashboard. After completion, call seo.summary to explain the result. Do not claim a refresh completed unless the tool data shows it.
6. If the user prefers the connected Apify app instead of saving a local key, use seo.plan, explain its estimate, run the returned Actor/input after the user agrees, ingest the complete dataset with seo.ingest, and finish with seo.summary. Do not mix this with a direct-mode automatic refresh.
