# booknotes — a two-way reading companion for technical books

A small setup that turns [Claude Code](https://claude.com/claude-code) into a **study
partner** for dense technical books. You read, raise a doubt, and Claude discusses it with
you — one idea at a time, with an analogy or diagram — until the concept actually clicks.
Then you save a structured note. Over time each book grows a clean, topic-indexed set of
notes that captures *the explanation that made it land*, not a lecture dump.

It's just Markdown files plus two Claude Code skills. No app, no server, no database.

> **No books are included.** This repo ships the *system*, not the texts. You bring your
> own legally-obtained `.epub` for each book. Epubs are git-ignored on purpose.

---

## What you get

- **`/doubt <ch#> <question>`** — open a discussion about something you don't get in the
  current book. Claude reads your existing notes first (so it won't repeat itself), logs
  the doubt as *in-flight*, then genuinely talks it through with you.
- **`/note [title]`** — once a doubt is clear, save it. Claude consolidates the discussion
  into the right chapter file, merges it into an existing topic section if there is one,
  and updates the book's topic index and progress tracker.
- **Durable memory across sessions.** Each book's `notes/PROGRESS.md` is the source of
  truth for where you are and what's still open, so you can stop mid-book and resume later
  even after the live conversation is gone.

Notes are **stored chapter-wise** (one file per chapter) and **navigated topic-wise**
(an auto-maintained `INDEX.md`).

---

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code) installed and working.
- Git.
- A legally-obtained `.epub` of each book you want to study.

---

## Setup

```bash
git clone https://github.com/akshatbaranwal/booknotes.git
cd booknotes
```

Open the folder in Claude Code. The skills in `.claude/skills/` and the conventions in
`CLAUDE.md` are picked up automatically — there's nothing to install.

### Add a book

1. Make a top-level folder for it (short slug), e.g. `ddia/` or `systemperformance/`.
2. Drop your own epub inside it, e.g. `ddia/your-book.epub`. It stays local — git ignores it.
3. Create the notes scaffold:

   ```bash
   mkdir -p ddia/notes
   ```

4. Seed `ddia/notes/PROGRESS.md` with your starting position and a chapter checklist
   (copy the shape from an existing book's `PROGRESS.md`). That's the only file you need
   to hand-write; `INDEX.md` and the chapter files get created for you as you take notes.

That's it — start reading and run `/doubt`.

---

## How a session flows

```
/doubt 1 what does "data-intensive" actually mean?
   → Claude resolves the active book, reads your ch01 notes + PROGRESS.md,
     logs the doubt as in-flight, and starts a back-and-forth discussion.

   ... you go back and forth until it clicks ...

/note                      # or: /note what "data-intensive" means
   → Claude writes the cleared doubt into ddia/notes/ch01-*.md,
     updates INDEX.md and PROGRESS.md, and clears the in-flight entry.
```

**Which book am I in?** Claude resolves the "active book" from each book's `PROGRESS.md`
(the current-position line). If it's ambiguous — more than one book, nothing in progress —
just say the book name or make the topic obvious, and Claude will ask rather than guess.

---

## Layout

```
<book>/                      e.g. ddia/, systemperformance/
  notes/PROGRESS.md          position, in-flight doubts, backlog, journey plan  ← read first
  notes/INDEX.md             topic-wise index (auto-maintained)
  notes/chNN-<slug>.md       chapter notes; topic sections inside
  <book>.epub                your own copy — git-ignored, never committed
.claude/skills/              the /doubt and /note skills (shared across all books)
CLAUDE.md                    how Claude should behave as a study partner
```

---

## Design principles

- **A doubt isn't safe until it's `/note`d.** Save cleared doubts before ending a session —
  the live conversation doesn't survive, the files do.
- **Learn by intuition + the right analogy, one idea at a time** — not walls of text. When
  a note is saved, the analogy that closed the doubt is the thing worth preserving most.
- **Zero-friction filing.** A note always goes to the chapter the doubt was raised in, so
  you never stop to decide where it belongs.

---

## License

The notes and this setup are yours to reuse. Book contents are **not** included and remain
the property of their respective publishers — bring your own copy.
