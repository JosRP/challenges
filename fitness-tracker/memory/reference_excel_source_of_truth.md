---
name: excel-source-of-truth
description: Growth V3.xlsx on Google Drive is the source of truth for all logged data. CLI is read-only/import-only.
metadata:
  type: reference
---

`H:\My Drive\8 - Fitness\Growth V3.xlsx` is the source of truth for all fitness data Jose logs. Sheets: `Chest`, `Weight`, `Run`. Each sheet starts at row 2 with headers, data from row 3 onward.

The fitness.py CLI is read-only / import-only:
- `python fitness.py sync` reads the Excel, upserts to `fitness.db` (SQLite at project root) by date.
- `python fitness.py init` is one-shot at setup time.
- `python fitness.py goal-check` queries the SQLite store and shows green/red status with 7-day staleness flags.

**Logging discipline:**
- Log working-set PRs or near-baseline working weights ONLY.
- Do NOT log deload reps or active recovery sessions in the Excel - they degrade goal-check display without representing real capacity (goal-check shows latest entry per tracker).
- During recovery: log a new chest press entry when it represents new sustained working capacity, not when running through a deload.
- Body weight: weekly is enough during a steady cut, daily if watching short-term noise.
- Run entries resume only after running is cleared.

**Rep-logging rule (load-bearing for goal-check honesty):**
- For any weight/reps entry (Chest sheet), always log the **lowest rep count across the 3 sets**, never the top set or the average.
- Example: chest press 65kg with sets of 15, 14, 12 reps -> log 65kg x 12.
- Rationale: underestimate, never overestimate. Goal-check pulls the latest row; if that row reads a top-set inflated number, the trajectory looks better than it is and Jose loses the early-warning signal that drop-off is widening across sets.
- When Jose reports a session as a single rep number (e.g. "65 x 12"), assume it's already the lowest set unless context says otherwise. When he reports a per-set breakdown like "15-14-12", log the lowest value (12).

**Excel quirk:** Run sheet "Elapsed Time" is formatted as `h:mm` but Jose types it as `mm:ss`. Code parses `time.hour` as minutes, `time.minute` as seconds. Whole-second precision. Runs longer than 60 minutes need the cell reformatted to `[mm]:ss` first.
