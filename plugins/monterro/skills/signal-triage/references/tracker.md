# Tracker home: where each field goes

The tracker already has properties for most of the standard's fields. Use them. The description carries only the template's sections, plus a short **Fields** block for the two or three values the tracker cannot hold, and only when they have a value.

Use only the tools for `tracker.kind`. A session can have both a Linear and a Jira connector, and both trackers can use the same key: a ticket that is not found in the configured tracker is not found, never looked up in the other one.

## Linear

Read `tracker` in the config for the team, the label names and the state names. Defaults below apply when a key is absent.

| Standard field | Linear | Config key |
|---|---|---|
| id | Issue identifier (`KEY-123`) | Never invent an id. The title has no prefix |
| title | Title | |
| type | The team's existing labels `Bug`, `Feature`, `Initiative` | `tracker.type_labels` |
| tier | Label `S`, `M` or `L` inside the label group `tier`. Never the Estimate field: tier is how much ticket the change needs, not effort, and Estimate belongs to engineering in whatever scale the team uses | `tracker.tier_label_group` |
| product_area | Label `<value>` inside the label group `area`, values from the CONTEXT.md taxonomy | `tracker.area_label_group` |
| status | Workflow state from `tracker.states`. If the team lacks it, the closest existing state, and say so in the closing note | `tracker.states` |
| owner | Assignee | |
| created, updated | Linear's own timestamps | Never written |
| signal_refs | Attachments and links on the issue. Screenshot uploaded as an attachment, referred to as "the attached screenshot" | |
| door, gate_decision, and at L outcome_metric and decision_owner | **Fields** block at the top of the description, one line each, only the ones with a value | M and L only. An S description starts at `## Intent` |

If a type label, an area label or a tier label is missing, create it (area and tier labels inside their groups) before filing; that is the one side effect allowed. Template guidance comments are never copied into the description. Anything the PM raised that the S template has no section for goes in the closing note.

## Jira

Read `tracker` in the config for the site, the project, the issue types and the status names. Defaults below apply when a key is absent.

| Standard field | Jira | Config key |
|---|---|---|
| id | Issue key (`KEY-123`) | Never invent an id. The summary has no prefix |
| title | Summary | |
| type | Issue type: `Bug`, `Feature`, and `Epic` for an initiative unless the config names another | `tracker.issue_types` |
| tier | Label `tier-S`, `tier-M` or `tier-L`. Never Story points or any estimate field: those belong to engineering | `tracker.tier_label_prefix` |
| product_area | Label `area-<value>`, values from the CONTEXT.md taxonomy. A component instead when the config says `"area": "component"` and the component exists | `tracker.area` |
| status | Status from `tracker.states`. If the project lacks it, the closest existing status, and say so in the closing note | `tracker.states` |
| owner | Assignee, resolved to an account id with the user lookup | |
| created, updated | Jira's own timestamps | Never written |
| signal_refs | Links in the description's Problem and evidence section. The MCP cannot upload files: a screenshot is named in the closing note as the one thing to attach by hand | |
| door, gate_decision, and at L outcome_metric and decision_owner | **Fields** block at the top of the description, one line each, only the ones with a value | M and L only. An S description starts at `## Intent` |

Write the description as markdown (`contentFormat: markdown`); Jira renders the headings and tables. Labels need no creating: a label exists once an issue carries it. Spell them exactly as above, since Jira labels are case-sensitive and cannot contain spaces. A missing issue type or component cannot be created through the MCP: use the closest type that exists, and say what to add in the closing note.

**Moving status.** Jira moves an issue by transition, never by setting the status field. Fetch the issue's transitions, pick the one whose target status is the name in `tracker.states`, and apply it. Match on the target status, not the transition's name: a transition can be called "In Testing" and lead to "In Handover". If no transition leads there from the current status, leave the status, say which one it is in, and name the move the PM makes by hand.

**Searching.** Use JQL scoped to the project: `project = KEY AND status not in ("<done>", "<canceled>") AND text ~ "<words>"` for duplicates, with the two names from `tracker.states` (never `statusCategory`: a team-managed project can leave Canceled in the In Progress category), `project = KEY AND status = "<triage status>"` for the triage queue. Read comments by asking for the `comment` field.
