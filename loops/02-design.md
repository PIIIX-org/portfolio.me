# Loop 2 — Design

**Goal:** decide what it looks like and how it renders, and prove both before a
single line of the real site is written.

**Input:** `BRIEF.md`, specifically the translation table.

**Output:** `DIRECTION.md`, one design image per section, one runnable prototype per
technique.

`DIRECTION.md` is written in two passes. 2a writes the direction half before any
worker is dispatched, because both workers in this loop open that file as their brief.
2b appends the craft half.

Bootstrap already copied the template into the run, so the file is sitting there from
Loop 0 with `<what>` where the accent hex belongs. A worker dispatched before 2a
fills it in does not error. It reads the placeholders, invents a palette to replace
them, and returns a board that looks finished — and the invention only surfaces at
Gate B1, by which time the whole set has been rendered against it.

```text
2a DIRECTION ──────► Gate B1 ──────► 2b CRAFT ──────► Gate B2
   palette, type,     concept          technique per     motion +
   2-3 concepts,      picked           section, motion   budget
   image per section                   spec, prototypes  locked

   └ DIRECTION.md                      └ DIRECTION.md
     direction half                      craft half appended
```

---

## 2a — Direction

### Read the translation table first

Archetype pulls a **style shortlist** from [`STYLES.md`](../STYLES.md) — three
families, never one answer. Shadow archetype supplies the collision. Anti-positioning
bans styles outright; honor that absolutely. Positioning states what the hero has to
accomplish in three seconds.

### Palette

Sampled from reality (`§8`) — their logo, their product, their real work. Build out:
one accent from the sample, a substrate, a type scale of neutrals, and a state set.
Work in OKLCH so the gradients have no gray dead zone.

Art-direct **light and dark as two designs**, not one inverted (`CRAFT.md`). Some
styles collapse under naive inversion; `STYLES.md` names which.

### Type

Two families at most, one if the style earns it. Variable where the design drives an
axis. Self-hosted, subset, `font-display: swap`. Check the script renders (`§17`) —
a Persian or Arabic subject needs a face that actually draws it.

### Concepts

Top-tier work, and the conductor does it rather than a worker
([`MODELS.md`](../MODELS.md)). The collision, the opening move, and the thing nobody
expects are what `§1–3` are asking for, and a cheaper model reaches for the category
reflex those rules exist to prevent. Mention the tier once if the session is lower,
then carry on.

Generate **2–3 distinct concepts** (`§4`). Each states:

- The **collision or subversion** — this run's `§3` answer, written as one sentence.
  Which parent carries structure, which carries surface.
- The **opening move** — what happens in the first three seconds
- How **projects are presented** — invent it. Not a card grid by reflex.
- How **navigation** works
- The **section list** and order, derived from the audience's decision

Run the category-reflex check on each: if it's guessable from "portfolio for a
[category]" alone, or from category-plus-obvious-twist, throw it out. Note one you
rejected and why, so the human sees the range.

### Write the direction half of `DIRECTION.md`

Before a single worker is dispatched. This is the brief every worker in the loop reads,
and it is the reason 2a produces a file at all rather than holding the direction in the
conductor's head.

It carries what 2a decided: the palette with its sampled sources, the type system, all
2–3 concepts with their collision sentences, and the section list under each. Concepts
stay plural here. Gate B1 picks the winner, and the record of which one won and why it
beat the others is written when the craft half is appended in 2b.

### Design images

Dispatch `section-designer`, **one agent per section**. Each returns one horizontal
image of that section.

Never compress multiple sections into one board. Eight sections means eight images.
A compressed board hides exactly the detail the human needs to judge.

Two things ride along in the dispatch. Only the conductor can supply either one,
because a worker sees its own section and nothing on either side of it:

- **The composition brief** — what the neighboring sections are doing, so this one
  varies from them. Left-text-right-image is every worker's safest independent guess,
  so eight workers left to themselves return eight boards of it. Varying the set is
  the conductor's job. The worker cannot see far enough to do it.
- **The color modes for this section** — one board or two. Light and dark are
  art-directed as two designs, so a section where the difference is more than an
  inversion needs both boards, and the rest need one.

**State the fan-out before dispatching.** Concepts multiply sections: three concepts
across eight sections is twenty-four agents and twenty-four images. A conductor that
has not done that multiplication ends up doing it by accident. Say the number out loud
before dispatching.

The cheap default is the same bounded set for every concept — the opening move and one
project section — with the full section run rendered for whichever concept wins B1.
Bounding it evenly is the part that matters. Render one concept further than its rivals
and the gate stops comparing concepts and starts comparing finish. Render everything
for everyone when the human asks for it and knows the count.

> ## Gate B1 — human decision
>
> Present the rendered images per concept, the palette with its sampled source, the
> type system, and the collision sentence for each. They pick a concept, or pick
> pieces across concepts. Iterate here — it is far cheaper than iterating in code.

### When B1 rejects

How far back it goes depends on what was rejected. Diagnose that before
regenerating anything.

| What was rejected | Where it goes back to |
|---|---|
| **Execution** — wrong palette in the render, wrong crop, the image did not capture the concept | Regenerate the images with corrected direction. Same concept, same section list, same collision |
| **Concept** — the collision does not land, the opening move is wrong, projects are presented wrong | Back to 2a concept generation. New concepts, not a patch on the rejected one |
| **Strategy** — their reaction reveals the archetype, audience, or positioning was misread | Back to Gate A. Fix `BRIEF.md` first. Designing on a misread brief produces another rejection |

The third row is the one that gets missed. When the human says the images look fine
and still sounds unhappy, the brief is the suspect, not the render.

## 2b — Craft

### Assign techniques

For each section, assign a technique from [`CRAFT.md`](../CRAFT.md) that serves the
approved style. Style decides what's on the table; the technique executes it.

The test for every assignment: **name the thing this technique makes the viewer
understand about the subject.** No answer means remove it (`§7`). Impressive and
irrelevant is a failure.

### Prototype before you design around it

Dispatch `technique-prototyper`, **one agent per technique**. Each one:

1. Researches the technique freely — look up anything, pull any library via CDN
2. Builds a standalone runnable HTML proof
3. Screenshots it, measures frame rate under load
4. Builds all three states: full, designed reduced-motion, no-WebGL fallback

Prototypes live in `runs/<slug>/prototypes/`. A technique that fails here is cut
now, cheaply. The same failure in Loop 4 costs a rebuild.

### Motion spec

Write it down: easing curves, durations, stagger, scroll mapping, what triggers
what. Motion character comes from the archetype — a Ruler moves slowly and
inevitably; an Outlaw moves abruptly. Undocumented motion gets rebuilt three times.

### The performance budget

Declare it now, per `§13`:

- **Shell** — HTML, CSS, fonts, critical JS. Under 100KB. Paints something real
  alone. LCP under 1.5s.
- **Heavy layer** — shaders, 3D, physics. Lazy-loaded after first paint, gated on
  intersection, never in the LCP path. State the number.

A technique that cannot be deferred has to justify its bytes at this gate.

### Complete `DIRECTION.md`

The direction half has been on disk since 2a: palette with hexes and sampled sources,
type system, concepts, section list. Cut the concepts down to the one that won and say
why it beat the others, keep the collision and which parent is structural, then append
the craft half — section-by-section technique assignment, motion spec, both budget
tiers, **what was invented** (the thing that exists in this run and no other, `§3`),
and the prototype results including the failures.

The file is now the whole of Loop 2. Loops 3, 4, 5, and 9 open it as settled.

> ## Gate B2 — human decision
>
> Show the prototype screenshots, the motion spec, and the budget. They approve the
> technique set, or cut what doesn't earn its place. This is the last stop before
> real code.

### When B2 rejects

Cut the technique, or swap it for another from [`CRAFT.md`](../CRAFT.md), then
re-prototype. A rejected prototype never gets argued into acceptance, tuned in the
build, or carried forward on the theory that it comes together in integration.

The replacement faces the same check the original faced: name the thing this
technique makes the viewer understand about the subject. Cutting with nothing in its
place is a valid outcome — the section ships in tier 1 and the budget improves.

### A rejection is information about the brief

Both gates. Record what the rejection **revealed** in `DIRECTION.md`, alongside the
new direction: what they reacted against, and what that says about the vision,
audience, or archetype that `BRIEF.md` failed to capture. Recording only the
correction throws away the more valuable half of it. A B1 rejection goes into the
direction half as soon as it happens, where the 2b workers will read it.

**Three rejections at the same gate means the brief is wrong, not the execution.**
Stop iterating. Re-open Loop 1d, fix the synthesis, and come back through Gate A.
The fourth attempt at the same gate has never worked.

## Skip costs

| Skipping | Costs |
|---|---|
| 2a design images | Seeing it before it's built. You iterate in code instead: slower, and biased toward whatever got built first |
| Gate B1 | The concept choice. You pick; they see it at Gate C |
| 2b prototypes | Proof the technique works before the design depends on it. Late failures are expensive |
| Gate B2 | Motion review, and the budget goes undeclared |
