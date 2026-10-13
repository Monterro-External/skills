# Changelog

Newest first. A **Re-run setup** line means repos configured on an earlier version need `/monterro:setup` again.

## 0.8.0, 2026-10-13

- **grill-preview**, a new skill. A dry run of the engineering grill on one ticket: it builds the question tree grill-with-docs would walk against the engineering repo, answers each question with the recommended answer, and sorts every question into product, engineering or boundary with the reason. One self-contained HTML report at `grill-preview/<ticket-id>.html` shows the questions, the facts it looked up instead of asking, and what domain-modeling would have written. Nothing is asked and the ticket is not touched. No time estimate, by design.

## 0.7.0, 2026-10-13

**Re-run setup** for a repo whose tickets live in Jira.

- **Jira as a ticket home.** The tracker mapping covers Jira field by field: issue types, `tier-S/M/L` and `area-<value>` labels, statuses from `tracker.states`, JQL for duplicates and the triage queue. setup reads the project's statuses and proposes the mapping; a state the project lacks is null and named as the thing to add.
- **Status moves by transition in Jira**, matched on the target status, never the transition's name. The grill and handover-triage say so.
- **One tracker per session, by config.** The skills use only the connector `tracker.kind` names, so a session with both Linear and Jira connected never finds a same-keyed ticket in the wrong one.
- **Screenshots are attached by hand in Jira**: the MCP cannot upload files, so the closing note names the attachment.

## 0.6.0, 2026-10-06

**Re-run setup.** The config gains `channels`, `support_cli` and `standard`; `decisions` is now a folder; tier moves from the tracker's estimate to a label.

- **Works on any product, not just the sample company.** No skill assumes a repo layout, a support script, a ticket standard file or company facts. Segments, ARR shares and key accounts are read from CONTEXT.md, whose stub now has a section for them.
- **Product Decision Records.** A decision that binds beyond its ticket is written as a file in `strategy/decisions/` (`PDR-nnnn-slug.md`), and the ticket keeps a `See PDR-nnnn` line. idea-to-ticket and advance-ticket write them as `proposed`; only a person sets `accepted`. The folder replaces the single-file decision log for every skill that read it, and setup offers to split an old log.
- **Tier is a label.** `S`, `M` or `L` in a `tier` label group in Linear, `tier-S/M/L` in Jira. The estimate field is left to engineering.
- **Setup asks for ticket templates** and offers to install the bundled ones, so the first ticket no longer fails for want of a template. It asks "Where should product tickets live?".
- **Feedback channels are configured one by one** (NPS, reviews, in-product, sales, CSM, escalations), any of them absent. The support queue is read from its export files; `support_cli` keeps a query script where a repo has one.
- **The grill and advance-ticket answer in the conversation** and never comment on the ticket. The grill still moves the state and the `needs-more-info` label.
- **The product side stays out of the code.** advance-ticket and idea-to-ticket never read source code or write a root-cause hypothesis; that is engineering's after handover.
- **No second source required.** idea-to-ticket no longer asks who else sees a problem; S tickets take at most three questions.
- The grill checks against the bundled rules (product ticket standard v1.13) unless `standard` names the company's own.
