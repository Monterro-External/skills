# Spec review checks

Sections 3 to 5 of `handover-artifact-standard.md`, restated as pass/fail rules, plus the build checks the standard implies and does not number. The fallback when the standard is not reachable in the session. Numbers are stable; the skill and its eval refer to them.

Where this file and the standard disagree, the standard wins and this file is wrong: say so in the verdict rather than grading against both.

"Requirement" below means an `FR-nnn` at M and L, and at S each of the two EARS lines: the corrected behaviour and the `SHALL CONTINUE TO` line.

## The Spec

| # | Check | Passes when | Applies |
|---|---|---|---|
| 1 | Link back | The Spec's first line links to the originating Product Ticket, and the tier matches the ticket's. A tier raised by engineering is allowed when the Spec says why. The Spec also carries the ticket faithfully: its expected behaviour and its regression scenario (AS-3) at S, or the outcome and scenarios at M and L, each appear as a requirement. A Spec narrower than its ticket fails unless it says what it leaves out and the ticket stays open for the rest. A Spec whose ticket describes a different problem fails, and that finding goes under Upstream as well | S M L |
| 2 | Tier shape | S: one EARS line for the corrected behaviour and one `WHEN <condition> THEN the system SHALL CONTINUE TO <behaviour>` line, nothing required beyond. M: sections 1 to 9 of the standard. L: M plus pre-mortem, red-team, migration and rollback, and a named decision owner | S M L |
| 3 | No implementation | Nothing from the prohibition list as a requirement: modules, services, schemas, tables, endpoints, frameworks, file or function names, implementation steps, estimates. Those belong in `design.md` or an ADR. Engineering writes the Spec with `/to-spec`, so this is where it leaks | S M L |
| 4 | Requirements | One `SHALL` each, in one of the five EARS patterns, with a stable ID never reused, and at least one Given/When/Then scenario including the error case. A tester who has never seen the code could tell whether it passed | M L |
| 5 | Non-functionals | All nine ISO/IEC 25010 rows carry a target or "N/A because". A blank row fails | M L |
| 6 | Hygiene | No vague adjective without a number (fast, scalable, secure, intuitive, robust, seamless, and kin), no unresolved placeholder, three or fewer open questions, experience precedence declared | M L |

## Traceability

| # | Check | Passes when | Applies |
|---|---|---|---|
| 7 | Coverage | Every requirement is named by at least one sub-issue of the Spec, and every sub-issue names at least one requirement. List the orphans on both sides. At S with no sub-issues, the Spec issue itself is the ticket and this passes | S M L |
| 8 | IDs, not prose | Sub-issues cite requirement IDs. A sub-issue that restates a requirement's text is a second source of truth: fails, and the fix is to replace the text with the ID | M L |

## The build

| # | Check | Passes when | Applies |
|---|---|---|---|
| 9 | A named proof | Every requirement has a test in the suite, or the Spec's section 10 verification step, that the reviewer can point at by file and name | S M L |
| 10 | The right condition | The proof exercises the requirement's own trigger or state, the `WHEN`, `WHILE` or `IF` clause, and not a neighbour of it. A requirement about the default tested only against an explicit value fails. So does one about a member's view tested through an admin, or one about a failure tested only on the success path | S M L |
| 11 | Fails without the change | Each proof of new or corrected behaviour, run from head against the base commit's code, fails. One that passes there proves nothing about this change. Exempt: a `SHALL CONTINUE TO` proof, which should pass on both, and fails check 10 instead if it never touches the condition | S M L |
| 12 | Green at head | The full suite, lint and typecheck pass at head. Name the commands run | S M L |
| 13 | No quiet weakening | Every assertion the diff deletes, loosens, or rewrites onto a different condition is listed and traced to a requirement that changed it. An untraced one fails, even with the suite green: it is how a regression ships with its test rewritten to agree | S M L |
| 14 | No drift | Every externally visible behaviour the diff changes is covered by a requirement, and nothing the diff does is excluded by the Spec's non-goals. Internal changes the observability test lets through are fine | S M L |

Two notes on judgment:

- Checks 10 and 11 are the reason the skill exists. When they disagree with a green suite, they win, and the verdict says so in those words.
- Check 13 reads the diff of the test files first, before the code. A rewritten test name ("the default policy is ..." to "a capped policy of ...") is the cheapest place to see it.
