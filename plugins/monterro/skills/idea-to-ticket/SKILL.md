---
name: idea-to-ticket
disable-model-invocation: true
description: Turn a PM's own idea, observation or defect report into a Product Ticket in the template for its size. The single entrance for anything a PM brings themselves, any type (bug, feature, initiative) and any tier (S, M, L). Use when a PM says they have an idea, noticed something wrong or missing, wants to ticket something, or pastes a rough idea or a screenshot to structure. Not for a queue of customer signals (signal-triage) and not for checking an existing ticket (grilling).
---

# idea-to-ticket

The PM says what they want or what they saw. You size it, ask only what is missing, and write the ticket in the company's template for that tier. Small things stay small: an S ticket can be complete after one or two questions.

The ticket lives in exactly one place, named by `ticket_home` in `.claude/monterro.json`: a markdown file in `tickets_dir`, or an issue in the tracker. The other place does not exist for this skill. When `ticket_home` is a tracker, never list, read, search, dedup against or relate to files in `tickets_dir`; the only files read from that folder are the templates named in `templates`. If the config is missing, say so in one sentence and stop.

## 1. Read

The config, then `context_file` (product areas, non-goals, what already ships), the tier templates, and the existing tickets in the ticket home only: the tracker team's issues when `ticket_home` is a tracker, the files in `tickets_dir` when it is files. Never both. If `glossary` resolves, read it; it is the only source of vocabulary. Read nothing else outside this repo.

## 2. Preserve

Keep the PM's words verbatim, with name and date, under **Original signal (verbatim)**. A pasted screenshot is part of the signal: attach it to the tracker issue, or save it as `tickets_dir/attachments/<id>-1.png` for a file ticket, and describe it in one line.

## 3. Size, then type

Say a provisional tier, type and the one-line reason before asking anything, worded as provisional ("sizing as S for now"), and let the PM correct you. The tier picks the template and the question budget; it is confirmed in step 6, once the answers are in.

- **S**: one sentence describes it and no product decision anyone would argue about. Defect, copy, default, a sort control, a missing column.
- **M**: a capability change with real product decisions in it (permissions, gating, existing customers, failure messaging), reversible.
- **L**: one-way door (pricing, platform, contracts, migration) or several capabilities.

Type is independent: **bug** if the product fails to do what it claims, **feature** if something is missing, **initiative** for L-sized multi-capability change.

If the idea is a stated non-goal in CONTEXT.md, cite it and stop; changing CONTEXT.md is the CPO's conversation.

## 4. Ask one question at a time

From `references/questions.md`, only what the PM has not already said, in the file's priority order. One question per message, in chat, plain text: ask it, stop, wait for the answer. Never a numbered list of questions, never two in one message. The answer may settle later questions, so re-check the list before each one. Budget: S at most four questions, M and L at most five. If the PM says "skip" or "enough", stop asking. Unanswered questions become guesses for the review in step 6, never blockers.

## 5. Dedup

Search the ticket home on the problem, not the feature name (tracker: search title and description across all states except done and cancelled; an empty or near-empty tracker means no matches, never a reason to look at files). Show up to three matches and let the PM choose: merge, relate, or new.

## 6. Review with the PM

Nothing the PM did not say goes into the ticket unreviewed. Print the complete draft as plain text, in the ticket's own order, so the PM reads it as the ticket it will become. Inside it, mark every sentence that is not the PM's: `[1 from: CONTEXT.md vocabulary]` when it comes from a file you read, `[2 guess]` when it fills a gap with no source. Numbered in sequence. Unmarked text is the PM's own.

Open the review by confirming the tier: repeat it with its one-line reason, or upgrade it when an answer exposed a product decision anyone would argue about (permissions, gating, existing customers, failure messaging) or a one-way door. An upgrade means switching to that tier's template before printing the draft. Then the draft, then one plain question, in chat, not the structured question tool: "Reply with numbers to drop, edits in your words, and anything missing. Reply 'ok' to keep all." Nothing is dropped unless the PM names it. Apply the reply, and if the edits were substantial show the changed sections once more; otherwise write.

Dropped items are dropped, not parked. Edited items carry the PM's wording. The markers exist for this review only: the written ticket carries none of them, no source parentheticals, no "from CONTEXT.md", no "guess". Evidence links and the verbatim signal are the only provenance a ticket carries.

If the session is unattended and no review is possible, do not create the ticket: output the marked draft and stop.

## 7. Write

The body is the template's sections for the tier, nothing more and nothing above the first heading. Vocabulary from the glossary: use the existing terms, never define or cite them in the ticket, and never rename one. When the idea needs a term the glossary lacks, or needs an existing term to mean something new, keep the PM's word and list it in the closing note. No implementation detail stated as a requirement anywhere; where an idea is worth keeping it is one line marked non-binding. Status: always `backlog`, whatever the tier and however complete the ticket is. The skill never sets `discovery`; a human does, and the closing lines say whether to.

For M and L, split the idea into the problem it implies and the solution it proposes. The problem drives the ticket. The solution stays as a non-binding leading-candidate line at the end of the ticket, unless it is genuinely decided (a contract names it), in which case it is a decision with its rationale. That line holds only what the PM proposed; never add ideas of your own.

An M or L at intake is an embryo and should read like one: the decisions section holds only the calls actually made (usually zero to two), Acceptance scenarios sketch the main path and the error case, and an L's success criteria are marked as hypotheses. Sections that discovery will fill stay short or empty and say so.

**File home**: copy the template, keep every frontmatter field, fill what you know, leave the rest null. Next id in sequence, filename `PREFIX-nnnn-slug.md`.

**Tracker home**: the tracker holds the fields, the description holds the sections. See `references/tracker.md` for the mapping. Never paste frontmatter, a field table, an id, or a "source of truth" line into the description; the tracker's own identifier is the id and the title carries no prefix.

## 8. Check and close

File home: run `validator` if configured, fix what fails, run again. Tracker home: check the body against `references/checklist.md` yourself. Then four lines:

```
Created: <path or issue link>   Type: <t>   Tier: <T>   Status: <s>
Change or problem: <one line>
Missing before grilling (S) / before the gate (M, L): <the gaps, or "nothing: ready to grill, recommend moving it to discovery">
Glossary gaps: <terms the glossary lacks or that need a new meaning, or none>
```
