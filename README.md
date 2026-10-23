# Monterro product process plugin

The product-side skills for the Monterro product development process. This repository is a Claude Code plugin marketplace holding one plugin, `monterro-product-process`.

## Install

In Claude Code:

```
/plugin marketplace add Monterro-External/skills
/plugin install monterro-product-process@monterro-product
```

Then, once per product repo:

```
/monterro-product-process:setup
```

`setup` asks where tickets live and where the reference files are, writes `.claude/monterro.json`, and scaffolds what is missing. Every other skill reads that file.

## The skills


| Skill                               | Job                                                                                                                                                                                                                                                                            |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `/monterro-product-process:setup`                   | Once per product repo. Where tickets live, where the reference files are                                                                                                                                                                                                       |
| `/monterro-product-process:idea-to-ticket`          | A PM's own idea, observation or defect into a Product Ticket in the right tier's template                                                                                                                                                                                      |
| `/monterro-product-process:brainstorm-ideas`        | Divergence and convergence over a customer pain, ending in idea cards with named riskiest assumptions                                                                                                                                                                          |
| `/monterro-product-process:interview-summary`       | One raw transcript into the research note, written blind to the rest of the round, every quote timestamped                                                                                                                                                                     |
| `/monterro-product-process:interview-synthesis`     | A round's notes into the synthesis report: themes with two or more independent voices                                                                                                                                                                                          |
| `/monterro-product-process:support-synthesis`       | A period of the support queue into the synthesis report, counted by reading rather than by tag                                                                                                                                                                                 |
| `/monterro-product-process:support-dig`             | One hypothesis against the whole queue. Writes nothing                                                                                                                                                                                                                         |
| `/monterro-product-process:ask-product`             | One question about how the product works, answered from the product's own sources with a verdict, a confidence and a citation on every claim. Reads only, files nothing                                                                                                        |
| `/monterro-product-process:feature-request-ledger`  | The request channel into the one living ledger.                                                                                                                                                                                                                                |
| `/monterro-product-process:feedback-synthesis`      | NPS, reviews and in-product feedback, the customer admin's voice kept apart from the end user's                                                                                                                                                                                        |
| `/monterro-product-process:sales-synthesis`         | Call transcripts and lost deals, market kept apart from book                                                                                                                                                                                                                   |
| `/monterro-product-process:account-synthesis`       | CSM notes and escalations, one section per account, weighted by ARR                                                                                                                                                                                                            |
| `/monterro-product-process:synthesis`               | The cross-source pass: what two or more independent channels show                                                                                                                                                                                                              |
| `/monterro-product-process:signal-triage`           | Triage and the reports' For triage lines into one typed, tiered embryo per problem                                                                                                                                                                                             |
| `/monterro-product-process:advance-ticket`          | Diagnose where a ticket stands and prepare the cheapest next action toward handover                                                                                                                                                                                            |
| `/monterro-product-process:solution-tournament`     | Rank alternative solutions by blind pairwise judging against the ticket's own outcome                                                                                                                                                                                          |
| `/monterro-product-process:grill-preview` | A dry run of the engineering grill: the questions grill-with-docs would ask, each bucketed product, engineering or boundary, in one HTML report. Asks nothing, changes nothing |
| `/monterro-product-process:product-grill-with-docs` | The readiness gate: pass a ticket to ready-for-handover, or leave it in discovery with the failing checks named. Grades against product ticket standard v1.10                                                                                                                  |
| `/monterro-product-process:handover-triage`         | The second gate, engineering's: reads a ticket at ready-for-handover against the engineering repo's code, ADRs and glossary, then hands it over on the grilling or the wayfinder route, or returns it to discovery with one reason code. Waits for the engineer before writing |
| `/monterro-product-process:spec-review`             | Reviews a Spec issue against the Product Ticket behind it, and runs each requirement's test against the base commit, so a suite that passes for the wrong reason fails the review. Fixes nothing, moves no state                                                               |


