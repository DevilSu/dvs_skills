# dvs_skills

A curated collection of technical blueprints, guides, and engineering standards for hardware/software co-design, FPGA workflows, and development planning.

## 📁 Repository Structure

```text
dvs_skills/
├── README.md
├── rules/
│   └── git.md
└── skills/
    ├── guide-create_a_plan.md
    ├── guide-hls_pipeline.md
    ├── connect_to_vivado.md
    ├── connect_to_PYNQ.md
    └── planning.md
```

## 📋 Rules & Development Standards

- **[Git Commit & Push Confirmation Rule](rules/git.md)**: Mandatory requirement to always ask for explicit user confirmation before executing any `git commit` or `git push` commands.
- **[Pre-Authoring Planning Alignment Directive](skills/planning.md)**: Mandatory requirement to always discuss and align with the user before writing or modifying any `.md` plan files.

## 📚 Guides Included

- **[Plan Specification & Authoring Guide](skills/guide-create_a_plan.md)**: A standardized blueprint and styling standard for writing technical specifications, software design documents, and engineering implementation plans.
- **[Vitis HLS IP Creation & CLI Pipeline Guide](skills/guide-hls_pipeline.md)**: An end-to-end guide detailing C++ synthesis, directives, testbench simulation, TCL automation, and CLI batch packaging (`vitis-run`) for AMD/Xilinx Vivado workflows.
- **[Vivado MCP Server Guide](skills/connect_to_vivado.md)**: Dual-modality guide for `vivado-gui` and `vivado-headless` servers.
- **[PYNQ Board Connection Guide](skills/connect_to_PYNQ.md)**: Network configuration, SSH credentials, and environment verification for PYNQ-Z2 hardware.
- **[Pre-Authoring Planning Directive](skills/planning.md)**: Directive to align with the user interactively in chat before authoring markdown specifications.
