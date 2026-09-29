---

## name: interview-summary
disable-model-invocation: true
description: Write the research note for one raw interview transcript, the way a researcher writes it up the day after the conversation.
---

# interview-summary

One transcript becomes one note. You write it blind: the transcript, the guide for its round, and the house format are all you read. Other interviews from the same round are off limits, because the note must record what this person said and the round is compared later, in `interview-synthesis`.

Interviews live in the folder named by `interviews` in `.claude/monterro.json`, default `discovery/interviews/`. Inside it, one folder per research round, named `<yyyy-mm>-round-<x>-<question>`. A round's recordings sit untouched in its `transcripts/` subfolder, its written-up notes in `notes/`, and its question, guide and participant table in its `README.md`. If the config is missing, say so in one sentence and stop.

## 1. Pick the transcript

If the person named a file, use it. Otherwise list the transcripts in every round's `transcripts/` subfolder whose frontmatter says `written_up: no`, with their round, and ask which one, in one plain question. One transcript per run; if asked to do several, do the first and say how to run the next. The list is for picking only: once the transcript is chosen, the other filenames are forgotten and never appear in the note.

## 2. Read

In this order, and nothing else:

1. `README.md` in the interviews folder: the rules quotes obey (verbatim, translated, original in brackets where the wording matters; observed versus reported).
2. `README.md` in the transcript's round folder: what the round is trying to learn and the questions in the order they were meant to be asked.
3. One existing note from a different round's `notes/`, as the format exemplar. The most recent one.
4. The transcript, start to finish, before writing a word.

Never open another transcript or a note from the same round.

## 3. Listen before you write

Go through the transcript once and mark, with timestamps:

- every answer to a guide question, and which question it answers
- every moment the participant shows something rather than tells it (reads a message aloud, opens a screen, a colleague or member interrupts and the exchange is caught)
- every aside, tangent and half-answer that is about the product, however small. The note-taker's classic mistake is to keep only what answers the guide
- every number: prices, counts, dates, how often, how long
- the answer to the closing "what would you want" question, kept separate because the guide weights it lightly

Translate as you go. A quote is verbatim in the participant's language first, then rendered faithfully; you never tidy their grammar, shorten a sentence, or merge two utterances into one.

## 4. Write the note

File: the transcript's filename, in the round's `notes/` folder, beside `transcripts/`. Create the folder if it is the round's first note. Frontmatter: copy the transcript's keys and values, drop `type`, `transcription` and `written_up`.

Body, in the exemplar's order and register:

- **Heading and opening paragraph.** Who they are in two or three sentences: tenure, size, role, how they pay or what they run. Facts from the transcript only.
- **Context.** Where, how long, what was on screen, interruptions, who else was present, anything about the setting that affects how much weight the answers carry.
- **What we heard.** Grouped under short lead-ins ("On the payment run:", "On receipts:") in the order the guide asks, then the things that came up unprompted. Each quote is a blockquote, translated, followed by the timestamp in square brackets and, when the conversation was not in English, by `[Original: "..."]` with the exact words from the transcript. Every quote carries its original; that is what makes the translation checkable. A paragraph beginning **Observed.** records what you saw happen, and only that. Everything else is reported.
- **Surprises.** What you did not expect, including the asides from step 3. Three to five bullets. An aside the participant did not think was worth mentioning belongs here if it is about the product.
- **Implications.** What this one conversation suggests for the product, hedged as one conversation. Two to four bullets. No solutions unless the participant proposed one, in which case it is theirs, attributed, and last.
- **Follow-ups.** What to check, who to ask, what data to pull. Never a ticket.
- **Not covered.** Guide questions that were not asked or not answered, one line each. Omit the section only if the guide was fully covered.



## 5. Check

Before saving, confirm each of these against the transcript:

- every quote has a timestamp and the words at that timestamp translate to it, and its original is in brackets
- the note names no other transcript, interview or file. A colleague the participant mentions by name is theirs to mention; which other interview that person gave is not
- nothing in the note comes from outside the transcript, the round README or the interviews README
- observed and reported are not mixed: nothing is marked observed that was only told
- every number in the transcript that concerns the product is in the note
- the closing wish is recorded as a wish and has not become an implication

Then set `written_up: <note filename>` in the transcript's frontmatter, and in the round `README.md` replace `transcript only` on the participant's row with a link to the note.

## 6. Close

Three lines:

```
Note: <path>   From: <transcript path>   Quotes: <n, all timestamped>
Surprises worth carrying into synthesis: <one line>
Not covered: <guide questions unanswered, or none>
```

