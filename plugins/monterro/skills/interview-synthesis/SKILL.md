---
name: interview-synthesis
disable-model-invocation: true
description: Read a round's interview notes and write the synthesis report, themes carried by two or more participants. Reads notes only, never transcripts.
---

# interview-synthesis

Notes go in, one report comes out. The report answers the round's question with what the participants did and said, ranks the themes by how many independent people carry them, and keeps single-voice findings visibly apart. It creates nothing in the tracker and moves nothing.

Interviews live in the folder named by `interviews` in `.claude/monterro.json`, default `discovery/interviews/`. Inside it, one folder per research round, named `<yyyy-mm>-round-<x>-<question>`. A round's recordings sit untouched in its `transcripts/` subfolder, its written-up notes in `notes/`, and its question, guide and participant table in its `README.md`. Synthesis reports go in the folder named by `synthesis`, default `discovery/synthesis/`. If the config is missing, say so in one sentence and stop.

## 1. Scope

The person names a round (`A`, `B`) or says all. A round is a folder in the interviews folder; its letter is in the folder name. Default: the most recent round with at least one note in `notes/`. List the notes in scope by filename before reading.

Transcripts are not input. If a transcript in the round's `transcripts/` has `written_up: no`, say so, name it, and ask whether to continue without it or stop so `interview-summary` can run first. Never read the transcript to fill the gap.

## 2. Read

1. `README.md` in the interviews folder: the rules, the round table, and "What we did not cover", which becomes the coverage section of the report.
2. `README.md` in the round folder: "What we are trying to learn" is the question the report answers; the guide says what was asked and the participant table who was heard.
3. Every note in scope, fully.
4. The existing reports in the synthesis folder, for format, and so the report says what is new against them rather than repeating them.

Nothing else. Not the signals folders, not tickets, not the OST. Those are combined with this report later, by other skills and by people.

## 3. Extract, one claim per row

Build a table before clustering, in your working notes, one row per claim: the claim in one line; the note file; participant role, segment, country; **observed** or **reported**; whether it answered a guide question or came up unprompted; the quote. A claim from the closing "what would you want" question is marked as a wish.

Do not cluster while extracting. The table is the evidence the report cites.

## 4. Cluster

A **theme** needs at least two participants from different tenants, or from different roles at the same tenant, saying the same thing about the same part of the product. Two quotes from one person are one voice.

For each theme, separate:

- what people **do** (observed, or a described routine) from what they **say they want**. Behaviour outranks opinion.
- what the round **asked about** from what came up **unprompted**. A round about payments will hear about payments; the unprompted mentions are the ones to weigh.
- **capability** ("we cannot do X at the desk") from **credibility** ("it makes us look old"), when both appear. They are different problems.

Everything that is one voice goes to **Heard once**, with the quote, never promoted to a theme and never dropped.

## 5. Write the report

File: `<produced date>-interviews-round-<x>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source: interviews, round <x>`, `period`, `interviews_read`, `produced`, `by` (the person who asked for the report, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro:interview-synthesis over the round's notes`.

Length: **hard ceiling 1200 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. One paragraph and its quotes per theme. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph** on what the round asked and who was heard: roles, segments, countries, and the recruitment bias the README names.
- **Themes table**, ranked by number of independent voices: theme, voices, roles, observed or reported, prompted or unprompted.
- **One section per theme.** The finding in two or three sentences. The quotes that carry it, each as a blockquote with the note linked. What is observed versus reported. Then one line headed **Worth testing:** stating the opportunity or the question this raises, phrased as a problem to explore, never a feature to build. If a participant proposed a solution, it appears here as theirs.
- **Heard once.** One bullet per single-voice claim from the step 3 table, with its quote and note, including the ones the participant said were not worth raising and the ones that look already fixed (say so, and say it needs checking). Say which of these the next round should ask about.
- **What this synthesis cannot tell you.** Coverage gaps from the README and from the notes' own Not covered sections; who was not in the room; what the recruitment method hides.
- **Method.** Notes read, rule for a theme, what was excluded and why.

## 6. Check

- the report body is under 1200 words; state the count
- every quote in the report is in a note in scope, word for word
- every theme has two independent voices named
- every single-voice row in the step 3 table is a bullet in Heard once; count them
- nothing observed in the report was only reported in the note
- no theme's "Worth testing" line names a solution
- the report says what is new against the existing synthesis reports

## 7. Close

Three lines:

```
Report: <path>   Notes read: <n>   Themes: <n>   Heard once: <n>
For signal-triage: <themes that describe something broken or missing today>
For the OST: <themes that are opportunities, and the one that no single note shows>
```
