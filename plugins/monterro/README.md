# monterro plugin

Product-side skills for the Monterro product development process. Engineering-side is Matt Pocock's skills plugin, unchanged; this plugin produces the Product Ticket that his `grill-with-docs` and `wayfinder` pick up.

Skills graduate into this plugin from `../skills/src/` one at a time, once an eval run on the Beefcake repos passes. Every skill here follows the lean rule: one job, SKILL.md on one screen, no environment detection, reads the company's own files, config from `/monterro:setup`, and `disable-model-invocation: true` in the frontmatter so it runs only when a person types the slash command.

| Skill | Job |
|---|---|
| `/monterro:setup` | Once per product repo. Asks where tickets live and where the reference files are, writes `.claude/monterro.json`, scaffolds what is missing |
| `/monterro:idea-to-ticket` | A PM's own idea, observation or defect into a Product Ticket in the right tier's template |
| `/monterro:interview-summary` | One raw interview transcript into the research note, written blind to the rest of the round, every quote timestamped |
| `/monterro:interview-synthesis` | A round's notes into the synthesis report: themes with two or more independent voices, single voices kept apart, never a ticket |
| `/monterro:support-synthesis` | A period of the support queue into the synthesis report: themes by what the customer was trying to do, counted from a per-ticket table rather than tags, the tenants absent from each theme named |
| `/monterro:support-dig` | One hypothesis against the whole queue: every matching ticket by tenant and month, the control tenant, quotes with ids, where else to look. Writes nothing |
| `/monterro:feature-request-ledger` | A sweep of the request channel into the one living ledger: messages clustered by the ask, named accounts and claimed repeats counted apart, in-thread decisions kept, status per ask, paste-ready replies. Never a ticket |
| `/monterro:feedback-synthesis` | A period of NPS comments, app reviews and desk notes into the synthesis report: themes by intent across the operator's voice and the member's, the members who asked for nothing in their own section, never promoted |
| `/monterro:sales-synthesis` | A period of call transcripts and lost-deal debriefs into the synthesis report: themes ranked by where in the call they were raised and what they cost, market kept apart from book, losses we chose named |
| `/monterro:account-synthesis` | A period of CSM notes and escalations into the account report: one section per account, workarounds without a ticket, the renewal sheet, open commitments, shared problems ranked by ARR share and never by mentions |
| `/monterro:synthesis` | The cross-source pass over every report, the ledger and the cohorts: findings two or more independent channel families show, the ones one channel shows kept apart, the patterns nobody asked for, contradictions between reports. One more report into the same folder |
| `/monterro:signal-triage` | The Triage state and the reports' For triage lines into one embryo per problem in `backlog`, typed and tiered, signal preserved; merges into the ticket that exists, declines against a non-goal or a release note, never touches a ticket past triage. Prints the plan and waits before writing |
| `/monterro:brainstorm-ideas` | Divergence then convergence over one customer pain, ending in idea cards with their riskiest assumptions named and a discard list kept |
| `/monterro:advance-ticket` | Read a ticket, diagnose where it stands against the standard, and name and prepare the cheapest next action toward handover. Works inside discovery, never moves the ticket out of it |
| `/monterro:solution-tournament` | Rank alternative solutions for a ticket by blind pairwise judging against the ticket's own outcome, evolving the strongest candidates, ending in a favoured candidate and a validation plan. Refuses when the signals are too thin to judge against |
| `/monterro:product-grill-with-docs` | The readiness gate. Checks a ticket against section 7 of the product ticket standard (v1.9, restated in the skill's `references/checks.md` for when the standard itself is not in the session), CONTEXT.md, the decision records and engineering's glossary, then moves it to ready-for-handover or leaves it in discovery with `needs-more-info` and the failing checks named. The only skill that opens that gate |

Local development: `claude --plugin-dir ./plugin` from this folder, or from a product repo `claude --plugin-dir ../beefcake-workshop/plugin`.
