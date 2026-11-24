# Interview questions

Ask only what the PM has not already said. One question per message, in this order, wait for each answer. Each entry is a topic: the first part is what you ask, anything after "and" or "Infer" is filled in as a guess, never asked in the same message.

## S (at most three)

1. **Where and when**: which surface, which role or customer account, which conditions. For a missing capability, the steps that end where the user gets stuck.
2. **Expected**: what should happen instead, in the user's terms. Infer the current behaviour from the PM's first message.
3. **Acceptance scenarios**: the happy path, the error case, and the one neighbouring behaviour that must keep working (the regression), each as Given / When / Then. Propose all three from context; the PM confirms or corrects.

## M and L (at most five, in this priority)

1. **Who**: segment or persona with the underlying problem. Infer how they cope today.
2. **Evidence**: what exists now (support, sales calls, research, usage) or nothing yet.
3. **Outcome**: what customers would do differently. Infer what that ladders to from the OKRs in CONTEXT.md.
4. **Non-goals**: what a reader would assume is included and is not.
5. **Door**: anything on the L list in step 3.

For L, also note that a named decision owner and the migration and rollback intent are needed before the gate. Placeholders in the ticket, not questions now.
