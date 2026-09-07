# Handoff: Connect Google Ads + Google Analytics

**Status: NOT STARTED** (agent access) — but **the campaigns are now LIVE as of Sept 2026.**
*When this is done, change the line above to `Status: COMPLETE — <date>` so it isn't repeated.*

> **Read `docs/google-ads-2026-09-07-review.md` first.** Ads are spending real money now, while
> the agent still has no API access and the site still has no tracking tag. That review flags a
> possible landing-page URL mismatch on two of the three campaigns which needs Josh to verify.
> Until then, Route A below (export to Drive) is the working path — the agent already has Google
> Drive access, so an export to Sheets is readable immediately with no credential setup.

---

## What this unlocks

Reporting and analysis on where the ad money is going and what it's producing. Josh's stated reason
for wanting an agent at all: *"I need someone to be on top of marketing."* This is that.

Once connected, the agent can review spend by campaign and ad group, find keywords burning budget
without converting, compare traffic sources, and tell you what changed week over week — without
anyone logging into a dashboard.

---

## Before any of this: the site has no analytics on it

**There is no tracking tag on dynastymarinecustoms.com today.** Not Google Analytics, not Google
Tag Manager, not an Ads conversion tag. All eight pages in `website/` load zero external scripts —
you can check it yourself in about five seconds:

```
grep -rn "gtag\|googletagmanager" website/
```

That returns nothing, and that changes the order of this whole document. Google Ads can tell you
what you *spent* and how many people *clicked*, because Google owns that side. It cannot tell you
what any of them did next, which pages they landed on, or which ones asked for an estimate —
because nothing on the site is reporting back. A GA4 property created today starts empty and stays
empty until a tag goes on the pages.

So instrumenting the site is a prerequisite, not an improvement to do later:

1. **Netlify first** (`handoffs/netlify.md`). Until the site is on Dynasty's own Netlify with this
   repo connected, tagging work sits in the repository unpublished — you would be adding tags to a
   site that nobody is serving. Every publish still routes through Bay until that moves.
2. **Create the GA4 property** and note its Measurement ID (`G-XXXXXXXXXX`). Dynasty is in
   Sarasota, Florida — set the property to **US Eastern time and USD**, not Pacific and not Mexico.
   Reporting timezone cannot be changed retroactively; get it right on day one.
3. **Put the tag on all eight pages** in `website/`. There is no shared header or template — each
   page is standalone HTML, so the snippet goes into each one individually. Miss one and that page
   is invisible in every report you ever run.
4. **Verify in GA4 Realtime** before moving on. Open the live site, click through two pages, and
   watch yourself appear. If you do not appear, nothing downstream of this works and you will not
   find out for weeks.
5. **Then** import the conversion into Google Ads and link Ads to GA4.
6. **Then** Route B below, which is the only part with a review queue attached.

## The estimate form is not a form

`website/quote.html` looks like a contact form and is not one. It is a multi-step JavaScript wizard,
and its `submit()` function at the bottom of the file does exactly one thing with the answers:

```js
location.href = `mailto:${EMAIL}?subject=...&body=...`;
```

It builds an email and hands it to the visitor's mail client. **There is no form post, no server, no
thank-you page load.** Two consequences, and both of them matter:

**Where the conversion fires.** The usual advice — put an Ads conversion tag on the thank-you page,
or fire on form submit — does not apply, because neither event exists. Fire a GA4 `generate_lead`
event *inside* `submit()`, immediately before the `location.href` line, then import that event into
Google Ads as a conversion. Do not hand-place an `AW-` tag in that code path; importing from GA4
keeps one definition of a lead instead of two that will disagree.

**What the number actually means.** A `mailto:` opens a draft in whatever mail client the visitor
has. It does not send it, and it cannot tell you whether they sent it. So `generate_lead` counts
*people who finished the wizard*, not *estimate requests Dynasty received* — the two will diverge,
and the gap is larger on mobile and on machines with no mail client configured. Treat the number as
intent, and be careful before letting Google Ads bid toward it, because it will optimise for the
signal it is given rather than the leads that actually arrive.

If the goal is leads that reliably land somewhere, the real fix is sending the wizard's answers into
GoHighLevel instead of into a mail draft — see `handoffs/gohighlevel.md`. That is a separate piece
of work from this one, but it is the one that changes the business outcome.

---

## What you need

Google's Ads and Analytics APIs need slightly more setup than a single API key. There are two routes.

### Route A — read-only reporting (simpler, start here)

For analysis and reporting, the agent can work from **exported data**. Josh can pull a CSV from the
Google Ads UI or Analytics and drop it in the repo, and the agent will analyze it immediately with
no credentials at all.

This sounds primitive but it is genuinely useful on day one and requires zero setup. Good for
answering *"where is my money going"* this week while Route B gets sorted.

**One limit worth knowing.** This works for **Google Ads** exports — spend, clicks, keywords,
impressions — because Google records those on its own side regardless of what the website does. It
does **not** work for Analytics yet: there is nothing to export, because there is no tag on the site.
Until the tag is on, half of Route A is available and half of it is empty.

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

Start with **Route A**, on the Ads side only, and start the developer token application early —
Google reviews those and it is the longest lead time in this document, so it should be waiting on
you rather than the other way round. Meanwhile the tagging work at the top of this page is what
turns the Analytics half from empty into useful. Move to Route B when you want the agent pulling numbers unprompted.

Ask your agent to help with Route B when you're ready — it can walk you through the console screens
step by step.

---

## Good first things to ask for

- *"Here's last month's Google Ads export. Which ad groups spent the most and produced the least?"*
- *"Compare this month's traffic to last month and tell me what actually changed."*
- *"Which pages do people land on and immediately leave?"* (needs the tag on the site first —
  see the top of this document)
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
