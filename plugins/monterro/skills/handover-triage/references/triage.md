# What handover triage reads

Section 6 of `product-ticket-standard.md` is the source: it tells authors what triage reads. This
file turns it into tests. Where the two disagree, the standard wins.

## 0. The code read

List every sentence in the ticket that states what the product does **today**: a default, a
limit, a behaviour, a count, and every regression scenario ("continues to", "exactly as before",
"unchanged"), because a regression scenario is a claim that today's behaviour is correct. For
each, find where the engineering repo does it and write one line: the file and the name, then
`agrees` or `contradicts`.

- A claim the code contradicts is a finding, whatever the documents say. Documents drift from the
  code and the grill only read the documents. Whether it fails test 2 is decided there.
- A regression scenario that would preserve behaviour the code shows is a defect fails test 2, and
  the note says the defect wants its own ticket.
- A claim you cannot locate is written as `not found, looked in <where>`. Not found is not a
  contradiction.
- Claims about the future, the customer, or the market are not code claims. Leave them.

## The four bounce tests, in order

The first failure decides the reason code.

### 1. `insufficient-evidence`

The grill checked that every claim carries a link. This checks that the links carry the weight
the tier puts on them.

Fails when:
- the problem rests on reports of what people say where the product already records the number
  and nobody has looked at it. "Site managers report" is not a measurement of a rate the product
  stores;
- the evidence is circular: a ticket's own discovery document, or a document that cites the
  ticket, offered as evidence;
- the outcome cannot be evaluated from anything the ticket names, and the ticket does not say who
  measures the baseline and when.

Passes, with a note, when the ticket names a thin base itself and records an exception granted
at the gate, with who granted it. A recorded exception is a decision; an unnoticed gap is not.

### 2. `fails-context-consistency`

Fails when a contradiction with the code (section 0), an ADR in the area it touches, or the
glossary's meaning of a term engineering will build to would change what engineering builds: the
problem's premise is false, part of the scope already ships, or a regression scenario would lock
in a defect. Also when the ticket asks for a change an ADR rules out and does not name the ADR.
Quote the line on both sides.

A contradiction whose fix is a sentence and changes neither the scope nor the route is not a
bounce. It goes under Code, and into Start with, where the first session corrects it. Test: would
the rewritten ticket be triaged the same way? If yes, note it. A defect the ticket leans on
without claiming anything about it is a dependency: name it, and say it wants its own ticket.

A silence is not a failure. Where the code, the ADRs and the glossary say nothing about something
the ticket asserts, note it for engineering.

### 3. `solution-space-too-open`, for the claimed tier

The tier is a claim about how well the answer's shape is known. This test asks whether the
problem is one problem.

Fails when the outcome has several distinct causes, each with a different solution, and the
ticket does not say which it addresses, or when some of those causes sit outside the ticket's
product area. The tell: you cannot write the destination of a wayfinder map in one line, because
the destination depends on which problem it turns out to be.

Passes into `wayfinder` instead when the problem and the outcome are one, the solution is open,
and the ticket says the solution is open. Open solution space with a known problem is what
`/wayfinder` is for. Open solution space with an unknown problem is still discovery.

### 4. `too-big-slice-it`

Fails when the ticket is more than one change: the Intent needs an "and", the scope spans
capabilities that could ship separately and each would move the outcome alone, or the scenarios
fall into groups that share nothing. An L legitimately spans capabilities; it fails only when a
slice could ship first and nobody has said why it should not.

## The route, when all four pass

| Grilling | Wayfinder |
|---|---|
| Acceptance scenarios statable today, including the error and the regression case | Scenarios thin, and the ticket says so and why |
| Decisions dense: roles, what the customer sees, what happens to existing data | Decisions set the constraints, and the non-goals name candidate answers as undecided |
| A design reference with precedence, or nothing to design: a defect, a list, a setting | No design, and the experience is the open question |
| Open questions a grilling session can close | Open questions that halve or double the solution space |

A missing design reference routes to wayfinder only when the experience is what is undecided. A
change with nothing to design goes to grilling without one.

**Tie-break.** If `/to-spec` could be written after one grilling session, it is grilling. If the
first session would end by listing what has to be decided before a spec, it is wayfinder. This
is the test `/wayfinder` applies to itself: no fog, no map.

An S routes to grilling. An S that looks like wayfinder is not an S; that is test 3 or 4.

## Start with

For grilling: the product questions the code read raised that a grilling session should close
first, in the glossary's words. For wayfinder: a one-line destination for the map and the two or
three decisions the ticket already names as open. Never a solution.
