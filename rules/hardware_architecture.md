# FPGA Clock Distribution, Sensor Ingestion, & Streaming Architecture Standard

| Field | Details |
| :--- | :--- |
| **Rule ID** | `RULE-HW-001` |
| **Topic** | Clock Distribution Hierarchy, Real-Time Sensor Ingestion, & Non-Blocking Streaming |
| **Created** | 2026-10-06 |
| **Status** | **Active / Mandatory** |

---

## 1. Rule Statement

> [!IMPORTANT]
> **1. Leaf processing and ingestion modules MUST NEVER generate, synthesize, or divide system clocks, nor drive external board clock pins.**
> Clock generation, division, buffering, and distribution belong strictly to top-level clocking infrastructure (Vivado Block Design / MMCM / PLL / vendor IP catalog blocks). Leaf modules must be pure consumers of system and interface clocks.
>
> <br>
>
> **2. Real-time physical sensor interfaces MUST operate in a strictly non-blocking regime.**
> Physical transducers (microphones, ADCs, image sensors, RF receivers) cannot be paused by downstream FPGA logic. Front-end samplers and early-stage decimation filters must run free-flowing without backpressure stalls. All elasticity buffering, FIFOs, and flow control must reside downstream after rate reduction.
>
> <br>
>
> **3. Always prioritize standard vendor IP catalog blocks for structural infrastructure over custom RTL reinvention.**

---

## 2. Principle 1: Clock Infrastructure & Leaf Module Decoupling

### 2.1 The Software Encapsulation Anti-Pattern in Hardware Design
In software engineering, encapsulation encourages bundling everything related to a peripheral into a single self-contained class. Applying this mindset to FPGA design causes fatal architectural defects:
- **The Anti-Pattern**: Creating a "smart" ingestion module that accepts an unrelated reference clock, divides it with internal flip-flops, drives external chip pins, and synchronizes data internally.
- **Why It Fails**:
  1. **Invisible Clock Nets**: Clocks generated inside leaf RTL modules bypass top-level Block Design visualization, making the real system clock frequency invisible.
  2. **Clock Tree Violations**: Fabric flip-flop division without dedicated clock buffers (`BUFG`) routes clock signals over general fabric routing lines, creating massive skew, jitter, and static timing analysis (STA) closure nightmares.
  3. **Broken Reusability**: If another module or diagnostic probe on the FPGA needs the same clock, it must tap into a leaf module output rather than a clean system clock net.

### 2.2 Standard Clocking Partitioning
Hardware designs must maintain strict separation between **Clock Infrastructure** and **Data Processing**:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                      TOP-LEVEL CLOCK INFRASTRUCTURE                    │
│                        (Vivado Block Design / Top)                     │
│                                                                        │
│   +------------------+  Reference   +--------------------+  3.84 MHz   │
│   | Clocking Wizard  |------------->| Vendor IP Divider  |--------+    │
│   | (MMCM / PLL)     |   (7.68 MHz) | (c_counter_binary) |        |    │
│   +------------------+              +--------------------+        |    │
└───────────────────────────────────────────────────────────────────┼────┘
                                                                    |
                                        +---------------------------+
                                        | System Clock Distribution
                                        v
┌───────────────────────────────────────┴────────────────────────────────┐
│                      LEAF DATA PROCESSING MODULES                      │
│                                                                        │
│   +------------------------------------+                               │
│   | External Transducer / PMOD Pins    |<--- Driven by 3.84 MHz net    │
│   +------------------------------------+                               │
│                                                                        │
│   +------------------------------------+                               │
│   | Sensor Ingestion Leaf Module       |<--- Pure consumer of 3.84 MHz │
│   | (pdm_sampler / adc_ingest)         |     (NO clock division inside)│
│   +------------------------------------+                               │
└────────────────────────────────────────────────────────────────────────┘
```

### 2.3 Handling MMCM / PLL Minimum Frequency DRC Limits
When vendor clocking primitives (e.g. Xilinx 7-Series MMCM with minimum VCO output frequency of 4.687 MHz) cannot directly output low frequencies (e.g. 3.84 MHz):
- **CORRECT Approach**: Synthesize a legal harmonic (e.g. 2x, 7.68 MHz) with the Clocking Wizard, place a standard vendor IP divider (**`c_counter_binary`**, configured as a 1-bit toggle counter) in the Block Design, and distribute the resulting clock net to both external pins and leaf modules.
- **INCORRECT Approach**: Pipe the high-frequency reference into a leaf module and write custom flip-flop divider RTL inside the leaf module.

---

## 3. Principle 2: Real-Time Physical Sensor Streaming & Non-Blocking Semantics

### 3.1 The Physical Reality of Real-Time Transducers
Physical sensors operate in physical time. An analog microphone array, camera sensor, or ADC does not pause when a downstream AXI-Stream consumer drops `tready`:
- If an ingestion module pauses its sampling state machine because `m_axis_tready == 0`, incoming physical samples are silently dropped or overwritten.
- Stalling front-end filters (such as CIC decimators or FIR filters) corrupts their internal integrator/comb delay lines, introducing catastrophic phase distortion and audio glitches.

### 3.2 Non-Blocking Front-End Rules
1. **Free-Flowing Sampling**: Front-end sampling logic and decimation filters must process every incoming sample unconditionally at the native sensor rate.
2. **Strobe-Mode AXI-Stream**: The interface between front-end samplers and decimation filters should operate in non-blocking mode: `tvalid` asserts with each new sample; the receiver must be architected to accept samples without introducing backpressure stalls.
3. **Buffer Placement Boundary**:
   - **Never buffer at the high-frequency raw sensor rate** (e.g. multi-MHz 1-bit PDM streams). It wastes precious Block RAM and adds latency.
   - **Always buffer downstream after rate reduction / decimation** (e.g. in the 96 kHz PCM domain). Place elastic FIFOs (`axis_data_fifo`) and DMA engines (`axi_dma`) where bandwidth is low and samples can be cleanly packetized into memory frames with drop-count logging.

```text
 Physical Sensors
 (Continuous Time)
        │
        ▼
┌────────────────────────┐
│ Ingestion Sampler Leaf │  <-- Non-Blocking (Free-flowing real-time sampling)
└──────────┬─────────────┘
           │ Native High-Rate Stream
           ▼
┌────────────────────────┐
│ Decimation / DSP Core  │  <-- Non-Blocking (Math runs continuously, zero stalls)
└──────────┬─────────────┘
           │ Decimated Low-Rate Stream (e.g. 96 kHz PCM)
           ▼
═══════════════════════════════ [ ELASTIC BUFFERING BOUNDARY ]
           │
           ▼
┌────────────────────────┐
│ Frame Builder & FIFO   │  <-- Elastic Buffering & Flow Control (Backpressure allowed here!)
└──────────┬─────────────┘
           │ AXI-Stream with Backpressure (tready / tvalid)
           ▼
┌────────────────────────┐
│ AXI DMA & Host Memory  │  <-- Memory latencies and software ring buffers absorbed cleanly
└────────────────────────┘
```

---

## 4. Principle 3: Standard Toolchain IP Reuse

When assembling FPGA architectures, always check the vendor IP catalog before writing custom RTL:

| Common Requirement | Antipattern (Custom RTL) | Preferred Solution (Vivado IP Catalog) |
| :--- | :--- | :--- |
| **Clock Frequency Division** | Custom toggle flip-flop in Verilog | **`c_counter_binary:12.0`** (1-bit counter) or Clocking Wizard |
| **Clock Buffering & Gating** | Instantiating raw logic gates on clocks | **`util_ds_buf:2.2`** (`BUFG`, `IBUFDS`, `OBUFDS`) |
| **Stream Elastic Buffering** | Custom circular BRAM register FIFO | **`axis_data_fifo:2.0`** (Parameterized depth, CDC-safe) |
| **Width Conversion / Packing** | Custom shift-register serializers | **`axis_dwidth_converter:1.1`** |
| **Multi-Clock Crossings** | Ad-hoc dual-flip-flop chains on buses | **`axis_clock_converter:1.1`** or `xpm_cdc_fifo` |

---

## 5. Architectural Pre-Implementation Checklist

Before finalizing any hardware architecture plan or writing RTL, verify:

- [ ] **Clock Ownership**: Are all clocks generated and divided in top-level infrastructure (Block Design / MMCM / vendor IPs) rather than inside leaf modules?
- [ ] **Leaf Module Purity**: Are leaf modules purely consumers of clock nets, with zero internal clock division or external clock pin driving?
- [ ] **Non-Blocking Ingestion**: Is the physical sensor front-end strictly non-blocking?
- [ ] **Decoupled Buffering**: Is elasticity buffering placed strictly after rate reduction / decimation rather than at the raw physical sensor interface?
- [ ] **IP Reuse**: Were standard toolchain catalog IPs considered and utilized before writing custom infrastructure RTL?

---

## 6. References & Cross-Standards

- **Root Hub**: [`how_to_talk_to_dvs.md`](how_to_talk_to_dvs.md)
- **Repository Architecture**: [`repo_structure.md`](repo_structure.md)
- **Step Verification Evidence**: [`backup_evidences.md`](backup_evidences.md)
- **Plan Specification Guide**: [`../skills/guide-create_a_plan.md`](../skills/guide-create_a_plan.md)
- **Planning Directive**: [`../skills/planning.md`](../skills/planning.md)
