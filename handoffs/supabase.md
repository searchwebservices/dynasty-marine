# Handoff: Set up Supabase (database)

**Status: NOT STARTED**
*When this is done, change the line above to `Status: COMPLETE — <date>` so it isn't repeated.*

**A free Supabase account is all you need** to start. The free tier is generous and Dynasty won't
come close to outgrowing it during the build.

---

## Why Dynasty needs a database

Right now this repo is eight static HTML pages. Static pages can't remember anything — every visitor
gets the same file and nothing is stored.

The **System Builder** (`docs/system-builder-spec.md`) can't exist without somewhere to keep:

- **A parts catalog** — engines, electronics, outdrives, and what each one costs
- **Labor rates** — hours per job type, shop rate
- **Saved configurations** — a customer builds a refit, leaves, comes back to it
- **Submitted quotes** — so Josh can see what people are configuring, even when they don't call

That last one is quietly the most valuable part. A customer who builds a $40,000 repower and then
doesn't submit is a lead worth knowing about.

Supabase gives Dynasty a real PostgreSQL database, an auto-generated API, file storage, and
authentication, without running a server.

**Nothing has been built yet.** There is no schema, no project, no tables. This is a clean start, and
it should be designed alongside the System Builder rather than guessed at in advance.

---

## Step 1 — Create the account and project

1. Go to https://supabase.com/dashboard and sign up
2. **Sign in with GitHub** — same account as Netlify, keeps things simple
3. **New project**:

   | Field | Value |
   |---|---|
   | Name | `dynasty-marine` |
   | Region | **East US (North Virginia)** — closest to Sarasota |
   | Database password | Generate a strong one and **save it in a password manager** |
   | Plan | **Free** |

4. Wait about two minutes while it provisions.

> **Save that database password properly.** Supabase does not show it again, and recovering from a
> lost one means resetting it. A password manager, not a sticky note.

**Free tier caveat worth knowing:** a free project **pauses after about a week of no activity**. It
un-pauses from the dashboard in a couple of minutes, but don't be alarmed the first time. Once
Dynasty has real customers using the System Builder, this is the reason to move to the paid tier.

---

## Step 2 — Collect the three values

**Settings → API** (and **Settings → General** for the first one):

| Value | Where | Sensitivity |
|---|---|---|
| **Project reference ID** | Settings → General *(a short string like `abcdefghijklmnop`)* | Not secret |
| **Project URL** | Settings → API *(`https://<ref>.supabase.co`)* | Not secret |
| **anon / public key** | Settings → API | Safe in browser code |
| **service_role key** | Settings → API | **SECRET — server only, never in a web page** |

The **anon key is designed to be public** — it goes in front-end code, and Supabase's row-level
security is what actually protects the data. That's normal and correct.

The **service_role key bypasses every security rule.** It never goes in a web page, never in a
committed file, never in a chat message. `.env` only.

Put them in `.env` at the root of this repo:

```
SUPABASE_PROJECT_REF=your-project-ref
SUPABASE_URL=https://your-project-ref.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

`.env` is gitignored and will never be committed.

---

## Step 3 — Create the access token for your agent

Separate from the keys above. This is what lets the agent read the schema, run queries, and create
tables for you.

1. https://supabase.com/dashboard/account/tokens
2. **Generate new token**, name it `Dynasty Claude`
3. **Copy it now** — shown once
4. Add to `.env`:

```
SUPABASE_ACCESS_TOKEN=your-token-here
```

---

## Step 4 — Turn the connection on

This repo already ships a `.mcp.json` declaring both the Supabase and Netlify integrations. It reads
every value from your environment, so **the file itself contains no secrets** and is safe to commit.

For Claude Code to see those values, they need to be in your shell. Ask your agent:

> Add my Supabase and Netlify tokens from `.env` to my shell profile.

It will append the `export` lines to `~/.zshrc`. Then **quit and reopen VS Code** — environment
variables are only read at startup. Claude Code will ask you to approve the new connections.

Verify it worked by asking:

> List the tables in my Supabase project.

An empty list is the correct answer at this stage. An error means the token or project ref is wrong.

> **Never type a real key into `.mcp.json`.** It is committed to the repository. Keys go in `.env`,
> which is not. Getting this backwards is how credentials end up public — it has happened to better
> setups than this one.

---

## Step 5 — Design the schema *(later, with the System Builder)*

Don't guess at tables now. The schema falls out of the pricing logic, and the pricing logic is in
Josh's head — see `docs/system-builder-spec.md` §"the hard part."

When you get there, the shape will be roughly:

- `parts` — catalog with cost, category, compatibility
- `labor_rates` — job type, hours, rate
- `vessels` — make/model/year and what fits them
- `configurations` — a customer's in-progress build
- `quote_requests` — submitted estimates, and where they go into GoHighLevel

Build it with migrations so there's a history, not by clicking around the dashboard. Your agent knows
how.

---

## Two safety notes

**Row-level security is off by default on new tables.** A table without RLS enabled and the anon key
in a public web page means anyone can read it. Turn RLS on for every table that the website touches.
Supabase warns about this in the dashboard — don't dismiss the warning.

**The agent has write access to the database.** That's intentional: it has to create tables to build
anything. Once the schema settles and there's real customer data in it, add `--read-only` to the
Supabase entry in `.mcp.json` and let migrations be the only way things change. Ask your agent to do
it when you're ready.

---

## Before this goes live to real customers

Bay offered a security review before anything customer-facing ships — see §2 of `CLAUDE.md`. A
database holding customer contact details and quote history is exactly what that review is for. Take
him up on it.
