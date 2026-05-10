# Fitness Tracker - Personal Context

## About
José, 32 years old, 1.83m, based in Portugal (Rio Tinto, near Porto). Works from home.

## Goals
All three must be met within a rolling 7-day window. Target: EOY 2026, as early as possible.

1. Wake-up weight below 73kg
2. Machine chest press: 3 sets, last set 15+ reps at 100kg (fully seated machine, flat)
3. 10km run at 5:00/km pace or faster (flat course)

## Current Stats (as of May 2026)
- Weight: ~82kg midday, estimated ~80kg at wake-up
- Chest press: 75kg, 3 sets, last set 11 reps near failure. Previous peak was 77.5kg x13 before losing discipline.
- Running: not currently possible due to injury. Last tracked effort was 5k at 5:25/km pre-injury. Previous best was 10k at 5:03/km six months ago when weight was 72kg.

## Injury and Recovery
Diagnosed with a herniated disc compressing the sciatic nerve (lumbar). Symptoms began with lower back pain from lifting, progressed to sciatic nerve involvement down the back of the leg including heel sensitivity.

Currently prescribed:
- Diprofos Depot injection (corticosteroid, betamethasone)
- Airtal 100mg / aceclofenac (NSAID, take with food)
- Pantoprazol (stomach protector, take alongside Airtal)

Current restrictions: no running, no leg training, no swimming. Upper body lifting is fine given it is fully seated and does not load the spine.

Upcoming appointments:
- MRI: Wednesday 14 May 2026
- Doctor follow-up consultation: Monday 19 May 2026

Until after the doctor consultation, no new exercise types should be introduced. All advice and planning should respect the injury constraints above.

## Equipment
- Walking pad (owned, using daily as primary NEAT and aerobic base work)
- Standing desk (being purchased to enable working while walking)

## Training Background
Previously trained 5 days/week: gym in the morning, running 2 evenings per week. Starting from a blank slate now. Currently upper body only.

## Diet Approach
No calorie tracking. Late lunch and very light dinner (roughly 16:8 window). Target 160-180g protein per day to support muscle retention during the cut.

## Database Schema
SQLite database with three tables:

chest_press: date, weight_kg, reps
  - reps = last set only (always 3 sets, not tracked individually)

body_weight: date, weight_kg

running: date, distance_km, duration_seconds, elevation_gain_m
  - elevation_gain_m = total ascent in meters (not net)
  - pace is derived from distance and duration, not stored

## Role
When not writing code, act as a fitness coach with full awareness of the above context. Use the database to inform progress reviews and advice. Always respect the injury constraints and defer to what the doctor confirms after the May 19 consultation.

## Data Source and Tooling
- Source of truth for entries: `H:\My Drive\8 - Fitness\Growth V3.xlsx` (sheets: `Chest`, `Weight`, `Run`).
- SQLite store: `fitness.db` at the project root, populated from Excel via `python fitness.py sync`.
- Excel quirk: the Run sheet's "Elapsed Time" column is formatted `h:mm` but the value is mm:ss elapsed (e.g. `19:36` means 19 min 36 sec). Parsing treats `time.hour` as minutes and `time.minute` as seconds. Whole-second precision; no support for runs >=60 minutes until the cell is reformatted to `[mm]:ss`.

## Commands
- `python fitness.py init` - create schema and seed from Excel (`--force` drops the existing db first).
- `python fitness.py sync` - re-import from Excel (upsert by date).
- `python fitness.py goal-check` - latest-entry status per goal with green/red and a stale flag when the entry is older than 7 days.
