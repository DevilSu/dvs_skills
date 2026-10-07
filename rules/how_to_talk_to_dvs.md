# How to Talk to DVS (Rules Index & Interaction Protocol)

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-TALK-001` |
| **Topic** | Communication Standards, Editor Tab Presentation, & Rules Management Hub |
| **Created** | 2026-10-04 |
| **Status** | **Active / Mandatory** |

---

## 1. Core Communication Rule: The 3-Paragraph Limit

> [!IMPORTANT]
> **If an assistant's response or technical explanation exceeds 3 paragraphs, it MUST be delivered in a formatted `.md` editor tab (an artifact markdown document) so DVS can view, review, and comment on it directly in the editor.**

### Guidelines:
1. **Short Responses (<= 3 paragraphs)**:
   - Simple confirmations, short answers, single-command executions, or brief status updates should be posted directly in the chat.
2. **Long Responses (> 3 paragraphs)**:
   - Create or update a formatted `.md` artifact file (with `RequestFeedback: true`) in the artifact directory.
   - Keep the chat response to a concise executive summary (1–2 paragraphs) pointing directly to the formatted editor tab via a clickable link.

---

## 2. Core Formatting Rule: Zero Unrendered LaTeX & Pre-Response Rendering Audit

> [!IMPORTANT]
> **Never output unrendered LaTeX delimiters or math syntax in chat responses or markdown documents.**
> The editor does not render LaTeX math syntax (`$ ... $`, `$$ ... $$`, `\text{...}`, `\times`, `\pm`, `\approx`, etc.). Raw LaTeX symbols appear as broken code and are strictly forbidden.

### Formatting Rules:
1. **Pre-Response Rendering Audit**:
   - Before emitting any response or writing any markdown file, the assistant must scan the output to verify that zero unrendered LaTeX tokens exist.
2. **Plain-Text Mathematical Equivalents**:
   - Instead of `$25\text{ bit} \times 18\text{ bit}$`, write `25-bit x 18-bit`.
   - Instead of `$\pm 0.25\text{ dB}$`, write `+-0.25 dB`.
   - Instead of `$\sim 96\text{ dB}$`, write `~96 dB` or `approx 96 dB`.
   - Instead of `$F_s = 96\text{ kHz}$`, write `F_s = 96 kHz`.
   - Instead of `$\alpha = 1 - 2^{-10}$`, write `alpha = 1 - 2^(-10)`.
   - Instead of `$120\text{ to }130\text{ dB SPL}$`, write `120 to 130 dB SPL`.
3. **Dimensions, Frequencies, and Variables**:
   - Always format numbers and engineering units using plain text or backticked inline code: `3.84 MHz`, `96 kHz`, `15 Hz`, `144 dB`, `24-bit`.

---

## 3. Mermaid Diagram Validation & Rendering Rule (`RULE-MERMAID-001`)

> [!IMPORTANT]
> **All Mermaid diagrams (` ```mermaid `) MUST be verified to render cleanly without syntax errors.**
> Broken Mermaid blocks containing unquoted brackets in edge labels, unquoted parentheses in node boxes, or invalid subgraph identifiers cause fatal parser failures in the editor and web previews.

### Guidelines:
1. **Double-Quote Special Characters in Node Labels**: Any label with `()`, `[]`, `/`, `+`, `-`, or `<br/>` must be double-quoted: `NODE["Label (Info)"]`.
2. **Double-Quote Special Characters in Edge Labels**: Always quote edge text with brackets or math: `-->|"pdm_in [NUM_CHANNELS-1:0]"|`.
3. **Clean Subgraph Syntax**: Use alphanumeric IDs without dots or spaces: `subgraph ID ["Display Title"]`.
4. Detailed standard: See [`mermaid.md`](mermaid.md).

---

## 4. Plan Update Prompting Rule: Ask at End of Response

> [!IMPORTANT]
> **If any technical discussion, explanation, clarification, or planning session yields findings, architecture decisions, or parameter refinements that affect a plan's `.md` file, the assistant MUST explicitly ask DVS at the very end of the response whether to update the corresponding plan `.md` file.**

### Guidelines:
1. **Identify Plan Impact**:
   - During technical discussions (e.g. word width choices, frequency response verification, pipeline microarchitecture, hardware resource allocation), assess whether the project's blueprints/plans (e.g. `blueprints/.../phase_X.md`, `overall.md`) should incorporate the conclusion.
2. **Mandatory Closing Prompt**:
   - At the bottom of the response, include an explicit closing prompt:
     `"Would you like me to update [<path_to_plan.md>](<link>) with these details?"`
   - Specify the exact section or bullet points that will be added or refined.
3. **Never Update Silently**:
   - Await explicit user approval before editing the plan file.

---

## 5. DVS User Shorthand & Shortnames Glossary

To streamline interactive chat workflows, DVS uses standardized shortnames that the assistant must immediately recognize and execute:

| Shortname | Full Meaning | Exact Assistant Action |
| :--- | :--- | :--- |
| **`addr cmt`** | **Address Comment** | Inspect the inline comment, highlight, or critique left by DVS on an artifact `.md` document or code file. Directly resolve the issue in the target document/code, verify accuracy, and report the correction concisely. |
| **`acp`** | **Add, Commit, and Push** | Stage local modifications (`git add`), create a concise Conventional Commit message, commit changes (`git commit`), and push to remote (`git push`). **`acp` serves as explicit user confirmation under `RULE-GIT-001`.** |

---

## 6. Rule Ingestion Protocol (Adding Rules in the Future)

Whenever DVS instructs the assistant to **add, modify, or update a rule**, the assistant must strictly follow this protocol:

```text
[ DVS Requests New Rule ]
            │
            ▼
┌───────────────────────────────────────────────┐
│ 1. Read `rules/how_to_talk_to_dvs.md`         │
│    Examine existing rules and categories      │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│ 2. Evaluate Rule Placement                    │
│    - Does it belong in an existing rule file? │
│      (e.g., git.md, how_to_talk_to_dvs.md)    │
│    - Or does it require a new specialized file│
│      (e.g., rules/<new_rule>.md)?             │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│ 3. Author / Update the Rule File              │
│    Use standardized format (Meta table,       │
│    Statement, Scope, Workflow, Verification)  │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│ 4. Update the Master Indexes                  │
│    - Add rule link to `how_to_talk_to_dvs.md` │
│    - Add rule link to `dvs_skills/README.md`  │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│ 5. Confirm with User before Committing        │
│    Follow RULE-GIT-001 (Ask before commit/push│
└───────────────────────────────────────────────┘
```

---

## 7. Master Index of Rules in `rules/`

| Rule ID | File | Description | Status |
| :--- | :--- | :--- | :--- |
| `RULE-TALK-001` | [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md) | **(Root File)** Response length threshold (>3 paragraphs -> `.md` tab), zero unrendered LaTeX, plan update prompts, and DVS shortnames glossary (`addr cmt`, `acp`). | **Active** |
| `RULE-MERMAID-001` | [`mermaid.md`](mermaid.md) | Mandatory syntax validation for Mermaid diagrams (double-quoted special characters, quoted edge labels, clean subgraphs) to prevent rendering failures. | **Active** |
| `RULE-GIT-001` | [`git.md`](git.md) | Mandatory explicit confirmation before executing `git commit` or `git push` (authorized via `acp`). | **Active** |
| `RULE-REPO-001` | [`repo_structure.md`](repo_structure.md) | Standard tripartite Subproject-Driven Repository Architecture (`blueprints/`, `hw/`, `sw/`) modeled after `mic_arr_v2`. | **Active** |
| `RULE-EVID-001` | [`backup_evidences.md`](backup_evidences.md) | Mandatory empirical evidence logging in `blueprints/<subproject>/backup_evidences.md` after completing any implementation step. Shared across all phases. | **Active** |
| `RULE-HW-001` | [`hardware_architecture.md`](hardware_architecture.md) | FPGA clock infrastructure decoupling, non-blocking real-time sensor streaming, and vendor IP catalog reuse. | **Active** |
| `DIR-PLAN-001` | [`../skills/planning.md`](../skills/planning.md) | Mandatory interactive chat alignment and commentable `.md` artifacts before modifying plans. | **Active** |

---

## 8. Repository Link Standards

- All links within rules and documentation must be **strictly relative repository paths** (e.g. `rules/git.md`, `../skills/planning.md`) so they render seamlessly on GitHub and across local clones.
- Math must be formatted as **plain text** without unrendered LaTeX delimiters (`$`, `$$`, `\frac`).
