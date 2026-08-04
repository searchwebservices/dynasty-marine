# Dynasty Marine Customs — Design System

> **Imported from Claude Design** — project `cf21adec-6eed-48be-96cb-09efe936d4f1`
> ("Dynasty Marine Customs Design System"), pulled 2026-07-28 via the `DesignSync` MCP.
> This is the local mirror of the tokens + rules. The canonical source is the Claude Design
> project; re-pull before making foundational changes so we don't fork the system.

Dynasty Marine Customs is a Sarasota, FL marine maintenance and engine-repower shop (source: [dynastymarinecustoms.com](https://www.dynastymarinecustoms.com/), a Hibu-hosted single-page marketing site — no codebase or Figma file was attached, so this system is built from the live site plus the user-uploaded screenshot `screencapture-dynastymarinecustoms-2026-07-28-14_04_52.png`). They service boats, yachts, and jet skis for owners across Sarasota, Manatee, Venice, Bradenton, and Nokomis — annual service, outdrive/outboard service, engine detailing, repowers, and marine electronics installs (Garmin, Simrad, SeaDek, etc). ABYC and NMEA certified. Run by "Josh," referenced by name in customer reviews.

## Index
- `styles.css` — root stylesheet, imports everything under `tokens/`.
- `tokens/colors.css`, `tokens/typography.css`, `tokens/spacing.css` — design tokens.
- `../dynasty-site/assets/dynasty.css` — the production implementation of this system (vanilla CSS, no React).

## Content fundamentals
- **Voice**: direct, service-trade confident — "Florida's Top Marine Maintenance & Repower Service Provider." Short benefit-driven sentences, frequent bolded phrases inside testimonials ("**been great to work** with").
- **Person**: mixed — "we/our" for the company, "you/your" addressing the boat owner directly ("Get your vessel top-quality maintenance").
- **Casing**: Title Case for headings ("Benefits of Boat Maintenance and Repower Service"), sentence case for body copy.
- **CTAs**: imperative, short — "Book My Consultation," "Submit," "Contact Us Today."
- **Trust signals over adjectives**: leads with certifications (ABYC, NMEA), star ratings (Google/Facebook/Yelp), and named-customer testimonials rather than generic superlatives.
- **No emoji.** Iconography instead (checkmarks, stars, circular badges).
- **Local + practical**: always grounds claims in FL geography ("Serving Sarasota and Manatee counties") and concrete specifics (100-hour service intervals, 48-hour estimate turnaround).

## Visual foundations
- **Color**: pure brand red (`#E4141A`, sampled from the logo) on black and white — a high-contrast, garage/workshop palette, not nautical pastel. Black is the dominant section background (hero overlay, benefits, brands bar, testimonials); white/off-white sections carry body content. A muted deep-water blue (`--water-*`) is a secondary accent for anything wanting a nautical feel without competing with the red.
- **Type**: condensed bold display face (Oswald) paired with a humanist body face (Barlow). Headlines large and tight; body text roomy (1.6 line-height).
- **Imagery**: real photography only — golden-hour boat/dock shots, aerial boat shots, cockpit/helm details. Warm, sunlit, slightly saturated water tones. No illustration, no gradients-as-decoration (gradients are only used as dark scrims over hero photography for text legibility).
- **Backgrounds**: full-bleed photographic hero; alternating black/white full-width sections (no card grids floating on gray) — long-scroll marketing page, not a dashboard.
- **Cards**: on dark sections, borderless or hairline-bordered rectangles (no drop shadow, no rounded pills) — flat, high-contrast, image-left/text-right rows for benefits.
- **Buttons**: solid rectangular (4px radius), uppercase, condensed display type, red-fill or black-fill — no pill buttons.
- **Corners**: 4–8px on cards/buttons/inputs; nothing rounded further (except small certification roundels).
- **Borders**: hairline 1px light-gray on white sections; on black sections, low-opacity white hairlines (`rgba(255,255,255,.18)`).
- **Shadows**: minimal — the source site is essentially flat/matte. Shadow tokens are for incidental elevation only (form panel).
- **Hover/press**: darker red on hover for primary buttons; no separate press treatment.
- **Transparency/blur**: only the hero dark scrim over photography. No glassmorphism.
- **Animation**: essentially none — accordion expand/collapse and a testimonial carousel. Easing minimal (standard ease, 120–200ms). No bouncy motion.
- **Layout**: centered content column (~1180px max width), generous vertical section padding, plain top header bar.

## Iconography
No icon font, no SVG icon system — plain typographic checkmarks, circular certification roundels, and star glyphs (★). No emoji anywhere. If Dynasty later needs a broader icon set, Lucide (stroke-based) is the closest-weight match.

## Intentional additions
- `Badge`, `RatingRow`: the source has ad-hoc rating/certification rows, not a formal component; generalized into a reusable pair.
- `--water-*` blue tokens: not used on the current site, sampled from its photography as a secondary nautical accent for future use (flagged, not required).

## Caveats / open asks for the client
- **No design codebase or Figma file exists** — everything is reverse-engineered from the live site's rendered HTML/CSS plus the screenshot. Hex values, spacing, and fonts are best-effort matches, not extracted source values.
- **Fonts are a substitution.** Oswald + Barlow (Google Fonts) were chosen as the closest visual match to the condensed-display / humanist-body pairing on-screen. **Confirm, or send the real font files if the site uses a licensed/custom face.**
- **Logo is a screenshot crop** (`../dynasty-site/assets/logo.png`, 318×224, plus a knocked-out `logo-transparent.png`). Ask Josh for the original vector/PNG.
- **Photography**: the design system's own `benefit-*.jpg` / `hero-boat-sunset.jpg` are crops from the same screenshot and carry upscaling artifacts. The production site uses our licensed stock instead (`boat-dock.jpg`, `yacht.jpg`, `superyacht.jpg`, `marina.jpg`) until Dynasty sends real shop photography.
- Only one product surface exists (the marketing website) — no app/portal.
