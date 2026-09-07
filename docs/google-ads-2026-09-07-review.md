# Google Ads — first review, 2026-09-07

Josh reports the campaigns are **live** as of early September 2026. This note records what the
agent could and could not see, and the one thing that needs checking urgently.

## What was reviewable

No Google Ads API access exists yet (`handoffs/google-ads-analytics.md` is still Route B pending),
so there is **no spend, click or conversion data available to the agent**. The only Ads artifact
reachable was a build sheet in Josh's Google Drive:

- **"Dynasty Google Ads Import 2026-09-03"** (Google Sheet, owned by Joshua@dynastymarinecustoms.com)

That is an *import/upload* sheet — the campaign structure, not a performance report. Everything in
it was still `Paused` at the time it was written, which predates the launch.

## Campaign structure as built

| Campaign | Type | Daily budget | Bid strategy | Ad group | Keywords |
|---|---|---|---|---|---|
| Mobile Service | Search | $20 | Maximize clicks | Mobile Mechanic | 12 phrase |
| Maintenance and Repair | Search | $20 | Maximize clicks | Outboard Service | 18 phrase |
| Marine Electronics | Search | $10 | Maximize clicks | Electronics | 7 phrase |

Geo on all three: Sarasota County + Manatee County, Florida. Each campaign carries the same ~60
negative keyword list (jobs/DIY/for-sale/parts-diagram style exclusions). Negatives look sensible.
One responsive search ad per ad group.

## ⚠️ Landing page URLs do not match the site

The Final URLs in the sheet point at three paths. Comparing against the pages that actually exist
in `website/`:

| Ad Final URL | Page exists? |
|---|---|
| `/mobile-marine-mechanic` | **No** — no such page, and no page in `website/` mentions mobile service, "we come to you", or dockside work at all |
| `/service-annual-maintenance` | **No** — the real page is `/service-annual-service` |
| `/service-marine-electronics` | Yes |

So two of the three campaigns — carrying $40/day of the $50/day total — point at paths with no
corresponding page in the site source. If those 404, that spend buys a bounce.

**Caveat, and it matters:** the ads point at `dynastymarinecustoms.com`, while this repo builds the
site published at `dynasty-marine-customs.netlify.app`, and the Netlify handoff is not done. The
agent's network egress is blocked from reaching `dynastymarinecustoms.com`, so it could **not**
confirm what is actually served there. It is possible that domain hosts a different or older site
where those pages do exist. **Josh should click the three URLs and confirm** — that settles it in
about ten seconds.

Note also that the entire "Mobile Service" campaign sells a service line that is not one of
Dynasty's six documented service lines in `CLAUDE.md` §1. Whether Dynasty offers mobile/dockside
service is a question for Josh, not an assumption to make.

## No conversion tracking exists

Verified directly — `grep -rn "gtag\|googletagmanager" website/` returns nothing, and no page in
`website/` loads any external script. There is no GA4 tag, no Ads conversion tag, no GTM container.

Consequence: Google Ads will report impressions, clicks and cost, because Google owns that side. It
will report **zero conversions**, because nothing on the site reports back. "How are the ads doing"
is therefore only half-answerable until a tag exists, and Maximize clicks is buying clicks with no
feedback on which ones were worth anything.

Two things worth knowing:
- **Call reporting is the exception.** Google Ads can track phone calls from call assets using a
  Google forwarding number, with no website tag at all. For a marine repair shop that is likely the
  main conversion. This is the one real conversion signal available today.
- The quote wizard's `submit()` in `quote.html` fires a `mailto:` — no form post, no thank-you page.
  See `handoffs/google-ads-analytics.md` for why that changes where a conversion can fire.

## Recommended order

1. Josh confirms the three landing URLs load. If they 404, pause those two campaigns or repoint the
   Final URLs before spending another day. **Any budget or pause change needs Josh's explicit
   go-ahead** — see `CLAUDE.md` §9.
2. Turn on call reporting — the only conversion signal available without touching the site.
3. Josh exports a performance + search terms report to Drive so the agent can review real numbers
   (see below).
4. Then the tagging work at the top of `handoffs/google-ads-analytics.md`.

## Giving the agent access to the numbers

**Works today, zero setup.** The agent already has working Google Drive access — that is how the
import sheet above was found. Josh can export from the Google Ads UI straight to Google Sheets and
the agent can read it immediately. No credentials, no API, no waiting.

Most useful exports, in order:
- **Search terms report** — what people actually typed. Where wasted spend shows up first.
- **Campaign / ad group / keyword performance** — impressions, clicks, CTR, avg CPC, cost.

**Full API access** is Route B in `handoffs/google-ads-analytics.md`. The developer token
application is the long pole — Google reviews those and it can take days — so it is worth starting
early, but it is not what unblocks a first look.
