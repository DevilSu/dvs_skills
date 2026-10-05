# Git Commit and Push Confirmation Rule

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-GIT-001` |
| **Topic** | Git Version Control & Remote Sync |
| **Created** | 2026-10-04 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **The AI assistant must NEVER execute `git commit` or `git push` autonomously.**
> Always ask the user for explicit permission before committing or pushing changes to any local or remote repository.

---

## 2. Scope & Applicability

This rule applies universally across all projects, subprojects, and repositories within the workspace (including `mic_arr_v2`, `dvs_skills`, and future projects).

---

## 3. Required Workflow

Before performing any Git commit or push:

1. **Review Local Modifications**:
   - Check `git status` to verify which files are modified, staged, or untracked.
   - Run `git diff` if needed to ensure only intended changes are staged.

2. **Propose the Commit to the User**:
   - Present the list of files to be committed.
   - Present the proposed commit message (following Conventional Commits format, e.g., `docs(...)`, `feat(...)`, `fix(...)`).
   - Clearly state whether `git push` will also be executed.

3. **Await Explicit Confirmation**:
   - Wait for the user to explicitly approve (e.g., "Yes, go ahead and commit/push", or similar confirmation).
   - If the user requests edits to the commit message or file selection, adjust accordingly.

4. **Execute Only Upon Approval**:
   - Only execute `git commit` and `git push` after confirmation is granted.

---

## 4. Authorized Shorthand (`acp`)

> [!TIP]
> **User Command: `acp` (Add, Commit, and Push)**
> When DVS explicitly sends `acp`, this acts as pre-approved authorization to:
> 1. Stage all relevant modifications (`git add`).
> 2. Commit with a concise Conventional Commit message (`git commit`).
> 3. Push immediately to the remote branch (`git push`).
>
> Reference: [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md).
