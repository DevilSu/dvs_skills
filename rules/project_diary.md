---
trigger: always_on
description: >-
  Mandatory append-only, immutable daily engineering journal tracking real-world actions, failures, and breakthroughs.
---

# Project Diary & Narrative Engineering Journal Standard

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-DIARY-001` |
| **Topic** | Project Diary, Immutable Daily Journaling & Narrative History Standard |
| **Created** | 2026-10-20 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **Every engineering project MUST maintain a single, append-only, immutable daily journal file located at:**
> `blueprints/diary.md`
>
> **The diary records the raw, unvarnished chronological journey of the project on a day-by-day basis. Entries are organized using the project day and calendar date as section headings (`## Day X — YYYY-MM-DD: <Theme/Milestone>`). Once a day's entry is written and committed, it is strictly IMMUTABLE and MUST NOT be rewritten, sanitized, or deleted.**

---

## 2. Core Philosophy & The "Acquired" Podcast Vision

1. **The Unvarnished Engineering Story**:
   Engineering projects are rarely linear paths from requirements to success. The most valuable technical insights, architectural breakthroughs, and institutional lessons are born from **failures, dead ends, race conditions, debug rabbit holes, and dramatic pivots**. Sanitized commit histories and final summaries erase this reality.
2. **Immutable Daily Ledger (Append-Only)**:
   This file is fundamentally different from `plan.md`, `overall.md`, or `phase_<N>.md` files:
   - **Plan files (`plan.md`, `phase_*.md`)** are **living, mutable documents** that are constantly updated, refined, and adjusted as the project unfolds to guide forward-looking execution.
   - **The Diary (`blueprints/diary.md`)** is an **immutable, append-only historical record**. Yesterday's mistakes, mistaken hypotheses, and failed tests stay exactly as they were written, providing an authentic time-capsule of what the team knew, thought, and experienced at that exact moment.
3. **The Acquired Podcast Foundation**:
   At the conclusion of the project, `blueprints/diary.md` along with the repository commit history will serve as the primary source material to script a comprehensive, narrative-driven podcast episode walking through the entire story of the project in the style of the ***Acquired*** podcast (hosted by Ben Gilbert and David Rosenthal):
   - **Act I: Origins & The Grand Vision** (What were we trying to build, and why was it ambitious?)
   - **Act II: The Early Architecture & False Dawns** (Initial prototypes, first passes, hidden assumptions)
   - **Act III: The Crucible / The Crisis** (The bugs, stalls, race conditions, and architectural disasters that threatened the project)
   - **Act IV: The Breakthrough & The Pivot** (The eureka moments, root-cause breakthroughs, and engineering rebuilds)
   - **Act V: The Climax & Victory** (Final saturation benchmarks, bit-exact verification, and sign-offs)
   - **Playbook & Analysis** (Key technical takeaways, architectural trade-offs, and timeless engineering principles)

---

## 3. Location & Naming Standard

* The diary file must reside at:
  `blueprints/diary.md`
* In projects with multiple subprojects, `blueprints/diary.md` at the repository root of `blueprints/` serves as the master project journal covering cross-subproject evolution. Subprojects may also link back to this master diary.

---

## 4. Entry Formatting & Content Requirements

Each daily entry must be titled with a level-2 markdown header and structured with clear narrative and technical depth:

```markdown
## Day X — YYYY-MM-DD: <Theme / Daily Epic Title>

### 1. The Goal & Hypothesis
What were we attempting to accomplish today? What was our initial mental model or working hypothesis?

### 2. Actions & Technical Decisions
What was designed, coded, synthesized, or tested? What tools, IP cores, or scripts were developed?

### 3. Failures, Anomalies & Rabbit Holes
What broke? What unexpected behaviors, timing violations, race conditions, or performance bottlenecks did we encounter? 
Document the false hypotheses and debugging dead ends honestly.

### 4. Root Causes & Breakthrough Moments
What was the actual underlying mechanism of the failure? What was the eureka moment or architectural fix?

### 5. Daily Verdict & Key Metrics
Concrete numbers: frame counts, throughput (Mbps), timing slack (WNS), error counters, and pass/fail verdicts.
```

---

## 5. Summary of Differences: Diary vs. Plans vs. Evidences

| Document | File Path | Lifecycle Policy | Content Focus |
| :--- | :--- | :--- | :--- |
| **Project Diary** | `blueprints/diary.md` | **Append-Only & Immutable** (Never edit past entries) | Chronological narrative, emotional/technical journey, failures, pivots, eureka moments, Acquired podcast source material. |
| **Project Plans** | `blueprints/<subproject>/phase_*.md` | **Living & Mutable** (Constantly updated and refined) | Forward-looking roadmap, specifications, test matrices, tasks, and status sign-offs. |
| **Backup Evidences** | `blueprints/<subproject>/backup_evidences.md` | **Cumulative Empirical Archive** | Verifiable technical proof: raw simulation logs, FFT spectra, timing summaries, and CLI reproducibility commands. |
