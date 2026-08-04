# Handoff: Publishing the website

**Status: BLOCKED — needs a decision from Josh + Bay before the agent can publish**
*When this is resolved, change the line above and record what was decided.*

---

## Read this before editing the site

You can edit every file in `website/` today. **You cannot publish those edits yet.** Nothing in this
repository deploys automatically. Understand why before you promise Josh a live change.

---

## Where the site actually lives

| | |
|---|---|
| **Live URL** | https://dynasty-marine-customs.netlify.app |
| **Host** | Netlify |
| **Netlify project** | `dynasty-marine-customs` |
| **Site ID** | `89802394-3b03-4350-8466-7d8b1ccea918` |
| **Current live deploy** | `6a696f92f7a1b064dfdb1f7e` |
| **Connected git repo** | **none** |
| **Netlify account** | Search Visionary Tech's team — **not Dynasty's** |

Two consequences, both important:

1. **Committing to this repository does not update the live site.** There is no link between them.
   Every deploy so far has been a manual upload. If you edit `website/index.html`, commit it, and
   tell Josh it's live — you will be wrong.
2. **Dynasty does not own the hosting account.** The site sits on Search Visionary Tech's Netlify
   team. Josh has no login to it as of this handoff.

---

## The decision that needs making

Josh should pick one of these with Bay. Until then, publishing runs through Bay.

### Option A — Transfer the Netlify site to Dynasty's own account *(recommended)*

Josh creates a free Netlify account, Bay transfers the project to it, then Josh connects this GitHub
repository to it. After that, **every push to `main` publishes automatically** and the agent can ship
changes end to end with no one in the middle.

This is the option that makes the handoff actually complete. It is also the most work up front —
maybe thirty minutes, once.

### Option B — Josh gets added to the existing Netlify team

Faster. Josh gets access without a transfer, and the repo can still be connected for auto-deploy.
But the account stays under Search Visionary Tech's billing and control, which is a strange
long-term arrangement for a business that now owns its own site.

### Option C — Leave it, publish through Bay

The agent makes changes, commits them, and Bay pushes them live. Works, but it puts a person in the
middle of every content update, which is the exact bottleneck this handoff exists to remove.

---

## Once auto-deploy is connected (Option A or B)

The workflow becomes:

```
edit files in website/  →  commit  →  push to main  →  Netlify builds  →  live in ~1 minute
```

Netlify settings for this site, when connecting the repo:

- **Base directory:** *(leave empty)*
- **Build command:** *(leave empty — there is no build step)*
- **Publish directory:** `website`

That last one matters. The site's files live in `website/`, not at the repo root.

---

## Note: Netlify Forms is not enabled

Worth knowing, because it's relevant to the "wire up the website forms" task in
`handoffs/gohighlevel.md`.

Netlify has a built-in form-capture feature that would collect submissions with almost no code — but
it is **currently switched off** on this project. It is one option for making the forms work. Sending
submissions straight into GoHighLevel is the better one, because it puts website leads in the same
pipeline as the Facebook leads instead of a second inbox.

Recommendation: **do the GoHighLevel route**, and leave Netlify Forms off.

---

## Until this is resolved

Edit freely. Commit freely. Just be precise with Josh about the difference between *changed* and
*live* — and when he wants something live, say that it needs to go through Bay for now.
