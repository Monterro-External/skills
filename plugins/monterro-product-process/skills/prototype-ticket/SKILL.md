---
name: prototype-ticket
disable-model-invocation: true
description: Prototype one Product Ticket on a Claude Design canvas built from the company's design system, iterate on it, and write the chosen variant back to the ticket as its reference line. Run with a ticket ID or link; run again with the same ID to iterate, with "component" to propose a component the design system lacks, or with "pick <variant>" to settle it.
---

# prototype-ticket

Turns a ticket with an open interaction choice into something people can look at: a canvas with the screen as it is today and two or three variants, built from the company's real components. The process behind it is `standards/prototype-process.md`, route 3, bound to the design system.

Four rules:

- **A candidate, never the spec.** The prototype shows intent. The ticket's prose governs unless a user validation is named, so the reference is `illustrative`, which the ticket never needs to say.
- **Real components only.** Build every screen from the design system's mounted components and tokens. A component the system lacks is drawn as plain markup inside a box labelled `NEEDS COMPONENT · <slug>: <what>`, never styled to look shipped. The slug is short, lower case and names the existing component it extends, if any (`th-sort`, `date-range-picker`); it is how the PM refers to the gap later.
- **Invent nothing.** Labels, rules, ids, numbers and states come from the ticket, the code or its seed data. Where the ticket is silent, the artboard shows `[OPEN: <question>]`. Every placeholder is listed in the reply.
- **One ticket's change only.** Each variant changes only what the ticket is about. Anything beyond it (a setting, a second screen, persistence) is listed as parked, not drawn.

## 1. Load

From `.claude/monterro.json`: `tracker`, `context_file` and `design_system`. If `design_system.artifact` is null, say: "No design system is configured. Open Claude Code in the engineering repo, run `/design-sync`, then add its link with `/monterro-product-process:setup`." and stop. Read the system's `project/README.md` and `project/tokens.json` before anything else.

Fetch the ticket through the read tools of `tracker.kind`, with its comments and attached images. Read `prototypes/<ticket-id>.md` if it exists: it holds the canvas link and the variants so far. If it exists, go to step 5 (step 6 for `component`, step 7 for `pick`).

## 2. Qualify

Prototype only when at least one holds: the ticket has an interaction choice a reader could disagree with (where a control sits, how state is shown, what the default is); the change touches a screen more than one persona uses and the layout consequence is unclear; the signal came with a screenshot and the fix is easier to show than describe.

If none holds, say which content or rule question is open, draft the two sentences that would settle it in the ticket, and stop. Write nothing.

## 3. Map and propose

In `design_system.code`, find the route and the files that render the screen the ticket is about, the components on it, and the data it shows. Then show the PM, in at most eight lines: the screen and its source path; the open interaction choices; the variants proposed, one line each, named `A · <idea>`, `B · <idea>` (a third only if cheap); what is parked; the ticket's regression scenario (AS-3 at S). Ask once whether to build these variants or others. If nobody answers, build the proposal.

If the exploration already shows a decision anyone would argue about, say so: the ticket is an M with a PDR carrying the rejected variant, not an S.

## 4. Build

Create one canvas from the Design type, titled `<ticket-id> · <ticket title>`, and follow that type's own instructions for installing the design system and mounting its components. Artboards, in one row:

1. `Current`: the screen as the code renders it today, with data shaped like the seed data. It is what every variant is read against, and it catches library gaps before the variants do.
2. One artboard per variant, titled `Variant A · <idea>`, in its default state. Add a second state of the same variant only when the choice is about a state (sorted, error, empty).

Below each variant, a note: `AS-3: holds` or `AS-3: breaks, because <why>`. A variant that breaks AS-3 is still drawn; the PM decides.

Write `prototypes/<ticket-id>.md`: the canvas link, the date, the design system link, the variants with one line each, the placeholders, each `NEEDS COMPONENT` under its slug, the parked items, and `status: exploring`.

Reply with the canvas link, the variants, every `[OPEN]` and `NEEDS COMPONENT`, and one line on how to iterate. For each `NEEDS COMPONENT`, give its slug and the command that proposes it: `component <slug>`.

## 5. Iterate

On a rerun, read the PM's message and the canvas's open comments. Change only the artboards they touch; a new idea is a new variant artboard, never an edit that erases one. The PM may also have edited artboards by hand: keep those edits. Update `prototypes/<ticket-id>.md` with what changed and why, then reply with what changed and what is still open.

## 6. Propose a component

`component <slug>` turns one `NEEDS COMPONENT` into a proposal for the design system's owner. The slug is the one on the canvas and in `prototypes/<ticket-id>.md`. With no slug: if the ticket has one gap, use it; if several, list them with their slugs and ask which. A slug that matches none is listed back the same way. It is never added to the design system itself: the system follows the code, and the component exists once engineering ships it and `/design-sync` runs again.

1. Read the closest existing component in the design system and its source in `design_system.code` (for a sortable header: `TH`). The proposal extends it; it does not replace it.
2. Add one artboard to the canvas, after the variants, titled `Proposed component · <slug>`. Show every state the variants need, side by side and labelled: the resting state, each value of the new property (for a sort: none, ascending, descending), hover, keyboard focus, and disabled only if a variant uses it. Build it from the system's tokens only; no new colour, size or radius. Under it, one line per state saying what it means to the user.
3. Below the states, a proposed prop in the code's own style (`sort?: 'none' | 'asc' | 'desc'` on `TH`), labelled as a proposal for engineering to accept or change.
4. In the variants that used the boxed `NEEDS COMPONENT` placeholder, swap it for the proposed states, still labelled `proposed`. Any `[OPEN]` the proposal settles (a hover or focus cue) is closed and named in the reply.

In `prototypes/<ticket-id>.md`, under Needs component, record the artboard and `status: proposed`. When the PM says who approved it, set `status: approved by <name>, <date>`. Only the PM sets that; the skill never assumes approval.

Reply with the canvas link, the states drawn, the proposed prop, and who has to approve it: the design system's owner (a designer, or the engineering lead where there is none).

## 7. Pick

`pick <variant>` settles it. On the canvas, move the other variants into a second row under a `Rejected` title, each with a note on why it lost, in the PM's words when given. Never delete one. The reasons stay on the canvas: the ticket names the chosen variant only, never the rejected ones or why.

Then write the reference to the ticket. Show the PM the exact text first and write it on a yes:

- S: the `### Reference` section of the description. M: the one `**Reference**` line. L: the Experience reference section.
- Text: `Design: <canvas link> (Variant <X> · <idea>).` Nothing about precedence, the design system or layout: a reference is `illustrative` unless it says `binding`. Add `Needs component: <slug> (proposed, see artboard "Proposed component · <slug>"; <status from prototypes/<ticket-id>.md>)` for each one the chosen variant uses; engineering builds it as part of this ticket. A component with no proposal yet is listed as `Needs component: <slug> (not yet proposed)`.
- `binding` only when the PM names what was validated, with whom and when: then add `Binding: <what was validated, with whom, when>.` on the line below.

Update the description through the tracker's update tool, changing only that section. Never change state, labels or comments. Set `status: picked` in `prototypes/<ticket-id>.md`.

Close in three lines: the ticket updated, the canvas link, and the next step (`/monterro-product-process:product-grill-with-docs <ticket-id>`).
