# Codebase Hygiene, Refactoring & Obsolete Artifact Lifecycle Standard

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-HYGIENE-001` |
| **Topic** | Codebase Hygiene, Refactoring & Obsolete Artifact Lifecycle Management |
| **Created** | 2026-10-06 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **When an architectural redesign, implementation refactor, or step redo supersedes existing modules, code files, testbenches, or build scripts, the obsolete artifacts MUST be decommissioned immediately in the same atomic commit.**
> Leaving orphaned, unused, or superseded files lingering in the codebase is strictly prohibited. Every refactor must be accompanied by an exhaustive reference sweep across plans, blueprints, and build scripts to eliminate stale component names and maintain absolute alignment between documentation and reality.

---

## 2. Rationale & Toolchain Hazards

1. **Toolchain Globbing & Build Collisions**:
   - Hardware EDA toolchains (Vivado, Vitis HLS) and software build tools (CMake, Python `find_packages`, Makefile wildcards) often ingest all source files in a directory (e.g., `add_files [glob *.v]`).
   - Orphaned modules lingering in source directories cause duplicate symbol definitions, unintentional synthesis of obsolete logic, or unexpected synthesis hierarchy errors.
2. **Reviewer & Cognitive Confusion**:
   - Stale or orphaned files prompt ambiguity during code reviews and architectural audits ("Is this module active?", "Why does this file exist?", "Which path is used in production?").
   - This creates unnecessary debugging overhead and erodes trust between human engineers and AI assistants.
3. **Documentation Drift & Architectural Rot**:
   - When code is refactored but blueprints, block design specs, or testbench descriptions still reference the deprecated module name, downstream phases inherit erroneous assumptions, leading to compounded rework.

---

## 3. The Atomic Decommissioning Protocol

Whenever an implementation step is refactored, redone, or redesigned to replace an earlier approach, follow this 4-phase lifecycle protocol:

```text
┌─────────────────────────────────────────────────────────────┐
│ 1. Implement & Validate New Architecture                     │
│    Construct replacement module / adopt vendor IP / refactor│
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. Decommission Obsolete Artifacts                          │
│    Explicitly remove dead source files (git rm <stale_file>)│
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. Exhaustive Cross-Document Reference Sweep                │
│    Grep repository for old module names and update all docs │
│    (overall.md, phase_*.md, summary.md, testbenches, scripts)│
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. Regression Pass & Atomic Commit                          │
│    Re-run unit/integration tests to ensure zero regressions;│
│    commit replacement, deletion, and doc updates together   │
└─────────────────────────────────────────────────────────────┘
```

### Detailed Steps:

1. **Phase 1: Implement & Validate Replacement Logic**:
   - Complete the new implementation adhering to system design principles.
   - Verify the replacement logic independently with unit tests.

2. **Phase 2: Remove Superseded Source Files**:
   - Delete the superseded file(s) from the source tree using `git rm <path/to/superseded_file>`.
   - Do NOT rename files to `.bak`, `.old`, or comment out the entire file while keeping it in the source folder.
   - If historical preservation of an obsolete approach is explicitly requested by the user, move the file into a dedicated historical archive directory (e.g., `archive/` or `legacy/`) clearly decoupled from active source compilation paths.

3. **Phase 3: Exhaustive Reference Sweep**:
   - Execute a codebase-wide grep for the deprecated filename and symbol name:
     ```bash
     grep -rn "<deprecated_module_name>" .
     ```
   - Update all occurrences across:
     - Master blueprints and phase roadmap documents (`overall.md`, `phase_*.md`, `summary.md`).
     - Simulation runner scripts, Makefiles, and synthesis scripts.
     - Block design automation TCL scripts and testbench instantiation lists.
     - Root readme status tables.

4. **Phase 4: Regression Verification**:
   - Re-execute the testbench or build script to confirm that the removal of the superseded artifact did not break any hidden or indirect dependencies.
   - Verify that test logs pass with zero errors before staging.

---

## 4. Pre-Commit Hygiene Checklist

Before marking any refactoring step complete or requesting `acp`, verify:

- [ ] **Zero Dead Files**: All superseded modules, wrappers, or scratch scripts are removed from active `rtl/`, `src/`, and `sim/` directories.
- [ ] **Clean Grep**: Grepping for the old component name returns zero matches across all active source files, build scripts, and blueprint documents.
- [ ] **Successful Build/Sim**: Build scripts and unit tests execute cleanly without relying on cached compilation artifacts from the deleted files.
- [ ] **Atomic Commit**: The new implementation, the deletion of deprecated files, and the documentation updates are bundled into the same commit.

---

## 5. References & Cross-Standards

- **Root Rules Hub**: [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md)
- **Git Commit & Push Standard**: [`git.md`](git.md)
- **Subproject Repository Architecture**: [`repo_structure.md`](repo_structure.md)
- **Step Verification Evidence Standard**: [`backup_evidences.md`](backup_evidences.md)
- **Pre-Authoring Planning Directive**: [`../skills/planning.md`](../skills/planning.md)
