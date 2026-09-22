# <span style="color: #88C0D0;">**Plan Specification & Authoring Guide**</span>

A standardized blueprint for writing technical specifications, software design documents, and engineering implementation plans for coding projects.

---

## <span style="color: #88C0D0;">**Table of Contents**</span>
- [<span style="color: #88C0D0;">**1. Visual Identity & Styling Standards**</span>](#1-visual-identity--styling-standards)
  - [<span style="color: #81A1C1;">1.1 Nord Palette Reference</span>](#11-nord-palette-reference)
  - [<span style="color: #81A1C1;">1.2 Title, Subtitle & Header Formatting Rules</span>](#12-title-subtitle--header-formatting-rules)
  - [<span style="color: #81A1C1;">1.3 Universal Bold & Yellow Text Rule</span>](#13-universal-bold--yellow-text-rule)
  - [<span style="color: #81A1C1;">1.4 Keyword, Concept & Number Highlighting Standard</span>](#14-keyword-concept--number-highlighting-standard)
  - [<span style="color: #81A1C1;">1.5 Table of Contents Styling & Placement</span>](#15-table-of-contents-styling--placement)
  - [<span style="color: #81A1C1;">1.6 Paragraph Spacing Standard</span>](#16-paragraph-spacing-standard)
  - [<span style="color: #81A1C1;">1.7 Plain-Text Math & Calculations</span>](#17-plain-text-math--calculations)
  - [<span style="color: #81A1C1;">1.8 Verification Completion Tagging Standard (`YYYY-MM-DD HH:MM:SS`)</span>](#18-verification-completion-tagging-standard-yyyy-mm-dd-hhmmss)
- [<span style="color: #88C0D0;">**2. Mandatory Document Structure**</span>](#2-mandatory-document-structure)
  - [<span style="color: #81A1C1;">2.1 Title, Subtitle & Document Metadata Table</span>](#21-title-subtitle--document-metadata-table)
  - [<span style="color: #81A1C1;">2.2 Table of Contents (TOC)</span>](#22-table-of-contents-toc)
  - [<span style="color: #81A1C1;">2.3 Section 1: Overview, ELI5 & Sources</span>](#23-section-1-overview-eli5--sources)
  - [<span style="color: #81A1C1;">2.4 Section 2: High-Level Architecture & Flowchart (ASCII)</span>](#24-section-2-high-level-architecture--flowchart-ascii)
  - [<span style="color: #81A1C1;">2.5 Section 3: Assumptions & Prerequisites</span>](#25-section-3-assumptions--prerequisites)
  - [<span style="color: #81A1C1;">2.6 Section 4: Detailed Flow & Component Breakdown</span>](#26-section-4-detailed-flow--component-breakdown)
  - [<span style="color: #81A1C1;">2.7 Section 5: Step-by-Step Implementation Roadmap</span>](#27-section-5-step-by-step-implementation-roadmap)
  - [<span style="color: #81A1C1;">2.8 Section 6: Verification & Sign-Off Checklist</span>](#28-section-6-verification--sign-off-checklist)
  - [<span style="color: #81A1C1;">2.9 Section 7: Risks, Trade-offs & Mitigation</span>](#29-section-7-risks-trade-offs--mitigation)
  - [<span style="color: #81A1C1;">2.10 Section 8: Post-Execution Retrospective & Delta Analysis</span>](#210-section-8-post-execution-retrospective--delta-analysis)
- [<span style="color: #88C0D0;">**3. Diagramming Guidelines (ASCII Block Diagrams)**</span>](#3-diagramming-guidelines-ascii-block-diagrams)
  - [<span style="color: #81A1C1;">3.1 Diagram Conventions</span>](#31-diagram-conventions)
  - [<span style="color: #81A1C1;">3.2 Standard ASCII Diagram Example</span>](#32-standard-ascii-diagram-example)
  - [<span style="color: #81A1C1;">3.3 Colored Diagram Options</span>](#33-colored-diagram-options)
- [<span style="color: #88C0D0;">**4. Reusable Plan Template**</span>](#4-reusable-plan-template)

---

## <span style="color: #88C0D0;">**1. Visual Identity & Styling Standards**</span>

All design plans must maintain high readability, clean whitespace, and aesthetic consistency using the **Nord** design palette.

### <span style="color: #81A1C1;">**1.1 Nord Palette Reference**</span>
Use these specific color tokens for section headers, callouts, and emphasis:

| Category | Token | Hex | Usage in Plans |
| :--- | :--- | :--- | :--- |
| <span style="color: #88C0D0;">**Frost (Primary)**</span> | `nord8` | `#88C0D0` | **Level 1 & 2 Headers (`#`, `##`)**, Primary TOC links, Highlights |
| <span style="color: #81A1C1;">**Frost (Secondary)**</span> | `nord9` | `#81A1C1` | **Level 3 Headers (`###`)**, Subsection TOC links, Subtitles, Module names |
| <span style="color: #8FBCBB;">**Frost (Teal)**</span> | `nord7` | `#8FBCBB` | **Level 4 Headers (`####`)**, Data structures, Protocol names, API endpoints |
| <span style="color: #A3BE8C;">**Aurora (Success)**</span> | `nord14` | `#A3BE8C` | Milestones completed, Passing tests, Low risk |
| <span style="color: #EBCB8B;">**Aurora (Yellow)**</span> | `nord13` | `#EBCB8B` | **All bold text** (inline emphasis, labels, table headers, list prefixes) |
| <span style="color: #BF616A;">**Aurora (Danger)**</span> | `nord11` | `#BF616A` | Critical bottlenecks, Blockers, High risks, **`[NEED TO DECIDE X/Y]`** callouts |
| <span style="color: #B48EAD;">**Aurora (Accent)**</span> | `nord15` | `#B48EAD` | ELI5 callout labels, Conceptual insights |
| <span style="color: #4C566A;">**Polar Night**</span> | `nord3` | `#4C566A` | Demarcation lines, Borders, Secondary metadata |

### <span style="color: #81A1C1;">**1.2 Title, Subtitle & Header Formatting Rules**</span>
All document titles, subtitles, main sections, and subsections **must be both bold and colored** using HTML span tags for universal markdown preview compatibility:

- **Document Title (`#`)**: Do **NOT** prefix with "Architecture Plan:" or "Plan:". Use just the clean, descriptive system/feature title:
  `# <span style="color: #88C0D0;">**[System / Feature Name]**</span>`
- **Document Subtitle (`###`)**: Immediately below the title, provide a subtitle showing the exact file name using `Filename:`:
  `### <span style="color: #81A1C1;">**Filename:** `filename.md`</span>`
- **Plan File Naming Convention**:
  - **Overall Architecture Plan**: For multi-phase initiatives, name the master overview plan `<project_name>_overall.md`.
  - **Phase-by-Phase Milestone Plans**: For each individual implementation phase or milestone, create a dedicated execution plan named `<project_name>_phase_<N>.md` (e.g., `<project_name>_phase_1.md`, `<project_name>_phase_2.md`).
  - **Filename Parity**: The `Filename:` subtitle in the markdown header must strictly match the filename on disk.
- **Main Sections (`##`)**: `## <span style="color: #88C0D0;">**[N. Section Title]**</span>`
- **Subsections (`###`)**: `### <span style="color: #81A1C1;">**[N.M Subsection Title]**</span>`
- **Sub-subsections (`####`)**: `#### <span style="color: #8FBCBB;">**[N.M.K Topic Title]**</span>`

### <span style="color: #81A1C1;">**1.3 Universal Bold & Yellow Text Rule**</span>
Whenever bolding text anywhere in the plan (including inline emphasis, key-value labels, list item prefixes, field names, and table headers), **always bold and color with Nord Yellow (`#EBCB8B`)**:

`<span style="color: #EBCB8B;">**[Text]**</span>`

*(Exceptions: Headers `#`, `##`, `###`, `####` use their assigned Frost colors, and `[NEED TO DECIDE X/Y]` tags use Aurora Danger red `#BF616A`).*

### <span style="color: #81A1C1;">**1.4 Keyword, Concept & Number Highlighting Standard**</span>
All critical keywords, core concepts, subsystem blocks, and key numbers throughout the plan (especially in the TL;DR, technical overviews, parameter lists, and lifecycle steps) must be explicitly highlighted using either:
1. **Bold Yellow Text**: `<span style="color: #EBCB8B;">**keyword**</span>` for core concepts, architecture mechanisms, and key outcomes.
2. **Short Inline Code Quotes**: `` `keyword` `` for numbers, identifiers, parameters, function names, metrics, API endpoints, or data types (e.g., `` `100 ms` ``, `` `1024` ``, `` `POST /api/v1/data` ``, `` `int32_t` ``).

> [!WARNING]
> **Never embed HTML span tags inside markdown backticks** (e.g., do NOT write `` `<span...>...</span>` ``). Backticks treat HTML tags as literal text and break rendering. Always use either `<span style="color: #EBCB8B;">**text**</span>` or `` `code` ``, never both nested together.

### <span style="color: #81A1C1;">**1.5 Table of Contents Styling & Placement**</span>
- **Placement**: The Table of Contents must be placed **before** Section 1 (immediately following the metadata table and divider).
- **Styling**: All links must follow the colored TOC convention:
  - Top-level sections: Wrapped in `<span style="color: #88C0D0;">**...**</span>`
  - Subsections: Wrapped in `<span style="color: #81A1C1;">...</span>`

```markdown
- [<span style="color: #88C0D0;">**1. Section Title**</span>](#1-section-title)
  - [<span style="color: #81A1C1;">1.1 Subsection Title</span>](#11-subsection-title)
```

### <span style="color: #81A1C1;">**1.6 Paragraph Spacing Standard**</span>
- **Universal Empty Line Rule**: In every section, callout block, and narrative description across the entire plan, always include an **empty line between paragraphs**. Never let two separate paragraphs run together without an intervening blank line.
- **Blockquote Visual Line Break**: Empty lines often fail to render visual separation inside markdown blockquotes (`>`). To guarantee proper visual separation between paragraphs inside a blockquote, place an explicit visual line break `<br>` on its own line:

```markdown
> Paragraph 1 text...
>
> <br>
>
> Paragraph 2 text...
```

### <span style="color: #81A1C1;">**1.7 Plain-Text Math & Calculations**</span>
- **No LaTeX Math Syntax**: Do **NOT** use LaTeX math markers (`$...$`, `$$...$$`, `\times`, `\text{...}`) because markdown previewers frequently fail to render them.
- **Readable Plain Text**: Write all mathematical calculations, throughput rates, and resource budgets in clear plain text (e.g., `10,000 req/s * 512 B = 5.12 MB/s`).

### <span style="color: #81A1C1;">**1.8 Verification Completion Tagging Standard (`YYYY-MM-DD HH:MM:SS`)**</span>
- **Green Bold Format**: In **Section 6 (Verification & Sign-Off Checklist)**, when a test or verification item is executed and confirmed, state that it is completed in the **Status** column using clean green bold timestamp text (`nord14` `#A3BE8C`):
  `<span style="color: #A3BE8C;">**YYYY-MM-DD HH:MM:SS**</span>`
- **Timestamp Syntax**: Formatted as pure date and time without brackets or prefixes: `YYYY-MM-DD HH:MM:SS` (e.g., `<span style="color: #A3BE8C;">**2026-09-19 20:44:21**</span>`).
- **Placement**: Placed inside the `Status` column of the verification table in Section 6.

---

## <span style="color: #88C0D0;">**2. Mandatory Document Structure**</span>

Every plan document must follow this standardized sequence of sections:

### <span style="color: #81A1C1;">**2.1 Title, Subtitle & Document Metadata Table**</span>
State the document title, subtitle with `Filename:`, and metadata formatted as a clean table:

| <span style="color: #EBCB8B;">**Field**</span> | <span style="color: #EBCB8B;">**Details**</span> |
| :--- | :--- |
| <span style="color: #EBCB8B;">**Author**</span> | devilsu |
| <span style="color: #EBCB8B;">**Created**</span> | [YYYY-MM-DD HH:MM PST] |
| <span style="color: #EBCB8B;">**Modified**</span> | [YYYY-MM-DD HH:MM PST] |
| <span style="color: #EBCB8B;">**Version**</span> | [v1.0.0] |
| <span style="color: #EBCB8B;">**Status**</span> | [Draft \| In Review \| Approved \| Completed] [YYYY-MM-DD HH:MM:SS] |

- **Timestamped Status Logging**: Always record the timestamp directly alongside the status state (e.g., `In Review 2026-09-19 20:35:00` or `Approved 2026-09-19 20:41:00`).
- **Post-Execution Status Update**: When the user signals to proceed with a plan and execution completes, update the metadata `Status` field to:
  `<span style="color: #A3BE8C;">**Completed YYYY-MM-DD HH:MM:SS**</span>`
  (and update the `Modified` field to match the completion time).
- *(Note: Use `Created` and `Modified` exactly as shown; do not use "Created Date & Time").*

### <span style="color: #81A1C1;">**2.2 Table of Contents (TOC)**</span>
- Placed immediately before Section 1.
- Links to each section and subsection using standard markdown anchor links with Nord styling.

### <span style="color: #81A1C1;">**2.3 Section 1: Overview, ELI5 & Sources**</span>
- **TL;DR**:
  - Must be formatted as **bullet points**.
  - Each bullet point must be **at most 2 sentences**.
  - Can only be nested **at most 3 layers deep**.
  - **Highlighting**: Every bullet point must have bold text (`<span style="color: #EBCB8B;">**...**</span>`) or short code quotes (`` `...` ``) highlighting the important keywords, concepts, and key numbers (e.g., key metrics, service names, endpoint routes).
- **ELI5 (Explain Like I'm 5)**:
  - An intuitive, plain-language analogy so any engineer or stakeholder can immediately grasp the mental model without specialized domain jargon.
  - Length constraint: Strictly **at most 2 short paragraphs** (each 5–6 sentences maximum).
  - An **empty blockquote line (`>`)** must separate paragraph 1 and paragraph 2 inside the quote block.
- **Sources**:
  - A bulleted list with links to websites, documentation, tickets, or references.
  - **Important Rule**: This section must contain **only links and materials explicitly provided by the user**. Do **not** populate this with links discovered independently during autonomous research.
  - **Do NOT include the planning guide itself**: `guide-create_a_plan.md` is an internal authoring guide and must **never** be listed in the Sources section.

### <span style="color: #81A1C1;">**2.4 Section 2: High-Level Architecture & Flowchart (ASCII)**</span>
- An ASCII block diagram enclosed in a fenced `text` code block showing the end-to-end data, control, or pipeline flow.
- Clearly delineates subsystems, operational layers, or services (e.g., Ingestion, Core Service, Storage, Client).
- Shows arrows, directions of flow, protocols, and interfaces.

### <span style="color: #81A1C1;">**2.5 Section 3: Assumptions & Prerequisites**</span>
Document all required dependencies and assumptions:
- **Environment & Dependencies Table**: Tabulate language runtimes, OS/platform requirements, frameworks, external libraries, and dependencies.
- **Architectural Assumptions**: Pre-existing data contracts, schemas, bandwidth/latency constraints, and interface boundaries.

### <span style="color: #81A1C1;">**2.6 Section 4: Detailed Flow & Component Breakdown**</span>
- A comprehensive walkthrough that builds upon Section 1 and Section 2.
- Step-by-step explanation of each component's responsibility and the end-to-end data lifecycle.
- **Undecided Options & Paths**: If there are architectural decisions, trade-offs, or routing choices that require stakeholder or user input, place a bold and red tag **on its own standalone line** immediately preceding the options/routes:

  `<span style="color: #BF616A;">**[NEED TO DECIDE X/Y] [Decision Title]**</span>`

  where `X` is the current item number and `Y` is the total count of undecided items across the plan (e.g., `1/3`, `2/3`, `3/3`). Do **not** embed it inside a bullet point or attach it to list items—it must be on its own dedicated line, separated by empty lines before and after. Clearly present the available alternatives as indented or distinct options beneath it.
- **Lifecycle Steps Formatting**: In Section 4.2 (End-to-End Data Lifecycle), each step title (e.g., `<span style="color: #EBCB8B;">**Step N - [Step Title]**</span>`) must be placed **on its own standalone line**, and the operational actions beneath it must be formatted as **bullet points**.

### <span style="color: #81A1C1;">**2.7 Section 5: Step-by-Step Implementation Roadmap**</span>
Chronological execution checklist broken into logical milestones:
- **Phase 1: Foundation & Core Domain Logic**
- **Phase 2: Service / Interface Integration**
- **Phase 3: Storage, Pipeline & Buffer Management**
- **Phase 4: End-to-End Verification & Benchmarking**
- Tasks are tracked using standard markdown checkboxes (`- [ ]` for pending, `- [x]` for completed).

### <span style="color: #81A1C1;">**2.8 Section 6: Verification & Sign-Off Checklist**</span>
Tabulate test cases with explicit criteria:
- Acceptance criteria and target metrics.
- **Status Column**: Initially set to `<span style="color: #4C566A;">**Planned**</span>`. Once a test or verification step has finished execution and passed, update its Status column with the clean green bold timestamp:
  `<span style="color: #A3BE8C;">**YYYY-MM-DD HH:MM:SS**</span>`
- Record the concrete verified output in the `Actual Result` column.

### <span style="color: #81A1C1;">**2.9 Section 7: Risks, Trade-offs & Mitigation**</span>
Tabulated matrix of potential failure modes:
- Identified risk, severity rating, and actionable mitigation strategy.

### <span style="color: #81A1C1;">**2.10 Section 8: Post-Execution Retrospective & Delta Analysis**</span>
- **Mandatory Post-Execution Requirement**: After the plan is executed, the agent **must add Section 8** to the existing plan document.
- **Retrospective Scope**:
  - **What Was Missing in Original Plan**: Document edge cases, unstated constraints, clock domain interactions, or toolchain-specific requirements that surfaced only during implementation.
  - **Extra Additions & Enhancements**: Document any extra parameters, defensive checks, fallback behaviors, or test cases added beyond the initial plan specification.
  - **Impact on Next Steps**: Summarize practical takeaways that directly feed into the subsequent implementation phase.

---

## <span style="color: #88C0D0;">**3. Diagramming Guidelines (ASCII Block Diagrams)**</span>

All system workflows, interconnects, and pipeline architectures must include clean ASCII diagrams placed inside fenced `text` code blocks.

### <span style="color: #81A1C1;">**3.1 Diagram Conventions**</span>
1. **Enclosure**: Wrap distinct operational layers (e.g., `INGESTION & CLIENT LAYER`, `APPLICATION SERVICE CORE`, `DATA PERSISTENCE & STORAGE`) in outer bounding boxes.
2. **Signal & Flow Labels**:
   - Indicate direction with standard arrows: `-->`, `<--`, `v`, `^`.
   - Place protocol names and transport mechanisms directly on or beside the arrows (e.g., `HTTP / REST`, `gRPC`, `IPC`, `AXIS`).
3. **Block Details**: Inside each block, list 2–3 primary responsibilities.
4. **Cross-Platform Characters**: Use standard ASCII characters (`+`, `-`, `|`) for maximum cross-platform rendering stability.

### <span style="color: #81A1C1;">**3.2 Standard ASCII Diagram Example**</span>

```text
+------------------------------------------------------------------------------------------------+
|                                    INGESTION & CLIENT LAYER                                    |
|                                                                                                |
|   +----------------------------------+   HTTP / JSON   +-----------------------------------+   |
|   |          Client App              |---------------->|         API Gateway / Router      |   |
|   |   - Web / Mobile / CLI           |                 |   - Auth & Rate Limiting          |   |
|   |   - Sends transaction requests   |                 |   - Route dispatching             |   |
|   +----------------------------------+                 +-----------------------------------+   |
+--------------------------------------------------------------------------|---------------------+
                                                                           | Internal RPC
                                                                           v
+------------------------------------------------------------------------------------------------+
|                                    APPLICATION & SERVICE CORE                                  |
|                                                                                                |
|   +----------------------------------+                 +-----------------------------------+   |
|   |       Processing Service         |---------------->|        Asynchronous Queue         |   |
|   |   - Payload validation           |   Enqueue Job   |   - Message broker (Rabbit/Redis) |   |
|   |   - Business rules execution     |                 |   - Dead-letter handling          |   |
|   +----------------------------------+                 +-----------------------------------+   |
|                    |                                                     |                     |
|                    | Read / Write                                        | Consume Task        |
|                    v                                                     v                     |
|   +----------------------------------+                 +-----------------------------------+   |
|   |        Relational Storage        |                 |          Worker Service           |   |
|   |   - Transactional state DB       |                 |   - Heavy compute & 3rd-party sync|   |
|   +----------------------------------+                 +-----------------------------------+   |
+------------------------------------------------------------------------------------------------+
```

### <span style="color: #81A1C1;">**3.3 Colored Diagram Options**</span>
Standard fenced markdown code blocks (` ```text `) do not interpret HTML tags, ensuring pure plain-text consistency across all terminals and editors. If colored visual diagrams are desired in markdown previewers, you can use:
1. **HTML `<pre>` Container**: Replace backticks with `<pre>` and style individual boxes using `<span style="color: #88C0D0;">...</span>`.
2. **Mermaid Diagrams**: Use ` ```mermaid ` with Nord-themed CSS classes for interactive, colored vector diagrams.

---

## <span style="color: #88C0D0;">**4. Reusable Plan Template**</span>

Copy and paste the template below when initiating any new `.md` architecture or implementation plan.

````markdown
# <span style="color: #88C0D0;">**[System / Feature Name]**</span>

### <span style="color: #81A1C1;">**Filename:** `[filename.md]`</span>

| <span style="color: #EBCB8B;">**Field**</span> | <span style="color: #EBCB8B;">**Details**</span> |
| :--- | :--- |
| <span style="color: #EBCB8B;">**Author**</span> | devilsu |
| <span style="color: #EBCB8B;">**Created**</span> | [YYYY-MM-DD HH:MM PST] |
| <span style="color: #EBCB8B;">**Modified**</span> | [YYYY-MM-DD HH:MM PST] |
| <span style="color: #EBCB8B;">**Version**</span> | [v1.0.0] |
| <span style="color: #EBCB8B;">**Status**</span> | [Draft | In Review | Approved | Completed] [YYYY-MM-DD HH:MM:SS] |

---

## <span style="color: #88C0D0;">**Table of Contents**</span>
- [<span style="color: #88C0D0;">**1. Overview & ELI5**</span>](#1-overview--eli5)
  - [<span style="color: #81A1C1;">1.1 TL;DR</span>](#11-tldr)
  - [<span style="color: #81A1C1;">1.2 ELI5 (The Simple Analogy)</span>](#12-eli5-the-simple-analogy)
  - [<span style="color: #81A1C1;">1.3 Sources</span>](#13-sources)
- [<span style="color: #88C0D0;">**2. High-Level Architecture & Flowchart**</span>](#2-high-level-architecture--flowchart)
- [<span style="color: #88C0D0;">**3. Assumptions & Prerequisites**</span>](#3-assumptions--prerequisites)
  - [<span style="color: #81A1C1;">3.1 Environment & Dependencies</span>](#31-environment--dependencies)
  - [<span style="color: #81A1C1;">3.2 Architectural Assumptions</span>](#32-architectural-assumptions)
- [<span style="color: #88C0D0;">**4. Detailed Flow & Component Breakdown**</span>](#4-detailed-flow--component-breakdown)
  - [<span style="color: #81A1C1;">4.1 Component Roles & Responsibilities</span>](#41-component-roles--responsibilities)
  - [<span style="color: #81A1C1;">4.2 End-to-End Data Lifecycle</span>](#42-end-to-end-data-lifecycle)
- [<span style="color: #88C0D0;">**5. Implementation Roadmap**</span>](#5-implementation-roadmap)
- [<span style="color: #88C0D0;">**6. Verification & Sign-Off Checklist**</span>](#6-verification--sign-off-checklist)
- [<span style="color: #88C0D0;">**7. Risks, Trade-offs & Mitigation**</span>](#7-risks-trade-offs--mitigation)
- [<span style="color: #88C0D0;">**8. Post-Execution Retrospective & Delta Analysis**</span>](#8-post-execution-retrospective--delta-analysis)

---

## <span style="color: #88C0D0;">**1. Overview & ELI5**</span>

### <span style="color: #81A1C1;">**1.1 TL;DR**</span>
- [Bullet 1: Main problem and core pipeline with <span style="color: #EBCB8B;">**critical concepts**</span> and key numbers like `96 kHz` or `128` channels highlighted (max 2 sentences).]

- [Bullet 2: Transport mechanism, kernel interface, and buffer management with <span style="color: #EBCB8B;">**DMA / Ring Buffer**</span> highlighted (max 2 sentences).]
  - [Optional sub-bullet level 2: Specific protocol, frame boundaries, or `register/API` details.]
    - [Optional sub-bullet level 3: Maximum 3 layers deep.]

- [Bullet 3: Target user outcome and application consumption pattern with <span style="color: #EBCB8B;">**standard read buffers**</span> highlighted (max 2 sentences).]

### <span style="color: #81A1C1;">**1.2 ELI5 (The Simple Analogy)**</span>
> <span style="color: #B48EAD;">**The Simple Analogy:**</span>
>
> [Paragraph 1: 5-6 sentences explaining the concept with an intuitive everyday analogy without technical buzzwords.]
>
> <br>
>
> [Paragraph 2 (optional): 5-6 sentences explaining how the pieces collaborate to produce the final result.]

### <span style="color: #81A1C1;">**1.3 Sources**</span>
<!-- Note: Only include links, specs, tickets, or docs provided directly by the user. Do NOT include guide-create_a_plan.md itself. -->
- [Specification Doc / User Reference Link 1](https://example.com/spec)

- [Issue / Feature Ticket Link 2](https://example.com/issue)

---

## <span style="color: #88C0D0;">**2. High-Level Architecture & Flowchart**</span>

```text
+-------------------------------------------------------------------+
|                        ENTRY / INGESTION                          |
|                                                                   |
|   +-------------------+      Request       +------------------+   |
|   |      Client       |------------------->|   Entry Gateway  |   |
|   +-------------------+                    +------------------+   |
+------------------------------------------------------|------------+
                                                       | Forward
                                                       v
+-------------------------------------------------------------------+
|                        CORE SERVICE LAYER                         |
|                                                                   |
|   +-------------------+      Sync/Async    +------------------+   |
|   |  Primary Service  |------------------->|  Worker / Queue  |   |
|   +-------------------+                    +------------------+   |
|             |                                        |            |
|             +-------------------+--------------------+            |
|                                 | Read / Write                    |
|                                 v                                 |
|                      +---------------------+                      |
|                      |   Database / State  |                      |
|                      +---------------------+                      |
+-------------------------------------------------------------------+
```

---

## <span style="color: #88C0D0;">**3. Assumptions & Prerequisites**</span>

### <span style="color: #81A1C1;">**3.1 Environment & Dependencies**</span>

| <span style="color: #EBCB8B;">**Dependency / Tool**</span> | <span style="color: #EBCB8B;">**Required Version**</span> | <span style="color: #EBCB8B;">**Purpose**</span> |
| :--- | :--- | :--- |
| <span style="color: #EBCB8B;">**Language Runtime**</span> | [e.g., Node.js >= 20 / Python >= 3.11] | Base application execution |
| <span style="color: #EBCB8B;">**Database / Store**</span> | [e.g., PostgreSQL 16 / Redis 7] | Persistent data and caching |
| <span style="color: #EBCB8B;">**Frameworks / Libraries**</span> | [e.g., FastAPI, Pydantic, SQLAlchemy] | Core routing and ORM |

### <span style="color: #81A1C1;">**3.2 Architectural Assumptions**</span>
- <span style="color: #EBCB8B;">**Network Boundaries**</span>: All inter-service calls operate within a trusted private VPC network.

- <span style="color: #EBCB8B;">**Traffic Profile**</span>: Average load of X requests/sec with peak burst capacity of Y.

- <span style="color: #EBCB8B;">**Security & Auth**</span>: Authentication handled upstream; services accept validated JWT claims.

---

## <span style="color: #88C0D0;">**4. Detailed Flow & Component Breakdown**</span>

### <span style="color: #81A1C1;">**4.1 Component Roles & Responsibilities**</span>
- <span style="color: #EBCB8B;">**Client / Ingestion Layer**</span>: Initiates requests, handles user sessions, receives responses.

- <span style="color: #EBCB8B;">**Primary Service**</span>: Validates schema, applies domain business logic, manages transactional consistency.

<span style="color: #BF616A;">**[NEED TO DECIDE 1/2] Architectural Route Selection**</span>

- <span style="color: #EBCB8B;">**Option A**</span>: [Description of Option A, benefits, trade-offs]

- <span style="color: #EBCB8B;">**Option B**</span>: [Description of Option B, benefits, trade-offs]

### <span style="color: #81A1C1;">**4.2 End-to-End Data Lifecycle**</span>
<span style="color: #EBCB8B;">**Step 1 - Request Ingestion**</span>

- The client issues an HTTP/gRPC request to the API entry point.
- The gateway parses headers and validates security credentials.

<span style="color: #EBCB8B;">**Step 2 - Validation & Processing**</span>

- The incoming payload is validated against schemas.
- Core business logic executes state transitions.

<span style="color: #EBCB8B;">**Step 3 - State Persistence & Queueing**</span>

- Transactional state commits to persistent database storage.
- Asynchronous tasks dispatch to background worker queues.

<span style="color: #EBCB8B;">**Step 4 - Response Delivery**</span>

- An immediate acknowledgment response is returned to the client.
- Status metrics and trace events are emitted to monitoring endpoints.

---

## <span style="color: #88C0D0;">**5. Implementation Roadmap**</span>

- [ ] <span style="color: #EBCB8B;">**Phase 1: Foundation & Core Domain Logic**</span>
  - [ ] Define data models, schemas, and validation contracts
  - [ ] Implement core domain algorithms and write unit tests

- [ ] <span style="color: #EBCB8B;">**Phase 2: Service APIs & Integration**</span>
  - [ ] Implement controllers, route handlers, and middleware
  - [ ] Wire service layer with queue publishers and consumers

- [ ] <span style="color: #EBCB8B;">**Phase 3: Storage & External Adapters**</span>
  - [ ] Create database migration scripts and seed data
  - [ ] Integrate external service clients with retry and circuit breakers

- [ ] <span style="color: #EBCB8B;">**Phase 4: End-to-End Testing & Deployment**</span>
  - [ ] Write integration test suite for end-to-end scenarios
  - [ ] Set up CI/CD pipeline, monitoring dashboards, and alerting rules

---

## <span style="color: #88C0D0;">**6. Verification & Sign-Off Checklist**</span>

| <span style="color: #EBCB8B;">**Test / Acceptance Case**</span> | <span style="color: #EBCB8B;">**Target Criteria**</span> | <span style="color: #EBCB8B;">**Actual Result**</span> | <span style="color: #EBCB8B;">**Status**</span> |
| :--- | :--- | :--- | :--- |
| <span style="color: #EBCB8B;">**Unit Test Suite**</span> | All unit tests pass with zero failures | 100% passing across 42 test suites | <span style="color: #A3BE8C;">**YYYY-MM-DD HH:MM:SS**</span> |
| <span style="color: #EBCB8B;">**Integration Flow**</span> | Happy path and edge case scenarios pass | Pending | <span style="color: #4C566A;">**Planned**</span> |
| <span style="color: #EBCB8B;">**Performance SLA**</span> | p99 latency < 200ms under benchmark load | Pending | <span style="color: #4C566A;">**Planned**</span> |

---

## <span style="color: #88C0D0;">**7. Risks, Trade-offs & Mitigation**</span>

| <span style="color: #EBCB8B;">**Identified Risk**</span> | <span style="color: #EBCB8B;">**Severity**</span> | <span style="color: #EBCB8B;">**Mitigation Strategy**</span> |
| :--- | :--- | :--- |
| <span style="color: #EBCB8B;">**Data Inconsistency on Failure**</span> | High | Use two-phase commits / outbox pattern for distributed operations |
| <span style="color: #EBCB8B;">**Downstream Rate Limits**</span> | Medium | Implement client-side token bucket rate limiter with exponential backoff |
| <span style="color: #EBCB8B;">**Schema Migration Downtime**</span> | Medium | Follow expand-and-contract migration strategy for backward compatibility |

---

## <span style="color: #88C0D0;">**8. Post-Execution Retrospective & Delta Analysis**</span>

### <span style="color: #81A1C1;">**8.1 Missing Elements in Original Plan**</span>
- [Itemize anything that was missing, unaccounted for, or required unexpected adaptation during implementation.]

### <span style="color: #81A1C1;">**8.2 Additions & Enhancements Implemented**</span>
- [Itemize any defensive logic, extra verification cases, or architectural extensions added during execution.]

### <span style="color: #81A1C1;">**8.3 Key Takeaways & Impact on Next Phases**</span>
- [Summarize learnings and architectural confirmations that carry forward into subsequent milestones.]
````