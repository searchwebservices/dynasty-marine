# Handoff: Set up Netlify (hosting)

**Status: NOT STARTED**
*When this is done, change the line above to `Status: COMPLETE — <date>` so it isn't repeated.*

**A free Netlify account is all you need.** Dynasty's site is small and static — the free tier covers
it comfortably.

---

## Why this matters

Right now the website is hosted on **Search Visionary Tech's** Netlify account, not Dynasty's. That
means:

- Pushing changes to this repository does **not** update the live site
- Josh has no login to the hosting
- Every publish has to go through Bay

Setting up Dynasty's own Netlify fixes all three at once. After this, editing a file and pushing it
puts it live in about a minute, and the agent can do that end to end without anyone in the middle.

**Current state, for reference:**

| | |
|---|---|
| Live URL | https://dynasty-marine-customs.netlify.app |
| Netlify project | `dynasty-marine-customs` |
| Site ID | `89802394-3b03-4350-8466-7d8b1ccea918` |
| Currently owned by | Search Visionary Tech |
| Git repo connected | none |

---

## Step 1 — Create the account

1. Go to https://app.netlify.com/signup
2. **Sign up with GitHub.** This matters — it links the accounts so connecting this repository later
   takes one click instead of a separate authorization dance.
3. Free tier. No card required.

---

## Step 2 — Get the site onto Dynasty's account

Two ways. Pick one.

### Option A — Connect this repo as a new site *(recommended, do it yourself)*

1. In Netlify: **Add new project → Import an existing project**
2. Choose **GitHub**, authorize, and pick this repository
3. Configure the build:

   | Setting | Value |
   |---|---|
   | Base directory | *(leave empty)* |
   | Build command | *(leave empty — there is no build step)* |
   | Publish directory | `website` |

   **That publish directory is the one people get wrong.** The site's files live in `website/`, not
   at the repo root. Point it at the root and you'll deploy a page listing folders.

4. Deploy. You'll get a URL like `dynasty-marine-customs-abc123.netlify.app`.
5. Rename it under **Project configuration → Change site name** to something clean.

From here, every push to `main` publishes automatically.

### Option B — Ask Bay to transfer the existing project

Keeps the current `dynasty-marine-customs.netlify.app` URL and its deploy history. Bay transfers the
project to Dynasty's Netlify team, then you connect the repo as in Option A step 2–3.

Slower (needs Bay), but you keep the existing address. **If Dynasty has already put that URL on
anything — ads, business cards, Google Business Profile — take this option.**

---

## Step 3 — Point the real domain at it *(when ready)*

If Dynasty has a proper domain (dynastymarinecustoms.com or similar), add it under **Domain
management → Add a domain**. Netlify walks through the DNS records and issues an HTTPS certificate
automatically and for free.

Don't rush this. Get the site publishing correctly first, then move the domain.

---

## Step 4 — Connect Netlify to your agent

This is what lets the agent check deploy status, read build logs, and diagnose a failed publish
without you opening a browser.

**Create a personal access token:**

1. Netlify → click your avatar → **User settings**
2. **Applications → Personal access tokens → New access token**
3. Name it `Dynasty Claude`
4. **Copy it now** — Netlify shows it once
5. Put it in `.env` at the root of this repo:

```
NETLIFY_PERSONAL_ACCESS_TOKEN=your-token-here
```

`.env` is gitignored and will never be committed.

**Then enable the connection.** This repo ships a `.mcp.json` that already declares the Netlify
integration. It reads the token from your environment — the file itself contains no secrets. When
you next start Claude Code in this folder, it will pick it up and ask you to approve the connection.

> **Never type a real token into `.mcp.json`.** It is committed to the repository. Tokens go in
> `.env`, which is not. Getting this backwards is how credentials end up public.

---

## Once it's connected, good things to ask

- *"Did the last deploy succeed?"*
- *"The site didn't update — what happened?"*
- *"Show me the build log for the most recent deploy."*
- *"Publish the changes we just made."*

---

## Note: Netlify Forms is switched off

Netlify has a built-in form handler that would capture submissions with almost no code. It is
currently **not enabled** on the project.

Leave it off. The better route for Dynasty's forms is sending them straight into GoHighLevel, so
website leads land in the same pipeline as the Facebook ones instead of a separate inbox. See
`handoffs/gohighlevel.md`.

---

## Until this is done

The agent can edit the site but **cannot publish it**. Be precise with the difference between
*changed* and *live* — and when something needs to go live before this is set up, it goes through
Bay.
