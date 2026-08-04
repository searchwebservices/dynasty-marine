# Handoff: Connect GoHighLevel

**Status: NOT STARTED**
*When this is done, change the line above to `Status: COMPLETE — <date>` so it isn't repeated.*

---

## What this unlocks

GoHighLevel is where every Dynasty lead already lands. Connecting it lets your agent read the
pipeline, report on it, update records, and eventually route website form submissions into it
instead of nowhere.

Today the flow is:

```
Facebook ads → lead form → (automated) → GoHighLevel → email notification
```

Once connected, the agent can answer things like *"how many leads came in last week and how many
made it to qualified?"* and act on the answer.

---

## The API key — Josh already made this

On the 2026-08-03 call, Josh created the key. It is **on his machine, not ours**, and that was
deliberate. Bay's words: *"this is going to be up to you because I'm not going to really take this
from you."*

**What was created:**

- GoHighLevel → **Settings → Private Integrations** (API v2.0)
- Name: **Dynasty Marine Claude**
- Description: *for Bay setting up Claude Code*
- Scopes: **all of them selected**
- The token was copied and saved locally

> **GoHighLevel shows a private integration token exactly once.** If Josh cannot find the one he
> saved, don't hunt for it — delete that integration and create a new one. Takes two minutes.

### If the key needs to be recreated

1. Log in to GoHighLevel
2. **Settings** (left sidebar) → **Private Integrations**
3. **Create new integration**
4. Name it `Dynasty Marine Claude`
5. Select the scopes you want the agent to have. Josh previously selected all of them — that is the
   right call if you want the agent managing the CRM rather than just reading it.
6. **Copy the token immediately** and save it somewhere local and durable. Not in a browser tab, not
   in this repository.

---

## Where to put the key

Create a file named `.env` in the root of this repository:

```
GHL_API_KEY=your-token-goes-here
GHL_LOCATION_ID=your-location-id
```

`.env` is already listed in `.gitignore`, so it will not be committed and will not end up on GitHub.

**Never paste this token into a chat message, a code file, or anything that gets committed.**
Ask your agent to read it from `.env` — it can do that without the value ever appearing in the
conversation.

### Finding your Location ID

In GoHighLevel, the Location ID (sometimes labeled Sub-Account ID) appears in **Settings → Business
Profile**, and also in the browser URL when you're inside the account — the long string after
`/location/`.

---

## Telling your agent to connect

Once `.env` exists, start a session and say something like:

> Read the GoHighLevel credentials from `.env` and connect. Then show me every lead from the last
> 30 days grouped by pipeline stage.

The agent uses GoHighLevel's **API v2** at `https://services.leadconnectorhq.com`. It authenticates
with a `Bearer` token in the `Authorization` header and requires a `Version` header. It can work out
the specific endpoints from GoHighLevel's API documentation — you don't need to supply them.

---

## Good first things to ask for

Start with read-only work. It builds trust in the connection before anything writes to the CRM.

- *"How many leads came in this month, and how many reached qualified?"*
- *"Which lead source is actually converting — and what's the drop-off stage?"*
- *"Show me every lead sitting in 'contacted' for more than 7 days with no follow-up."*
- *"Draft an SMS follow-up sequence for leads that go quiet after the first reply."*

Then move to writes once the reads look right.

---

## Careful with these

- **Automations that send real messages.** Editing a GoHighLevel workflow can start texting real
  customers the moment it's saved. Have the agent describe the change and confirm before saving.
- **Bulk edits.** Anything touching many contacts at once — verify the filter first on a small set.
- **Deleting.** Don't. Archive or tag instead.

---

## Still open: website forms → GoHighLevel

**This is not done and is worth doing early.**

The website's forms — `website/quote.html` and the two inline forms on `index.html` — currently
collect nothing. A customer fills one out and it goes nowhere.

Once GoHighLevel is connected, point those forms at it so website leads land in the same pipeline as
the Facebook ones. That gives Dynasty a single place to work, and it makes the System Builder's
output land somewhere useful when it's built.

Ask your agent: *"wire the website forms into GoHighLevel so submissions create leads."* It will need
to pick an approach — a GoHighLevel inbound webhook is usually the simplest — and it should walk you
through the choice before implementing.
