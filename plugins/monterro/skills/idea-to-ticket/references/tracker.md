# Tracker home: where each field goes

The tracker already has properties for most of the standard's fields. Use them. The description carries only the template's sections, plus a short **Fields** block for the two or three values the tracker cannot hold, and only when they have a value.

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

Same mapping with Jira's issue type for `type`, a `tier-S`, `tier-M` or `tier-L` label for `tier`, components or a label for `product_area`, status for `status`, assignee for `owner`, attachments for `signal_refs`, and the same Fields block for the rest.
