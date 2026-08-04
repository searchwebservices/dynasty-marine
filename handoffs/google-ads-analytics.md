# Handoff: Connect Google Ads + Google Analytics

**Status: NOT STARTED**
*When this is done, change the line above to `Status: COMPLETE — <date>` so it isn't repeated.*

---

## What this unlocks

Reporting and analysis on where the ad money is going and what it's producing. Josh's stated reason
for wanting an agent at all: *"I need someone to be on top of marketing."* This is that.

Once connected, the agent can review spend by campaign and ad group, find keywords burning budget
without converting, compare traffic sources, and tell you what changed week over week — without
anyone logging into a dashboard.

---

## What you need

Google's Ads and Analytics APIs need slightly more setup than a single API key. There are two routes.

### Route A — read-only reporting (simpler, start here)

For analysis and reporting, the agent can work from **exported data**. Josh can pull a CSV from the
Google Ads UI or Analytics and drop it in the repo, and the agent will analyze it immediately with
no credentials at all.

This sounds primitive but it is genuinely useful on day one and requires zero setup. Good for
answering *"where is my money going"* this week while Route B gets sorted.

### Route B — live API access (full capability)

For the agent to pull data itself on demand:

**Google Analytics 4**
1. Go to https://console.cloud.google.com and create a project (or reuse one)
2. Enable the **Google Analytics Data API**
3. Create a **service account**, download its JSON key
4. In Google Analytics → **Admin → Property Access Management**, add the service account's email
   address with **Viewer** access
5. Note the **GA4 Property ID** (Admin → Property Settings — a numeric string)

**Google Ads**
1. In the same Cloud project, enable the **Google Ads API**
2. Apply for a **developer token** at Google Ads → Tools → API Center
   *(Google reviews these. Basic access can take a few days.)*
3. Note the **Customer ID** — the `xxx-xxx-xxxx` number in the top corner of Google Ads

Then in `.env` at the repo root:

```
GA4_PROPERTY_ID=
GOOGLE_ADS_DEVELOPER_TOKEN=
GOOGLE_ADS_CUSTOMER_ID=
GOOGLE_APPLICATION_CREDENTIALS=./google-service-account.json
```

`.env` and `*.json` credential files are gitignored. **The service account JSON must never be
committed** — it is a live credential.

---

## Which route to take

Start with **Route A**. Get value this week from exported data while the developer token application
is pending. Move to Route B when you want the agent pulling numbers unprompted.

Ask your agent to help with Route B when you're ready — it can walk you through the console screens
step by step.

---

## Good first things to ask for

- *"Here's last month's Google Ads export. Which ad groups spent the most and produced the least?"*
- *"Compare this month's traffic to last month and tell me what actually changed."*
- *"Which pages do people land on and immediately leave?"*
- *"Are the leads from Google Ads converting at a different rate than the Facebook ones?"*
  (needs GoHighLevel connected too — see `gohighlevel.md`)

---

## Careful with these

- **Never let the agent change ad spend or bids without explicit confirmation.** Analysis and
  recommendations are safe; budget changes cost real money immediately.
- **Pausing campaigns** is reversible but disruptive mid-flight. Confirm first.
- Analytics is read-only by nature — low risk. Ads is not.

---

## Note on Facebook / Meta

Meta does not offer the same kind of API access for this use case, and Bay flagged on the handoff
call that they are restrictive about it. **This is fine.** Dynasty's Facebook lead ads already feed
into GoHighLevel automatically, so connecting GoHighLevel captures that traffic's outcomes even
without a direct Meta connection. You lose the ability to adjust Meta campaigns programmatically —
you keep the ability to see what they produce.
