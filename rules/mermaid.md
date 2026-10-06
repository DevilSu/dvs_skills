# Mermaid Diagram Validation & Rendering Rule

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-MERMAID-001` |
| **Topic** | Markdown Mermaid Diagram Syntax & Rendering Guarantee |
| **Created** | 2026-10-04 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **Every Mermaid diagram (` ```mermaid `) in any response, artifact document, or plan specification MUST be syntactically valid and verified to render cleanly in standard Mermaid previewers and markdown viewers.**
> Unrendered or broken Mermaid codeblocks with syntax errors (e.g., parse errors on brackets, unquoted parentheses, or invalid subgraph identifiers) are strictly unacceptable.

---

## 2. Mandatory Mermaid Syntax Requirements

To guarantee 100% reliable rendering across GitHub, IDE tabs, and web markdown engines, follow these structural rules:

### **1. Always Double-Quote Node Labels with Special Characters**
Whenever a node label contains parentheses `()`, brackets `[]`, slashes `/`, hyphens `-`, pluses `+`, equals `=`, or line breaks `<br/>`, enclose the entire label string in double quotes:
- **Correct**: `HLS["cic_fir_decimator<br/>(pcm_out)"]`
- **Incorrect**: `HLS[cic_fir_decimator<br/>(pcm_out)]` *(Parentheses break the node bracket parser)*

### **2. Always Double-Quote Edge Labels Containing Special Characters**
Edge labels enclosed in pipes `|...|` must be double-quoted if they contain brackets, parentheses, colons, or mathematical symbols:
- **Correct**: `-->|"pdm_in.data [NUM_CHANNELS-1:0]"|`
- **Incorrect**: `-->|pdm_in.data [NUM_CHANNELS-1:0]|` *(Unquoted `[` triggers fatal parse error: expecting SPACE, got '[')*
- **Correct**: `-->|"3.84 MHz (50% Duty)"|`
- **Incorrect**: `-->|3.84 MHz (50% Duty)|`

### **3. Alphanumeric Subgraph IDs and Quoted Titles**
Subgraph definitions must use pure alphanumeric identifiers (no dots, spaces, or hyphens) followed by a double-quoted display title:
- **Correct**: `subgraph SAMPLER_BLOCK ["pdm_sampler_top.v (Phase 2 RTL Module)"]`
- **Incorrect**: `subgraph pdm_sampler_top.v [Phase 2 RTL Module]` *(Dot `.v` and unquoted brackets crash the parser)*

### **4. Correct Shape Delimiter Quoting**
- **Cylinder / Database**: `DDR[("PS DDR3 Memory<br/>Circular Ring Buffer")]`
- **Rounded Box**: `NODE("Rounded Title")`
- **Stadium / Capsule**: `NODE(["Capsule Title"])`

### **5. Avoid Raw HTML Tags Beyond `<br/>`**
Do not use `<div>`, `<span>`, or other HTML tags inside Mermaid labels. Use `<br/>` strictly inside double-quoted strings for multi-line labels.

---

## 3. Pre-Response Verification Checklist

Before emitting a response or saving an artifact containing Mermaid:
1. Scan for any `[` or `]` inside edge pipes `|...|` and ensure they are double-quoted `|"..."|`.
2. Scan for any `(` or `)` inside node boxes `[...]` and ensure the label is double-quoted `["..."]`.
3. Check all `subgraph` lines to ensure no dots or spaces exist in the identifier.
4. Verify all shape definitions (`[(...)]`, `[...]`, `(...)`) have internal double quotes.

---

## 4. Reference
- Root Hub: [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md)
