# Loop 5 — Share & Convert

**Goal:** make the link look like something when it travels, and give the reader a
way to act when they decide to.

**Input:** the built site, `BRIEF.md`, `DIRECTION.md`.

**Output:** OG image, meta, structured data, favicons, a working contact path, and
— optionally — `cv.pdf`.

---

## Why this exists

Portfolios travel by link. Pasted into Slack, DMed to a hiring manager, posted on
LinkedIn. **The unfurl is the first impression** — it lands before the site does,
and it decides whether the link gets clicked at all.

And every portfolio has a job. Someone reads it, decides to act, and finds nothing
to act with. That is the whole pipeline wasted at the last inch.

Both are cheap. Both are usually missing.

## The OG image

Generate it with the same craft rules as the site (`§10`). **Not a screenshot** — a
screenshot at 1200×630 is unreadable and says nothing.

Design it: the name, the positioning line from `BRIEF.md`, the brand color, and one
element of the site's visual language so the unfurl and the site read as one thing.
1200×630, under 300KB, PNG. Test it dark and light — some clients composite on
their own background.

If the subject's photo belongs in it, use their real one (`§22`). Never generate a
face.

## Meta

```html
<title>Name — the positioning in five words</title>
<meta name="description" content="One sentence. What they do, who for.">

<meta property="og:title" content="…">
<meta property="og:description" content="…">
<meta property="og:image" content="https://…/og.png">
<meta property="og:url" content="https://…">
<meta property="og:type" content="website">

<meta name="twitter:card" content="summary_large_image">
```

Absolute URLs on `og:image`. Relative paths silently fail in every unfurl.

## Multi-page

Every page carries its own `title`, `description`, `og:title`, `og:description`,
`og:image`, and `rel="canonical"`. A case study that unfurls as the homepage wastes
the click, and a citation is the whole reason it has its own URL
([`ARCHITECTURE.md`](../ARCHITECTURE.md)).

Generate per-page OG images at build time from the page title, using the site's
visual language. One template, N images, no manual work per post.

A blog also ships an RSS feed and a sitemap. The feed is the first thing writers ask
for and it costs nothing. Drafts appear in neither.

A feed that only exists as a `<link rel="alternate">` tag is invisible to
almost everyone who isn't already using a reader. A real, visible link
somewhere in the page chrome — the footer is the conventional place — makes
it findable by a human, not only a browser extension. The feed icon is a
platform-logo case, not a brand-carrying one, per `CRAFT.md`'s icon split:
pull it from the same icon set the rest of the page's functional icons
come from, recolored to the accent token, never regenerated from scratch.

## Structured data

`Person` schema as JSON-LD: name, `jobTitle`, `url`, `sameAs` for their real
profiles, `knowsAbout`. Every field sourced from `EVIDENCE.md` (`§5`).

## Favicons

SVG favicon, 180px apple-touch-icon, `manifest.json` with theme color. Derived from
the site's visual language, not a letter in a circle by reflex — usually the
smallest legible fragment of the wordmark, or the accent shape reduced to its
plainest silhouette. Test it at 16px, where the browser tab actually renders it;
anything that only reads at favicon-preview size is not a favicon.

## The CV

Optional — ask. Not every subject wants a downloadable résumé; an agency site
or an inbound-only freelancer often doesn't. When they do, it's a second render
of material this pipeline already has, not new material: `EVIDENCE.md`'s proof
ladder and `BRIEF.md`'s positioning and project list, condensed from the case
studies Loop 3 already wrote rather than re-derived from scratch. Same rule as
everywhere else — every line traces to a source, `§5` still applies.

**Ask which it's for**, because the honest answer changes the design:

- **Styled to match the site** — carries the visual identity, reads as one
  thing with the portfolio. Right for a design-forward audience who will open
  it as a PDF and look at it.
- **Plain and parser-safe** — standard structure, no columns, no icons-as-text,
  nothing an ATS mangles. Right when the CV's first reader is software, and a
  human sees it only after it survives that pass.

When unsure which the audience needs, default to plain — a CV that never
reaches the human because a parser choked on it cost more than losing some
personality would have.

Author `cv.md`, render to PDF (`make-pdf` if available, or any HTML-to-PDF
capability the platform has), and publish it at a stable, predictable URL —
`/cv.pdf` at the root, not buried in a path that changes across runs — so the
résumé link below has something durable to point at. On a re-run that updates
a claim (`loops/09-rerun.md`), regenerate it in the same pass; a CV that quietly
drifts out of sync with the site is its own kind of rot.

## The accessibility statement

Optional, and a different question from `§12` itself — `§12` is HARD and
applies whether or not this page exists. The statement is a public, dated
claim about conformance and known limitations, not a substitute for actual
compliance.

Ask, and default to no, for the same reason as the CV: most subjects don't
need one, and a stale statement — a conformance claim nobody re-checked in
two years — is worse than none, per `loops/09-rerun.md`'s rot check. It
clearly earns its place when accessibility, UX research, or inclusive
design is part of the subject's own expertise; there it is evidence
(`§5`, `BRAND.md`'s proof ladder), not a courtesy page. State the
conformance target honestly, list what's actually still unresolved rather
than claiming a clean bill, and date it so a visitor can judge how current
the claim is.

## The voice intro

Optional, and a different kind of ask than the CV or the accessibility
statement: those are utility and evidence, this one is exposure. Ask, and
default to no — a voice clip means a stranger hears them before they've
read a word, and not every subject wants that.

When they do, it's `loops/01-substance.md`'s recorded-intake practice
surfaced rather than kept internal: a real recording of them speaking,
never a synthesized clone reading a script in their name — a cloned voice
standing in for them is a fabrication in the same family `§5` already
bans everywhere else. Reuse an actual moment from the interview if
they're comfortable with it going public, or record a short, separate
take if they'd rather keep the interview private and the site's clip
distinct from it. Either way it is their unrehearsed voice, not a
read-aloud pass over copy Loop 3 already wrote for the page.

A few seconds to perhaps thirty — long enough to hear who they are,
short enough that it never competes with the work `§7` says should be
the hero. Placed beside identity, wherever the name and photo already
live, not a standalone section of its own. Self-hosted per `§9`, native
controls, no autoplay with sound, and a transcript alongside it exactly
as `loops/04-build.md`'s video-and-audio section already requires for
anything carrying information — the transcript is what makes the clip
accessible rather than decorative.

## The conversion path

**Coupled to the deploy target.** A static host cannot take a form POST without a
third party; a VPS can. Pick from what the target actually supports:

| Target | Options |
|---|---|
| GitHub Pages, cPanel, S3 | `mailto:`, Formspree, Tally, or a link out |
| Netlify | Netlify Forms, native |
| Vercel | Serverless function |
| Cloudflare Pages | Worker |
| VPS (nginx or Docker) | A real endpoint. Rate-limit it |

Whatever it is, it gets tested end to end before Gate C. An untested contact form is
worse than a `mailto:` — it fails silently and nobody knows.

**The ask matches the audience.** A hiring manager gets `/cv.pdf` and an email, if
the CV above was built. A client gets a scoped inquiry. An investor gets a
calendar link, below. One clear action, derived from the decision named in
`BRIEF.md`.

## The booking calendar

Optional, ask-first, the same pattern as the CV and the newsletter: most
subjects don't want their calendar exposed to a stranger, and the ones who
do are usually the audience the conversion path above already names — an
investor, a consulting client, anyone whose actual next step is booking
time rather than sending an email.

**Self-hosted preferred, per `§9`.** Cal.com self-hosted over an embedded
Calendly — the same instinct already governing fonts and libraries applies
to a scheduling tool: an embedded third party's iframe ships its own
script and, with Calendly specifically, its own tracking the moment it
renders, exactly what `scripts/preflight.py`'s tracker check exists to
catch. Self-hosting keeps the booking data with the subject rather than a
vendor, and it's a small enough service that a subject already comfortable
self-hosting their own site can usually run it too.

**The embed is the fallback, not the default** — the same escape valve
already used for video and audio: when self-hosting genuinely isn't
feasible, a privacy-respecting embed is the honest fallback, and which one
ran gets said in the run report exactly as it does there.

**One clear ask, not a page of its own.** It lives where the conversion
path above already puts the calendar link, beside the contact ask matched
to the audience, never a dedicated route competing with the actual work
for attention.

## Legal minimum

Only if something is collected. A form means an EU visitor makes an imprint and a
privacy line a legal requirement, not a preference. Skip entirely when nothing is
collected — that is the common case and it needs no page.

A privacy line and a cookie-consent banner are not the same requirement. The
privacy line is informational — what's collected, why, where it goes — and a
contact form alone earns one regardless of cookies. A consent banner is a
heavier, interactive requirement that only applies once a non-essential cookie
actually gets set, and following `loops/04-build.md`'s own analytics
preference order (the host's own server-side analytics, or a cookieless tool)
usually means one never does. Build the banner only when the subject insists
on something that sets one — `loops/04-build.md` has the spec.

The indexing decision, `robots.txt`, `sitemap.xml`, and structured data beyond
`Person` belong to [`loops/06-seo.md`](./06-seo.md), which runs right after this
one.

## Skip cost

Skipping the share layer means the best work in the pipeline shows up in Slack as a
gray box with a URL. Skipping the conversion path means someone decides to reach out
and finds no way to. These are the two cheapest steps here and the two most
expensive to have left out.
