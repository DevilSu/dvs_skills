# Implementation Step Verification & Backup Evidence Standard

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-EVID-001` |
| **Topic** | Implementation Step Verification & Empirical Evidence Standard |
| **Created** | 2026-10-06 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **After completing ANY implementation step within an engineering plan, the AI assistant MUST provide concrete empirical evidence proving that the step completed correctly.**
> This evidence MUST be recorded in a dedicated markdown document named `backup_evidences.md` located under the subproject's blueprint directory:
> `blueprints/<subproject_name>/backup_evidences.md`

---

## 2. Blueprint Subproject Architecture Standard

Aligned with `RULE-REPO-001` ([`repo_structure.md`](repo_structure.md)), every subproject blueprint directory (`blueprints/<subproject_name>/`) maintains a standardized four-tier document set:

```text
blueprints/<subproject_name>/
├── overall.md                  # Master architecture blueprint & cross-phase roadmap
├── phase_1..N.md               # Detailed execution blueprints for individual phases
├── summary.md                  # Post-execution comprehensive report, register maps & benchmarks
└── backup_evidences.md         # Single shared repository of empirical evidence across all phases
```

### Key Principles of `backup_evidences.md`:
1. **Single Shared File Per Subproject**:
   - `backup_evidences.md` is **shared across all phases and plan documents** under the subproject folder.
   - Do **NOT** create separate evidence files per phase (e.g. do not create `phase_1_evidences.md`, `phase_2_evidences.md`). All step verification evidence across Phase 1, Phase 2, ..., Phase N accumulates chronologically into the single `backup_evidences.md`.
2. **Standard Subproject Triumvirate**:
   - For any subproject milestone, the standard blueprint documentation package consists of:
     - One **`overall.md`** (high-level vision, hardware-software partitioning, phase breakdown)
     - Individual **`phase_N.md`** files (step-by-step actionable roadmaps)
     - One **`summary.md`** (final retrospective, sign-off metrics, register maps)
     - One **`backup_evidences.md`** (concrete verification traces, waveforms, timing analysis, test logs)
3. **Cross-Document Linking**:
   - In each `phase_N.md`, the step's verification checklist must link directly to the corresponding section anchor in `backup_evidences.md`.
   - In `backup_evidences.md`, each step entry must link back to its corresponding step specification in `phase_N.md`.

---

## 3. Acceptable Forms of Concrete Evidence

Asserting that code "looks correct" or "compiles without syntax errors" is insufficient. Every completed step must be backed by empirical evidence matching the domain:

| Domain | Mandatory Evidence Types | Unacceptable Evidence |
| :--- | :--- | :--- |
| **FPGA / RTL Design** | - Waveform captures (GTKWave/Vivado screenshot or plotted digital traces `.png`)<br>- Quantitative simulation logs showing zero assertion failures across test vectors<br>- Clock domain & strobe timing analysis (setup/hold slack, center-of-eye margin in ns) | - "RTL compiles cleanly in syntax check"<br>- Unexecuted testbench code blocks |
| **Vitis HLS / Algorithms** | - C-simulation bit-exact convergence logs (max error = 0 LSB, SNR dB vs golden model)<br>- C/RTL co-simulation pass logs with transaction counts<br>- Synthesis latency, initiation interval (II), and resource utilization tables | - "C++ algorithm compiled in g++"<br>- Hypothetical DSP/BRAM estimates without tool reports |
| **Vivado Implementation** | - Post-routing timing summary report (WNS > 0 ns, TNS = 0 ns, WHS > 0 ns)<br>- Design Rule Check (DRC) report showing 0 fatal errors / violations<br>- Implemented resource utilization breakdown (LUTs, FFs, BRAMs, DSPs, MMCMs) | - "Bitstream generation completed without reading timing"<br>- Ignoring setup/hold violations |
| **Embedded SW / PYNQ** | - Live hardware register readbacks / memory dumps via MMIO<br>- DMA circular buffer transfer logs with sample counts<br>- End-to-end benchmark results (latency histograms, FPS, throughput MB/s) | - "Python script executed with no uncaught exception"<br>- Synthetic software loop without hardware access |

---

## 4. Required Entry Structure in `backup_evidences.md`

Every step entry in `backup_evidences.md` must follow this standardized template:

```markdown
### Step X.Y: [Step Name]
- **Phase**: [Link to phase_N.md](phase_N.md#step-xy-step-name)
- **Completed**: YYYY-MM-DD HH:MM:SS
- **Status**: VERIFIED PASS

#### 1. Verification Objective
Brief description of what was tested and what constitutes passing criteria.

#### 2. Quantitative Results & Logs
Console log excerpts, test assertions, or numeric verification metrics.

#### 3. Visual & Timing Evidence
Embedded waveform diagram, timing budget breakdown, or logic analyzer trace.
Always use relative repository paths for embedded assets:
![Waveform Description](../../hw/subprojects/<subproject>/sim/<waveform>.png)

#### 4. Reproducibility Command
The exact command line or TCL script used to reproduce the verification:
```bash
./run_sim_<step>.sh
```
```

---

## 5. Verification Workflow & Sequence

When executing an implementation step from a plan:

```text
[ Execute Implementation Step ]
             │
             ▼
[ Run Simulation / Co-Sim / Synthesis / Hardware Test ]
             │
             ▼
[ Capture Empirical Evidence (Logs, Waveforms, Timing Reports) ]
             │
             ▼
[ Append Entry to `blueprints/<subproject_name>/backup_evidences.md` ]
             │
             ▼
[ Update Step Status & Timestamp in `phase_N.md` ]
             │
             ▼
[ Present Summary to User with Link to `backup_evidences.md` ]
```

1. **Step Execution**: Write the RTL, HLS, TCL, or software code as specified in `phase_N.md`.
2. **Empirical Verification**: Run the automated simulation, synthesis, or hardware execution script. Collect the log, waveform image, or timing summary.
3. **Document Evidence**: Append the step's evidence section into `blueprints/<subproject_name>/backup_evidences.md`.
4. **Update Plan**: Update the checklist item in `phase_N.md` with completion timestamp and a relative link to `backup_evidences.md`.
5. **Present to User**: Keep the chat response concise (<= 3 paragraphs per `RULE-TALK-001`), linking directly to `backup_evidences.md`.

---

## 6. References & Cross-Standards

- **Root Hub**: [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md)
- **Repository Architecture**: [`repo_structure.md`](repo_structure.md)
- **Plan Authoring Guide**: [`../skills/guide-create_a_plan.md`](../skills/guide-create_a_plan.md)
- **Planning Directive**: [`../skills/planning.md`](../skills/planning.md)
- **Git Protocol**: [`git.md`](git.md)
