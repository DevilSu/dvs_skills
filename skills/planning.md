# <span style="color: #88C0D0;">**Planning Process & Pre-Authoring User Alignment Directive**</span>

### <span style="color: #81A1C1;">**Filename:** `planning.md`</span>

| <span style="color: #EBCB8B;">**Field**</span> | <span style="color: #EBCB8B;">**Details**</span> |
| :--- | :--- |
| <span style="color: #EBCB8B;">**Directive ID**</span> | `DIR-PLAN-001` |
| <span style="color: #EBCB8B;">**Topic**</span> | Engineering Planning & Specification Workflow |
| <span style="color: #EBCB8B;">**Author**</span> | devilsu |
| <span style="color: #EBCB8B;">**Created**</span> | 2026-10-04 13:57 PST |
| <span style="color: #EBCB8B;">**Modified**</span> | 2026-10-04 13:57 PST |
| <span style="color: #EBCB8B;">**Version**</span> | v1.0.0 |
| <span style="color: #EBCB8B;">**Status**</span> | <span style="color: #A3BE8C;">**Active / Mandatory Directive**</span> |

---

## <span style="color: #88C0D0;">**Table of Contents**</span>
- [<span style="color: #88C0D0;">**1. Mandatory Directive Statement**</span>](#1-mandatory-directive-statement)
- [<span style="color: #88C0D0;">**2. Rationale & Objectives**</span>](#2-rationale--objectives)
- [<span style="color: #88C0D0;">**3. Standard Interactive Planning Workflow**</span>](#3-standard-interactive-planning-workflow)
- [<span style="color: #88C0D0;">**4. Cross-Reference Standards**</span>](#4-cross-reference-standards)

---

## <span style="color: #88C0D0;">**1. Mandatory Directive Statement**</span>

> [!IMPORTANT]
> **1. When planning, ALWAYS ask and align with the user first before writing to any top-level `.md` blueprint files (`overall.md`, `phase_<N>.md`).**
> The AI assistant must never unilaterally write or overwrite project blueprint documents without first presenting the proposed plan structure, architectural decisions, and scope to the user and receiving explicit approval.
>
> <br>
>
> **2. Provide Proposals in Commentable Markdown Format:**
> When presenting architecture trade-offs, options analyses, or proposed plans for user evaluation, format the content as an interactive markdown artifact (`RequestFeedback: true`) so the user can easily review and leave inline comments directly in the IDE UI.
>
> <br>
>
> **3. Mandatory Pre-Submission Rendering Quality Check:**
> The assistant **MUST verify formatting and rendering before showing any markdown document**. Strictly prohibit unrendered LaTeX math syntax (`$$...$$`, `$...$`, `\frac`, `\approx`, `\times`). Always write mathematical formulas and calculations in clean, readable plain text. Verify tables, ASCII diagrams, and code fences render flawlessly prior to presenting to the user.
>
> <br>
>
> **4. Mandatory Closing Prompt for Plan Updates:**
> If any discussion, explanation, or planning session yields decisions, parameters, or insights that affect a plan's `.md` file, the assistant **MUST explicitly ask the user at the end of the response** whether to update the corresponding plan `.md` file.

---

## <span style="color: #88C0D0;">**2. Rationale & Objectives**</span>

1. **Avoid Presumptive Specifications**: Generating large blueprint documents before understanding all technical nuances risks committing to incorrect assumptions, topologies, or tool configurations that the user must then spend time correcting.
2. **Interactive Architectural Alignment**: Complex FPGA, DSP, and hardware-software systems involve key trade-offs (e.g., custom HLS vs. vendor IP, sample rates, buffer dimensions, resource allocation). Aligning on these decisions in chat first ensures shared intent.
3. **Seamless Inline Feedback**: Presenting proposals in a commentable artifact allows the user to highlight specific sentences, calculations, or trade-offs and attach targeted feedback without ambiguous back-and-forth chat exchanges.
4. **Zero-Defect Rendering**: Broken LaTeX markers or corrupted formatting degrades readability and forces unnecessary clarification cycles. Pre-checking rendering ensures clean, professional documentation every time.
5. **Clean Git History**: Prevents churn and unnecessary file revisions by ensuring that when a plan file is written, it represents an agreed-upon, high-confidence blueprint.

---

## <span style="color: #88C0D0;">**3. Standard Interactive Planning Workflow**</span>

Whenever a user requests to plan a project, subproject, or phase:

1. <span style="color: #EBCB8B;">**Step 1 - Scoping & Options Analysis in Commentable Format**</span>:
   - Formulate the proposal, architecture trade-offs, or option comparisons.
   - Run a pre-submission rendering audit: guarantee zero LaTeX markers (`$`), verify plain-text math, and check table/diagram formatting.
   - Present the proposal as a commentable markdown document (`RequestFeedback: true`) so the user can leave inline comments on specific points.

2. <span style="color: #EBCB8B;">**Step 2 - Interactive Alignment & Comment Resolution**</span>:
   - Address any comments or feedback left by the user directly.
   - Align on key architectural decision points (e.g. `[NEED TO DECIDE X/Y]`).
   - Await explicit go-ahead before writing to repository blueprint documents.

3. <span style="color: #EBCB8B;">**Step 3 - Write Markdown Specification Only After Approval**</span>:
   - Upon receiving the user's explicit confirmation or design choices, create or update the target `.md` document following [guide-create_a_plan.md](guide-create_a_plan.md).
   - Use strictly relative paths, plain-text math, and Nord styling standards.

4. <span style="color: #EBCB8B;">**Step 4 - Review & Git Confirmation**</span>:
   - Present the created plan summary to the user.
   - In accordance with [git.md](../rules/git.md), ask the user before staging, committing, or pushing any changes.

---

## <span style="color: #88C0D0;">**4. Cross-Reference Standards**</span>

- **Styling & Structure**: [guide-create_a_plan.md](guide-create_a_plan.md)
- **Version Control Rules**: [git.md](../rules/git.md)
