---
name: note
description: Save a cleared reading doubt as a structured note. Usage: /note [optional topic title]. Resolves the active book, consolidates the current in-flight discussion, files it under the active chapter's markdown (merging into an existing topic section if present), and updates that book's INDEX.md and PROGRESS.md. Invoke when the user says the doubt is cleared / to save / take a note / store it.
---

# /note — save a cleared doubt into chapter notes

Consolidate the discussion that just resolved and file it. The active chapter + topic
come from the most recent in-flight doubt; don't make the user re-type them.

## Resolve the active book first

This repo has one folder per book (`ddia/`, `systemperformance/`, ...), each with its
own `notes/`. Determine the **active book** from the working directory / conversation
(same book as the `/doubt` that opened this thread). All paths below are relative to
that book folder.

## Steps

1. **Find the active topic + chapter.**
   - Read `<book>/notes/PROGRESS.md`; the relevant **In-flight doubts** bullet gives
     the chapter (`[chN]`) and topic title.
   - A topic title passed as an arg wins. If PROGRESS has no in-flight bullet (context
     was summarized), infer chapter + topic from the recent discussion and confirm in
     one line before writing.

2. **Resolve the chapter file** — `<book>/notes/chNN-<slug>.md` (zero-pad to 2 digits,
   e.g. `ch05-replication.md`).
   - Exists → read it.
   - Missing → create with a header:
     ```
     # Chapter N — <Chapter Title>

     > Notes from *<Book Title>*. Topic sections below; discussions merged in place.

     ---
     ```
     Pick a sensible slug/title from the topic; if unsure of the chapter's real title,
     use the topic as the slug and move on — don't block.

3. **Consolidate into the topic template.** Pull the substance from the discussion
   since the doubt was opened — especially the last exchanges that closed it. The
   winning analogy/diagram goes at the **top**; it's the rereading hook.

   ```markdown
   ## <Topic title>   {#<anchor-slug>}
   **Status:** Clarified · **Ref:** §X.Y · **Date:** <today> · **Tags:** #tag #tag

   ### 💡 What made it click
   <the analogy / ASCII or ```mermaid diagram / reframing that closed it>

   ### My doubt
   <the original confusion, in the user's words, brief>

   ### Resolution
   <the tight consolidated explanation>

   ### One-liner to reread
   <compressed takeaway>

   ### Related
   [[chNN-...#anchor]] · <cross-links, if any>
   ```
   Preserve any Mermaid/ASCII diagram verbatim. Keep prose tight — this is for
   rereading, not a transcript.

4. **Merge, don't duplicate.** If a section for this topic already exists (match by
   title/anchor), fold the new discussion into it: update the resolution, add the new
   analogy under "What made it click" if better, refresh the date. Only create a new
   section when the topic isn't there yet.

5. **Update `<book>/notes/INDEX.md`** — add/update the topic's row:
   `| <Topic> | N | [link](chNN-<slug>.md#<anchor>) | #tags | Clarified |`. Remove the
   placeholder `_(none yet)_` row if present. Add any new tags to the tag vocabulary.

6. **Update `<book>/notes/PROGRESS.md`** — remove this doubt's bullet from **In-flight
   doubts** (it now lives in notes + index). Leave **Current position** as is.

7. **Report back** in one or two lines: which file + section was written/merged and any
   cross-links added. Don't paste the whole note back.
