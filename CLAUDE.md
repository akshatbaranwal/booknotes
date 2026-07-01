# Booknotes — reading-companion repo

This repo holds my study notes for technical books. Claude acts as a **two-way study
partner**: I read, raise doubts, we discuss until a concept truly clicks, then save a
structured note. One top-level folder per book.

## Layout

```
<book>/                       e.g. ddia/, systemperformance/
  notes/PROGRESS.md           position, in-flight doubts, backlog, journey plan  ← read first
  notes/INDEX.md              topic-wise index (auto-maintained)
  notes/chNN-<slug>.md        chapter notes; topic sections inside
  <book>.epub                 source text (committed)
.claude/skills/               shared skills, auto-discovered from any book folder
```

## Skills

- **`/doubt <ch#> <question>`** — open a discussion about a doubt in the current book;
  logs it as in-flight in that book's `PROGRESS.md`, then we discuss.
- **`/note [title]`** — consolidate the cleared discussion into the chapter file
  (merging into an existing topic section), and update the book's `INDEX.md` / `PROGRESS.md`.

## Conventions

- **Notes are stored chapter-wise, navigated topic-wise** (chapter files are the store;
  `INDEX.md` is the topic view). Filing is zero-friction: a note always goes to the
  chapter the doubt was raised in.
- **`<book>/notes/PROGRESS.md` is the durable source of truth** — read it at the start
  of any session to recover where I am and what's open. The live conversation does not
  carry across sessions.
- The user learns via intuition + the right analogy/diagram, one idea at a time — not
  lecture dumps. When saving a note, the analogy that closed the doubt is the thing to
  preserve most.
- A doubt isn't safe until it's `/note`d; before ending a session, save any cleared-but-
  unsaved doubt.
