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
    └── guide-hls_pipeline.md
```

## 📋 Rules & Development Standards

- **[Git Commit & Push Confirmation Rule](rules/git.md)**: Mandatory requirement to always ask for explicit user confirmation before executing any `git commit` or `git push` commands.

## 📚 Guides Included

- **[Plan Specification & Authoring Guide](skills/guide-create_a_plan.md)**: A standardized blueprint and styling standard for writing technical specifications, software design documents, and engineering implementation plans.
- **[Vitis HLS IP Creation & CLI Pipeline Guide](skills/guide-hls_pipeline.md)**: An end-to-end guide detailing C++ synthesis, directives, testbench simulation, TCL automation, and CLI batch packaging (`vitis-run`) for AMD/Xilinx Vivado workflows.
