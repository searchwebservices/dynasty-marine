# Dynasty Marine Customs

The website codebase and operations workspace for **Dynasty Marine Customs, LLC** — Sarasota, FL.

Built and handed over by [Search Visionary Tech](https://searchvisionary.tech).

---

## Start here

**1. Open this folder in VS Code, then open Claude Code.**

**2. Say this to your agent:**

> Read CLAUDE.md, then tell me what's outstanding.

That's genuinely all. `CLAUDE.md` is the operating manual — it explains the business, the website,
what's connected, what isn't, and what's pending. Your agent reads it and picks up from there.

---

## What's in here

| | |
|---|---|
| `CLAUDE.md` | The operating manual. Your agent reads this first. |
| `website/` | The live website — 8 pages, no build step. |
| `handoffs/` | Setup tasks that aren't done yet. Work through these. |
| `docs/` | Reference: the System Builder spec, the handoff call, the design system. |

---

## First three things to do

1. **Finish signing in to Claude** in the VS Code extension *(update the card on the Claude account
   if the payment is still bouncing)*
2. **Connect GoHighLevel** — `handoffs/gohighlevel.md`
3. **Settle site publishing access** — `handoffs/deploying-the-site.md`

---

## One rule

**Never commit secrets to this repository.** API keys, tokens, and passwords belong in a local
`.env` file, which is already gitignored. This repository may be public.

---

## Live site

https://dynasty-marine-customs.netlify.app

Note that pushing to this repository does **not** currently update the live site — see
`handoffs/deploying-the-site.md`.
