# The System Builder

**Status: not started. Read this before writing any code for it.**

This is Josh's flagship project — the thing he most wants to exist. This document captures it in his
own words from the 2026-08-03 handoff call, so the intent survives intact and nobody has to
reconstruct it from memory.

---

## What it is, in one sentence

A customer configures a full refit of their boat online, answering questions about how they actually
use it, and gets a real itemized quote — labor and parts — without talking to anyone first.

---

## Why Josh wants it

In his words:

> *"If I can build this and it's selling my systems and my jobs and my refits overnight to my
> customers, and they're getting a labor price, and it's transparent and it's fast and accurate —
> all my leg work of me having to talk all over the phone is over."*

The measurable goal is the shape of the sales call. Today a discovery conversation runs two hours.
Josh wants the customer to arrive already configured:

> *"Hey, I worked with your system builder, I really enjoyed it, I have a few questions about some
> things. And then my conversation that used to be two hours long is now 25, 30 minutes. I'm closing
> the deal, I'm getting the payment, and I'm walking away."*

His framing of the experience: *"It's like shopping, but instead you have a system that will
literally direct you through every single thing."*

This matters more than usual right now. Josh reports the market has slowed and customers are holding
back — so the funnel needs to work harder per lead, not just bring in more of them.

---

## Start from what exists

**`website/quote.html` is the seed. Do not start over.**

Josh opened it during the handoff call and reacted:

> *"Build my estimate — this thing is just freaking awesome. This is exactly what I want my system
> to be like."*

It already implements the hard interaction parts: a multi-step wizard, a progress bar, back
navigation, single- and multi-select option cards, conditional branching, a review screen, and
state that survives navigating between steps. The branching lives in a function called `buildFlow()`
which decides what to show based on earlier answers.

The System Builder is that wizard, grown up: deeper questions, real pricing, and an output that is a
quote rather than a contact request.

---

## The flow Josh described

Sequential, as he laid it out:

**1. Contact information**
Captured up front. This is a lead-generation tool as much as a quoting tool — Josh wants to know who
is configuring, even if they never submit.

**2. Boat make and model**
Determines what is physically possible and what parts fit.

**3. What they want done**
The broad direction — repower, electronics, custom install, and so on. Maps to Dynasty's six service
lines.

**4. How they actually use the boat**
This is the part that makes it different from a generic quote form, and Josh named these
specifically:

- *What kind of fishing do you do?*
- *How often do you fish?*
- *What is your ideal day on the water?*
- *What is your budget?*

The point is that the right recommendation for someone offshore twice a month is not the right
recommendation for someone inshore every weekend. These answers should genuinely change what gets
recommended — not just get recorded.

**5. Product-specific questions**
Then, per his words, *"product specific questions for each individual thing."* Each item being
configured has its own follow-up set. A repower asks different questions than a electronics package.

**6. Choose it yourself, or ask what Dynasty recommends**
At each decision point the customer either picks, or asks for the recommendation:

> *"You can choose, or you can ask for a recommendation — what Dynasty recommends."*

This is the trust mechanism. It turns the tool from a configurator into an advisor.

**7. The quote builds, live**

> *"It builds you a quote — labor, parts, everything, right then and there."*

Itemized. Transparent. Both labor and parts, not a vague range.

**8. The customer can adjust it**
> *"You can drag and drop and change things."*

They should be able to modify the configuration and watch the price move.

**9. It lands in a back-end portal for Dynasty**
> *"On our back end we have a portal where it has their customer information, what they requested,
> what they want."*

**10. Dynasty imports it into their existing invoicing software**
The quote is not the final invoice. It gets pulled into the system Dynasty already uses for
estimates and invoicing. **Find out which software that is before designing the export** — it
determines the format.

---

## The hard part is not the interface

The wizard is the easy half, and half of it is already built. The real work is the pricing brain:

- **Labor rates**, and how labor time is estimated per job type
- **The parts catalog** — what Dynasty sells, at what price, and what fits what
- **The per-product question sets** for each service line
- **The recommendation rules** behind "what Dynasty recommends"

**None of this exists anywhere yet, and none of it can be invented.** It is Josh's domain expertise
and the entire reason the tool would be credible. A quote built on guessed numbers is worse than no
quote at all — it will produce prices Dynasty cannot honor.

### How to get it out of his head

Do not send Josh a spreadsheet to fill in. Ask him to **talk**.

Claude Code has accurate built-in voice transcription that handles long, unstructured context dumps
well — Bay demonstrated this on the call specifically so Josh would use it for this. Josh can talk
for ten minutes straight about how he prices a repower, and the agent can parse that into structure.

Suggested approach: one service line at a time. Start with the one he sells most. Get the questions,
the options, the labor logic, and the recommendation rules for that single line, build it end to
end, and let him use it. Then repeat. A tool that quotes one service line accurately beats a tool
that quotes six badly.

---

## Timeline

Josh asked whether a week was realistic. Bay's answer:

> *"I think it's possible, but it's not going to be at the level you're going to feel like you're
> going to have. I would say more like a month — and that's you've done all the testing, all the
> polishing, you've really considered it."*

**Bay reviews it for security risks before it goes live to real customers.** Send it to him before
launch, not after.

---

## Related open items

- The website forms don't submit anywhere yet — see `handoffs/gohighlevel.md`. The System Builder
  will need somewhere to send its output, and GoHighLevel is that somewhere.
- The back-end portal in step 9 may not need to be built from scratch. If GoHighLevel can hold the
  configuration data on the lead record, that might be the portal. Worth checking before building a
  second system to maintain.
