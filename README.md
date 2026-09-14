# Monterro product process plugin

The product-side skills for the Monterro product development process. Engineering-side is Matt Pocock's skills plugin, unchanged; these skills produce the Product Ticket that his `grill-with-docs` and `wayfinder` pick up.

This repository is a Claude Code plugin marketplace holding one plugin, `monterro`.

## Install

In Claude Code:

```
/plugin marketplace add Monterro-External/monterro-product-process
/plugin install monterro@monterro-product-process
```

Then, once per product repo:

```
/monterro:setup
```

`setup` asks where tickets live and where the reference files are, writes `.claude/monterro.json`, and scaffolds what is missing. Every other skill reads that file.

## The skills

| Skill | Job |
|---|---|
| `/monterro:setup` | Once per product repo. Where tickets live, where the reference files are |
| `/monterro:idea-to-ticket` | A PM's own idea, observation or defect into a Product Ticket in the right tier's template |
| `/monterro:brainstorm-ideas` | Divergence and convergence over a customer pain, ending in idea cards with named riskiest assumptions |
| `/monterro:interview-summary` | One raw transcript into the research note, written blind to the rest of the round, every quote timestamped |
| `/monterro:interview-synthesis` | A round's notes into the synthesis report: themes with two or more independent voices |
| `/monterro:support-synthesis` | A period of the support queue into the synthesis report, counted by reading rather than by tag |
| `/monterro:support-dig` | One hypothesis against the whole queue. Writes nothing |
| `/monterro:feature-request-ledger` | The request channel into the one living ledger. Never a ticket |
| `/monterro:feedback-synthesis` | NPS, app reviews and member feedback, the operator's voice kept apart from the member's |
| `/monterro:sales-synthesis` | Call transcripts and lost deals, market kept apart from book |
| `/monterro:account-synthesis` | CSM notes and escalations, one section per account, weighted by ARR |
| `/monterro:synthesis` | The cross-source pass: what two or more independent channels show |
| `/monterro:signal-triage` | Triage and the reports' For triage lines into one typed, tiered embryo per problem |
| `/monterro:advance-ticket` | Diagnose where a ticket stands and prepare the cheapest next action toward handover |
| `/monterro:solution-tournament` | Rank alternative solutions by blind pairwise judging against the ticket's own outcome |
| `/monterro:product-grill-with-docs` | The readiness gate: pass a ticket to ready-for-handover, or leave it in discovery with the failing checks named |

## How these are built

One job per skill, SKILL.md on one screen, no environment detection, reads the company's own files, configuration from `/monterro:setup`. Every skill carries `disable-model-invocation: true`, so it runs only when a person types the slash command and never because the model thought it fit.

A skill enters this plugin only after an eval run against a reference product passes.
