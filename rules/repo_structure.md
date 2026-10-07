# Subproject-Driven Repository Architecture Standard

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-REPO-001` |
| **Topic** | Repository & Subproject Directory Architecture |
| **Created** | 2026-10-06 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **All hardware/software co-design, FPGA, and embedded streaming repositories MUST adhere to the standardized Subproject-Driven Tripartite Architecture (`blueprints/`, `hw/`, `sw/`).**
> Projects must maintain strict symmetry across architectural specifications, hardware synthesis/overlays, and software drivers by isolating functional milestones into dedicated, matching subproject directories.

---

## 2. Standard Repository Hierarchy

Every repository must follow the architectural layout established in `mic_arr_v2`:

```text
<repository_root>/
├── readme.md                           # Master repository guide, Mermaid roadmap & status dashboard
├── .gitignore                          # Global toolchain, intermediate build & log ignore rules
│
├── blueprints/                         # Architectural blueprints & engineering specifications
│   └── <subproject_name>/              # Dedicated directory per major milestone/subproject
│       ├── overall.md                  # Master architecture blueprint & cross-phase roadmap
│       ├── phase_1..N.md               # Detailed execution blueprints for individual phases
│       ├── summary.md                  # Post-execution comprehensive report, register maps & benchmarks
│       └── backup_evidences.md         # Empirical verification evidence (waveforms, logs, timing) shared across all phases
│
├── hw/                                 # Hardware designs, RTL, IPs & Overlays
│   ├── common/                         # Shared board definitions, constraints & global IP catalog
│   │   ├── board_files/                # Vendor / carrier board definition files
│   │   └── ip_repo/                    # Reusable packaged IP catalog
│   └── subprojects/                    # Subproject hardware modules
│       └── <subproject_name>/          # Matches blueprints/<subproject_name>
│           ├── vitis_hls/              # HLS C++ sources, directives & batch TCL export scripts
│           ├── vivado/                 # Vivado project files (.xpr), block designs (.bd)
│           └── overlay/                # Production bitstream (.bit), hardware handoff (.hwh, .xsa)
│
└── sw/                                 # Embedded software, drivers, test suites & deployment
    ├── common/                         # Shared utility libraries, timing meters, protocol parsers
    └── subprojects/                    # Subproject software packages
        └── <subproject_name>/          # Matches blueprints/<subproject_name>
            ├── deploy_overlay.sh       # Target deployment / board sync script
            ├── test_*.py               # Component & unit verification test scripts
            ├── benchmark_*.py          # Performance & throughput sweep scripts
            └── benchmark_results.json  # Bit-exact measurements & latency benchmarks
```

---

## 3. Directory Mirroring & Subproject Alignment

1. **Subproject Name Symmetry**:
   - The identifier `<subproject_name>` must match identically across all three domains:
     - `blueprints/<subproject_name>/`
     - `hw/subprojects/<subproject_name>/`
     - `sw/subprojects/<subproject_name>/`
2. **Decoupled Isolation**:
   - Each subproject directory in `hw/subprojects/` and `sw/subprojects/` must be self-contained. Freezing or completing an earlier milestone must never break subsequent subprojects.
3. **Common vs. Subproject Scope**:
   - Only place assets in `hw/common/` or `sw/common/` if they are strictly reused across multiple subprojects without modification.

---

## 4. Root Dashboard (`readme.md`) Requirements

Every repository's root `readme.md` must include:
1. **Executive Scope**: Clear overview of the platform, target SoC, and primary engineering objectives.
2. **Repository Tree**: Rendered ASCII folder structure mapping out active and planned subprojects.
3. **Mermaid Multi-Stage Roadmap**: Horizontal `graph LR` depicting stages styled with Nord status classes (`done`, `next`, `future`), verified per `RULE-MERMAID-001`.
4. **Subproject Breakdown Table**: Tabulating Stage, Subproject Name, Description, Status, and Relative Link to its `blueprints/<subproject>/overall.md` or `summary.md`. Blueprint links in the table must display concisely as `[Link](blueprints/<subproject>/overall.md)` rather than repeating the filename `overall.md` across every stage row, preventing visual confusion.

---

## 5. References & Cross-Standards

- **Root Hub**: [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md)
- **Step Verification Evidence Standard**: [`backup_evidences.md`](backup_evidences.md)
- **Plan Specification Guide**: [`../skills/guide-create_a_plan.md`](../skills/guide-create_a_plan.md)
- **Planning Directive**: [`../skills/planning.md`](../skills/planning.md)
- **Git Protocol**: [`git.md`](git.md)
