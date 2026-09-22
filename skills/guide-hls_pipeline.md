# <span style="color: #88C0D0;">**Vitis HLS IP Creation & CLI Pipeline Guide**</span>

### <span style="color: #81A1C1;">**Filename:** `guide-hls_pipeline.md`</span>

| <span style="color: #EBCB8B;">**Field**</span> | <span style="color: #EBCB8B;">**Details**</span> |
| :--- | :--- |
| <span style="color: #EBCB8B;">**Author**</span> | devilsu |
| <span style="color: #EBCB8B;">**Created**</span> | 2026-09-19 20:25 PST |
| <span style="color: #EBCB8B;">**Modified**</span> | 2026-09-19 20:28 PST |
| <span style="color: #EBCB8B;">**Version**</span> | v1.1.0 |
| <span style="color: #EBCB8B;">**Status**</span> | Published |

---

## <span style="color: #88C0D0;">**Table of Contents**</span>
- [<span style="color: #88C0D0;">**1. Overview & Pipeline Flowchart**</span>](#1-overview--pipeline-flowchart)
  - [<span style="color: #81A1C1;">1.1 TL;DR</span>](#11-tldr)
  - [<span style="color: #81A1C1;">1.2 End-to-End Pipeline Flowchart</span>](#12-end-to-end-pipeline-flowchart)
- [<span style="color: #88C0D0;">**2. Prerequisites & Environment Setup**</span>](#2-prerequisites--environment-setup)
- [<span style="color: #88C0D0;">**3. Step-by-Step HLS IP Development Pipeline**</span>](#3-step-by-step-hls-ip-development-pipeline)
  - [<span style="color: #81A1C1;">3.1 Step 1: Write HLS C++ Core & Interface Directives</span>](#31-step-1-write-hls-c-core--interface-directives)
  - [<span style="color: #81A1C1;">3.2 Step 2: Verify Logic with C-Simulation (Testbench)</span>](#32-step-2-verify-logic-with-c-simulation-testbench)
  - [<span style="color: #81A1C1;">3.3 Step 3: Create the Batch Export TCL Script</span>](#33-step-3-create-the-batch-export-tcl-script)
  - [<span style="color: #81A1C1;">3.4 Step 4: Run Synthesis & Packaging via CLI (`vitis-run`)</span>](#34-step-4-run-synthesis--packaging-via-cli-vitis-run)
  - [<span style="color: #81A1C1;">3.5 Step 5: Import the IP into Vivado Block Design</span>](#35-step-5-import-the-ip-into-vivado-block-design)
- [<span style="color: #88C0D0;">**4. HLS Interface Pragmas Reference**</span>](#4-hls-interface-pragmas-reference)
- [<span style="color: #88C0D0;">**5. Best Practices, Diagnostics & Troubleshooting**</span>](#5-best-practices-diagnostics--troubleshooting)

---

## <span style="color: #88C0D0;">**1. Overview & Pipeline Flowchart**</span>

### <span style="color: #81A1C1;">**1.1 TL;DR**</span>
- Designs and synthesizes custom <span style="color: #EBCB8B;">**hardware IP cores from C++ source code**</span> targeting FPGA architectures using the Vitis HLS toolchain.

- Executes the entire compilation, C-simulation, RTL generation, and packaging workflow <span style="color: #EBCB8B;">**entirely from the command line**</span> via batch TCL scripts and `vitis-run`.
  - Produces a production-ready `<module_name>_ip.zip` package containing IP-XACT metadata (`component.xml`), out-of-context constraints (`.xdc`), and synthesized RTL.

- Seamlessly imports the resulting IP catalog into <span style="color: #EBCB8B;">**Vivado IP Integrator Block Designs**</span> through either the GUI repository manager or automated TCL commands.

### <span style="color: #81A1C1;">**1.2 End-to-End Pipeline Flowchart**</span>

```text
+----------------------------------------------------------------------------------------------------+
|                                    VITIS HLS CLI IP PIPELINE                                       |
|                                                                                                    |
|   +-----------------------+              +-----------------------+              +--------------+   |
|   | 1. C++ Core & Header  |------------->| 2. C-Sim Testbench    |------------->| 3. Batch TCL |   |
|   |    (.cpp / .h)        |   Validate   |    (g++ verification) |   Automate   |    (export)  |   |
|   +-----------------------+              +-----------------------+              +--------------+   |
|                                                                                         |          |
|                                                                                         | Run CLI  |
|                                                                                         v          |
|   +-----------------------+              +-----------------------+              +--------------+   |
|   | 6. Vivado Block Design|<-------------| 5. Packaged IP Core   |<-------------| 4. vitis-run |   |
|   |    (BD Integration)   |   Import IP  |    (.zip / Catalog)   |   Synthesize |    (Batch)   |   |
|   +-----------------------+              +-----------------------+              +--------------+   |
+----------------------------------------------------------------------------------------------------+
```

---

## <span style="color: #88C0D0;">**2. Prerequisites & Environment Setup**</span>

Before running any HLS commands or compilation scripts, ensure that your FPGA development tools environment is loaded into your active shell session:

```bash
source <xilinx_install_dir>/settings64.sh
```

| <span style="color: #EBCB8B;">**Tool / Environment**</span> | <span style="color: #EBCB8B;">**Required Version**</span> | <span style="color: #EBCB8B;">**Description**</span> |
| :--- | :--- | :--- |
| <span style="color: #EBCB8B;">**Vivado Design Suite**</span> | Compatible toolchain version | RTL synthesis, implementation, and Block Design integration |
| <span style="color: #EBCB8B;">**Vitis HLS Toolchain**</span> | Matching toolchain version | High-Level Synthesis compiler (`vitis-run --mode hls`) |
| <span style="color: #EBCB8B;">**Host C++ Compiler**</span> | GCC / G++ (C++14 or newer) | Standalone software C-simulation testbench compilation |

---

## <span style="color: #88C0D0;">**3. Step-by-Step HLS IP Development Pipeline**</span>

### <span style="color: #81A1C1;">**3.1 Step 1: Write HLS C++ Core & Interface Directives**</span>

Create your header (`.h`) and implementation (`.cpp`) files. Define appropriate HLS interface pragmas to declare how the core communicates with surrounding hardware logic:

#### <span style="color: #8FBCBB;">**Generic Implementation Template: `<module_name>.cpp`**</span>

```cpp
#include "<module_name>.h"

void <module_name>(ap_uint<32> in_data, ap_uint<32>& out_data) {
    #pragma HLS INTERFACE mode=ap_ctrl_none port=return
    #pragma HLS INTERFACE mode=axis port=in_data
    #pragma HLS INTERFACE mode=axis port=out_data
    #pragma HLS PIPELINE II=1

    // Core processing logic
    out_data = in_data;
}
```

#### <span style="color: #8FBCBB;">**Generic Header Template: `<module_name>.h`**</span>

```cpp
#ifndef MODULE_NAME_H_
#define MODULE_NAME_H_

#include <ap_int.h>

void <module_name>(ap_uint<32> in_data, ap_uint<32>& out_data);

#endif
```

---

### <span style="color: #81A1C1;">**3.2 Step 2: Verify Logic with C-Simulation (Testbench)**</span>

Before executing hardware synthesis, verify algorithmic behavior in software using a C++ testbench (`tb_<module_name>.cpp`):

```bash
g++ -O3 -I<xilinx_vitis_include_dir> \
    <module_name>.cpp \
    tb_<module_name>.cpp \
    -o tb_sim

./tb_sim
```

- <span style="color: #EBCB8B;">**Rapid Iteration**</span>: Software simulation runs in milliseconds without invoking synthesis engines.
- <span style="color: #EBCB8B;">**Self-Checking**</span>: Ensure the testbench returns `0` on success and non-zero on assertion failures.

---

### <span style="color: #81A1C1;">**3.3 Step 3: Create the Batch Export TCL Script**</span>

Create an automation script named `export_ip.tcl` in your IP source directory. This script configures project settings, target silicon, clocks, synthesis, and export packaging:

```tcl
# 1. Initialize project and reset any previous runs
open_project -reset prj_<module_name>

# 2. Set the top-level C++ function
set_top <top_function_name>

# 3. Add synthesis and testbench source files
add_files <module_name>.cpp
add_files -tb tb_<module_name>.cpp

# 4. Create solution targeted for Vivado
open_solution -reset "solution1" -flow_target vivado

# 5. Set target FPGA part
set_part {<target_part>}

# 6. Set target clock period in nanoseconds
create_clock -period <clock_period_ns> -name default

# 7. Run C-Synthesis to generate RTL (Verilog & VHDL)
csynth_design

# 8. Package the IP as a .zip archive in the current directory
export_design -format ip_catalog -rtl verilog -output <module_name>_ip.zip

exit
```

---

### <span style="color: #81A1C1;">**3.4 Step 4: Run Synthesis & Packaging via CLI (`vitis-run`)**</span>

Execute the batch script using the unified `vitis-run` CLI:

```bash
vitis-run --mode hls --tcl export_ip.tcl
```

#### <span style="color: #8FBCBB;">**Automated Synthesis Execution Stages**</span>:
- <span style="color: #EBCB8B;">**Compilation**</span>: Converts high-level C++ constructs into an intermediate hardware representation.
- <span style="color: #EBCB8B;">**Scheduling & Binding**</span>: Maps operations into discrete clock cycles based on timing constraints (`<clock_period_ns>`) and pipelining directives (`II=1`).
- <span style="color: #EBCB8B;">**RTL Generation**</span>: Produces synthesizable Verilog (`.v`) and VHDL (`.vhd`) inside `prj_<module_name>/solution1/syn/`.
- <span style="color: #EBCB8B;">**IP Packaging**</span>: Invokes the Vivado IP Packager to generate:
  - `component.xml` (standard IP-XACT schema definition)
  - Out-of-context constraints (`.xdc`)
  - GUI parameter customization metadata (`xgui/`)
  - Output archive: `<module_name>_ip.zip`

---

### <span style="color: #81A1C1;">**3.5 Step 5: Import the IP into Vivado Block Design**</span>

#### <span style="color: #8FBCBB;">**Option A: Using the Vivado GUI**</span>
1. Open your Vivado project.
2. In the **Flow Navigator**, select **Settings** (under *Project Manager*).
3. Expand **IP** and click **Repository**.
4. Click the **+** (Add) icon and select the directory containing your `.zip` archive (or extract the zip into a folder first).
5. Click **Apply** -> **OK**.
6. Open your Block Design (`.bd`), click the **+** icon in the diagram, search for your IP name (e.g., `<module_name>`), and instantiate it.

#### <span style="color: #8FBCBB;">**Option B: Using the Vivado TCL Console**</span>

```tcl
# Add the IP repository path to the current project
set_property ip_repo_paths [list /path/to/<ip_directory>] [current_project]
update_ip_catalog

# Instantiate the IP cell into an active Block Design
create_bd_cell -type ip -vlnv <vendor>:hls:<module_name>:1.0 <instance_name>
```

---

## <span style="color: #88C0D0;">**4. HLS Interface Pragmas Reference**</span>

| <span style="color: #EBCB8B;">**Pragma Mode**</span> | <span style="color: #EBCB8B;">**Port Target**</span> | <span style="color: #EBCB8B;">**Hardware Protocol**</span> | <span style="color: #EBCB8B;">**Typical Use Case**</span> |
| :--- | :--- | :--- | :--- |
| `ap_ctrl_none` | `port=return` | Pure free-running logic | Pipelines executing continuously on clock edges without block-level control handshakes |
| `s_axi_control` | `port=return` | AXI4-Lite Slave | Register interface for software-accessible configuration, status, and control |
| `ap_none` | `port=<name>` | Raw wire (no protocol) | Direct hardware wire connections and discrete control/status signals |
| `axis` | `port=<name>` | AXI4-Stream | High-speed data streaming with packet handshaking (`TVALID`/`TREADY`) |
| `m_axi` | `port=<name>` | AXI4 Memory-Mapped Master | Direct Memory Access (DMA) master transactions to system memory |
| `ap_fifo` | `port=<name>` | Synchronous FIFO interface | Read/write interface with standard FIFO handshaking |

#### <span style="color: #8FBCBB;">**Core Pipelining Directives**</span>:
- `#pragma HLS PIPELINE II=1`: Directs the scheduler to achieve an <span style="color: #EBCB8B;">**Initiation Interval of 1**</span> (accepts a new input sample and emits an output sample on every single clock edge).
- `#pragma HLS UNROLL factor=<N>`: Unrolls inner loops to parallelize compute elements across silicon DSP slices.

---

## <span style="color: #88C0D0;">**5. Best Practices, Diagnostics & Troubleshooting**</span>

- <span style="color: #EBCB8B;">**Review Synthesis Reports**</span>: Inspect `prj_<module_name>/solution1/syn/report/<top_function_name>_csynth.rpt` after synthesis. Always verify that:
  - Worst-Case Slack (WCS) is positive (`Slack > 0.0 ns`).
  - Target Initiation Interval meets your specification (`II = 1`).
  - Resource utilization (BRAM, DSP, FF, LUT) fits comfortably within target FPGA boundaries.

- <span style="color: #EBCB8B;">**Match Clock Domains**</span>: Ensure that the clock period specified in `create_clock -period <clock_period_ns>` in `export_ip.tcl` matches the physical clock frequency driven into `ap_clk` in Vivado Block Design.

- <span style="color: #EBCB8B;">**Safe Workspace Cleanup**</span>: The temporary synthesis directory `prj_<module_name>/` can be safely deleted once `export_design` finishes; only the `.zip` archive or extracted IP catalog directory is required for Vivado integration.
