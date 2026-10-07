# dvs_skills

A curated collection of technical blueprints, guides, and engineering standards for hardware/software co-design, FPGA workflows, and development planning.

## 📁 Repository Structure

```text
dvs_skills/
├── README.md
├── rules/
│   ├── how_to_talk_to_dvs.md
│   ├── git.md
│   ├── mermaid.md
│   ├── repo_structure.md
│   ├── backup_evidences.md
│   ├── hardware_architecture.md
│   └── codebase_hygiene.md
└── skills/
    ├── guide-create_a_plan.md
    ├── guide-hls_pipeline.md
    ├── connect_to_vivado.md
    ├── connect_to_PYNQ.md
    └── planning.md
```

## 📋 Rules & Development Standards

- **[How to Talk to DVS (Root Rules Hub)](rules/how_to_talk_to_dvs.md)**: Master communication rule (>3 paragraphs in formatted editor tabs, zero unrendered LaTeX, closing prompts for plan updates, shortnames glossary `addr cmt` / `acp`) and the ingestion protocol for adding/updating future rules.
- **[Codebase Hygiene & Obsolete Artifact Lifecycle Standard](rules/codebase_hygiene.md)**: Mandatory immediate decommissioning of superseded source files upon refactoring (`RULE-HYGIENE-001`), zero orphaned code, build tool collision prevention, and cross-document reference sweeps.
- **[Subproject-Driven Repository Architecture Standard](rules/repo_structure.md)**: Mandatory tripartite architecture (`blueprints/`, `hw/`, `sw/`) isolating hardware/software co-design milestones into mirrored subprojects, modeled after `mic_arr_v2`.
- **[FPGA Clock Distribution & Streaming Standard](rules/hardware_architecture.md)**: Mandatory clock infrastructure decoupling (leaf modules never synthesize clocks), non-blocking real-time physical sensor streaming, and vendor IP catalog reuse.
- **[Implementation Step Verification & Backup Evidence Standard](rules/backup_evidences.md)**: Mandatory empirical verification evidence (waveforms, timing slack, zero-error test logs) documented in a single shared `backup_evidences.md` under each subproject blueprint directory after completing any implementation step.
- **[Mermaid Diagram Validation Rule](rules/mermaid.md)**: Mandatory syntax verification and double-quoting requirements for all Mermaid diagrams to prevent rendering parser failures.
- **[Git Commit & Push Confirmation Rule](rules/git.md)**: Mandatory requirement to always ask for explicit user confirmation before executing any `git commit` or `git push` commands.
- **[Pre-Authoring Planning Alignment Directive](skills/planning.md)**: Mandatory requirement to always discuss and align with the user before writing or modifying any `.md` plan files.

## 📚 Guides Included

- **[Plan Specification & Authoring Guide](skills/guide-create_a_plan.md)**: A standardized blueprint and styling standard for writing technical specifications, software design documents, and engineering implementation plans.
- **[Vitis HLS IP Creation & CLI Pipeline Guide](skills/guide-hls_pipeline.md)**: An end-to-end guide detailing C++ synthesis, directives, testbench simulation, TCL automation, and CLI batch packaging (`vitis-run`) for AMD/Xilinx Vivado workflows.
- **[Vivado MCP Server Guide](skills/connect_to_vivado.md)**: Dual-modality guide for `vivado-gui` and `vivado-headless` servers.
- **[PYNQ Board Connection Guide](skills/connect_to_PYNQ.md)**: Network configuration, SSH credentials, and environment verification for PYNQ-Z2 hardware.
- **[Pre-Authoring Planning Directive](skills/planning.md)**: Directive to align with the user interactively in chat before authoring markdown specifications.
