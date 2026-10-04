# <span style="color: #88C0D0;">**PYNQ Board Ethernet, SSH & Remote Automation Connection Guide**</span>

### <span style="color: #81A1C1;">**Filename:** `connect_to_PYNQ.md`</span>

| <span style="color: #EBCB8B;">**Field**</span> | <span style="color: #EBCB8B;">**Details**</span> |
| :--- | :--- |
| <span style="color: #EBCB8B;">**Author**</span> | devilsu |
| <span style="color: #EBCB8B;">**Created**</span> | 2026-10-03 20:25 PDT |
| <span style="color: #EBCB8B;">**Modified**</span> | 2026-10-03 20:25 PDT |
| <span style="color: #EBCB8B;">**Version**</span> | v1.0.0 |
| <span style="color: #EBCB8B;">**Status**</span> | <span style="color: #A3BE8C;">**Active Reference & Health-Check Guide**</span> |
| <span style="color: #EBCB8B;">**Target Hardware**</span> | PYNQ-Z1 / PYNQ-Z2 / Zynq-7000 Boards |
| <span style="color: #EBCB8B;">**Target OS**</span> | PYNQ Linux (Ubuntu 18.04 / 20.04 / 22.04 LTS on ARM Cortex-A9) |

---

## <span style="color: #88C0D0;">**Table of Contents**</span>
- [<span style="color: #88C0D0;">**1. Purpose & Usage Model**</span>](#1-purpose--usage-model)
- [<span style="color: #88C0D0;">**2. Network Architecture & Subnet Topology**</span>](#2-network-architecture--subnet-topology)
- [<span style="color: #88C0D0;">**3. Fresh Host Setup (One-Time Configuration)**</span>](#3-fresh-host-setup-one-time-configuration)
  - [<span style="color: #81A1C1;">3.1 Identify Ethernet Interface</span>](#31-identify-ethernet-interface)
  - [<span style="color: #81A1C1;">3.2 Create Dedicated Permanent Network Profile</span>](#32-create-dedicated-permanent-network-profile)
  - [<span style="color: #81A1C1;">3.3 Instant Temporary Fallback (Without Profile)</span>](#33-instant-temporary-fallback-without-profile)
- [<span style="color: #88C0D0;">**4. SSH & Automation Credentials Setup**</span>](#4-ssh--automation-credentials-setup)
  - [<span style="color: #81A1C1;">4.1 Default PYNQ Credentials</span>](#41-default-pynq-credentials)
  - [<span style="color: #81A1C1;">4.2 Passwordless SSH Key Installation</span>](#42-passwordless-ssh-key-installation)
  - [<span style="color: #81A1C1;">4.3 Passwordless Sudo Configuration (Crucial for DMA/Xlnk)</span>](#43-passwordless-sudo-configuration-crucial-for-dmaxlnk)
- [<span style="color: #88C0D0;">**5. Session Health-Check Procedure (Run in Fresh Sessions)**</span>](#5-session-health-check-procedure-run-in-fresh-sessions)
  - [<span style="color: #81A1C1;">5.1 Automated 6-Point Readiness Script</span>](#51-automated-6-point-readiness-script)
  - [<span style="color: #81A1C1;">5.2 Step-by-Step Manual Diagnosis</span>](#52-step-by-step-manual-diagnosis)
- [<span style="color: #88C0D0;">**6. Troubleshooting Matrix**</span>](#6-troubleshooting-matrix)

---

## <span style="color: #88C0D0;">**1. Purpose & Usage Model**</span>

This document serves two primary operational roles:

1. <span style="color: #EBCB8B;">**Fresh PC Setup Blueprint**</span>: Step-by-step instructions to configure a new Linux workstation to reliably interface with a directly-connected PYNQ-Z2 board without affecting standard Wi-Fi or office network routing.
2. <span style="color: #EBCB8B;">**Fresh Session Verification Checklist**</span>: When a user starts a new development session and asks <span style="color: #EBCB8B;">**"Check if PYNQ is connected and ready using connect_to_PYNQ.md"**</span>, the AI agent or engineer executes the verification steps in Section 5 to confirm hardware link, SSH access, root permissions, and PYNQ runtime health.

---

## <span style="color: #88C0D0;">**2. Network Architecture & Subnet Topology**</span>

When the PYNQ-Z2 is connected directly to a PC with an Ethernet patch cable (without a DHCP router), the board defaults to its factory static fallback address:

```text
+------------------------------------+             +------------------------------------+
|          HOST WORKSTATION          |             |           PYNQ-Z2 BOARD            |
|                                    |             |                                    |
|   Interface:  enp61s0 (or eth0)    |   Ethernet  |   Interface:  eth0                 |
|   Static IP:  192.168.2.1          |<----------->|   Fallback:   192.168.2.99         |
|   Netmask:    255.255.255.0 (/24)  |  Direct RJ45|   Netmask:    255.255.255.0 (/24)  |
|   Gateway:    NONE (Preserve Wi-Fi)|             |   SSH Port:   22                   |
|   DNS:        NONE                 |             |   Jupyter:    9090                 |
+------------------------------------+             +------------------------------------+
```

> [!IMPORTANT]
> **Never set a default gateway on the PYNQ Ethernet connection.** Leaving the gateway blank ensures that all regular internet traffic remains seamlessly routed through your primary Wi-Fi or corporate network interface (`wlo1` / `wlan0`).

---

## <span style="color: #88C0D0;">**3. Fresh Host Setup (One-Time Configuration)**</span>

### <span style="color: #81A1C1;">**3.1 Identify Ethernet Interface**</span>

Plug the Ethernet cable from the PYNQ board into the PC. Run:

```bash
nmcli device status
```

Identify the wired Ethernet device name (typically `enp61s0`, `enp0s31f6`, or `eth0`).

### <span style="color: #81A1C1;">**3.2 Create Dedicated Permanent Network Profile**</span>

Create an autoconnecting NetworkManager profile that automatically binds to the board without conflicting with standard network routes:

```bash
sudo nmcli connection add type ethernet ifname enp61s0 con-name "PYNQ-Direct" ip4 192.168.2.1/24 ipv4.never-default yes connection.autoconnect yes
sudo nmcli connection up "PYNQ-Direct"
```

- <span style="color: #EBCB8B;">`ip4 192.168.2.1/24`</span>: Assigns host static IP on the PYNQ subnet.
- <span style="color: #EBCB8B;">`ipv4.never-default yes`</span>: Prevents NetworkManager from ever routing default internet traffic over this cable.
- <span style="color: #EBCB8B;">`connection.autoconnect yes`</span>: Reconnects automatically whenever the board is powered on or plugged in.

### <span style="color: #81A1C1;">**3.3 Instant Temporary Fallback (Without Profile)**</span>

If `nmcli` is unavailable or you need an instant one-line test without saving a permanent profile:

```bash
sudo ip addr add 192.168.2.1/24 dev enp61s0
```

*(Persists until the next reboot or cable unplug).*

---

## <span style="color: #88C0D0;">**4. SSH & Automation Credentials Setup**</span>

### <span style="color: #81A1C1;">**4.1 Default PYNQ Credentials**</span>

- <span style="color: #EBCB8B;">**Username**</span>: `xilinx`
- <span style="color: #EBCB8B;">**Password**</span>: `xilinx`
- <span style="color: #EBCB8B;">**IP Address**</span>: `192.168.2.99`
- <span style="color: #EBCB8B;">**SSH Port**</span>: `22`
- <span style="color: #EBCB8B;">**Jupyter Web UI**</span>: `http://192.168.2.99:9090` (Password: `xilinx`)

### <span style="color: #81A1C1;">**4.2 Passwordless SSH Key Installation**</span>

To allow AI coding assistants and automation scripts to deploy overlays and run tests without interactive password prompts, install your host public key:

1. Generate host key if not already present:
   ```bash
   [ -f ~/.ssh/id_ed25519.pub ] || ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519
   ```

2. Copy the key to the PYNQ board:
   ```bash
   ssh-copy-id -o StrictHostKeyChecking=no xilinx@192.168.2.99
   ```
   *(Enter password `xilinx` when prompted).*

3. Verify passwordless login:
   ```bash
   ssh -o BatchMode=yes xilinx@192.168.2.99 "uname -a"
   ```

### <span style="color: #81A1C1;">**4.3 Passwordless Sudo Configuration (Crucial for DMA/Xlnk)**</span>

In PYNQ Linux v2.6.2 and v3.0, loading overlays via `/dev/xdevcfg` and allocating contiguous DMA memory buffers via the Xlnk / CMA driver (`/dev/xlnk`) requires **root (`sudo`) privileges**.

To prevent non-interactive SSH commands from failing with `sudo: no tty present and no askpass program specified`, configure passwordless sudo on the board:

Execute on the host:
```bash
ssh -tt xilinx@192.168.2.99 'echo "xilinx ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/xilinx && sudo chmod 0440 /etc/sudoers.d/xilinx'
```
*(Enter password `xilinx` once).*

Verify:
```bash
ssh -o BatchMode=yes xilinx@192.168.2.99 "sudo whoami"
```
*(Must output `root` without prompting for a password).*

---

## <span style="color: #88C0D0;">**5. Session Health-Check Procedure (Run in Fresh Sessions)**</span>

Whenever starting a new chat or pairing session, execute this procedure to certify that the PYNQ environment is operational.

### <span style="color: #81A1C1;">**5.1 Automated 6-Point Readiness Script**</span>

Run this single-line diagnostic script from the host terminal:

```bash
python3 -c '
import subprocess, sys

def check(step, cmd):
    print(f"[*] Checking {step}...", end=" ", flush=True)
    res = subprocess.run(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
    if res.returncode == 0:
        print("[PASS]")
        return True, res.stdout.strip()
    else:
        print("[FAIL]")
        return False, res.stderr.strip() or res.stdout.strip()

all_ok = True
ok, out = check("1. Ping Connectivity (192.168.2.99)", "ping -c 2 -W 1 192.168.2.99")
all_ok = all_ok and ok

if ok:
    ok, out = check("2. Passwordless SSH Access", "ssh -o BatchMode=yes -o ConnectTimeout=3 xilinx@192.168.2.99 uname -r")
    print(f"    Kernel: {out}")
    all_ok = all_ok and ok

    ok, out = check("3. Passwordless Sudo Privileges", "ssh -o BatchMode=yes xilinx@192.168.2.99 sudo whoami")
    print(f"    Sudo User: {out}")
    all_ok = all_ok and ok

    ok, out = check("4. PYNQ Python Package", "ssh -o BatchMode=yes xilinx@192.168.2.99 \"python3 -c \\\"import pynq; print(pynq.__version__)\\\"\"")
    print(f"    PYNQ Version: {out}")
    all_ok = all_ok and ok

    ok, out = check("5. NumPy DSP Library", "ssh -o BatchMode=yes xilinx@192.168.2.99 \"python3 -c \\\"import numpy; print(numpy.__version__)\\\"\"")
    print(f"    NumPy Version: {out}")
    all_ok = all_ok and ok

    ok, out = check("6. Working Directory (/home/xilinx/audio_pipeline)", "ssh -o BatchMode=yes xilinx@192.168.2.99 \"mkdir -p /home/xilinx/audio_pipeline && ls -la /home/xilinx/audio_pipeline\"")
    all_ok = all_ok and ok

print("\n" + ("="*50))
if all_ok:
    print("[+] PYNQ BOARD IS 100% READY FOR HARDWARE OVERLAY STREAMING!")
else:
    print("[-] PYNQ BOARD IS NOT READY. Review failed checks above.")
print("="*50)
sys.exit(0 if all_ok else 1)
'
```

### <span style="color: #81A1C1;">**5.2 Step-by-Step Manual Diagnosis**</span>

| Step | Check Name | Command | Expected Output | Remediation if Failed |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Physical Ethernet** | `nmcli device status \| grep ethernet` | `connected (PYNQ-Direct)` | Run `sudo nmcli connection up PYNQ-Direct` |
| **2** | **ICMP Ping** | `ping -c 2 192.168.2.99` | `0% packet loss, time < 1ms` | Wait for green `DONE` LED on PYNQ board |
| **3** | **SSH Session** | `ssh -o BatchMode=yes xilinx@192.168.2.99` | Connects without password | Re-run `ssh-copy-id xilinx@192.168.2.99` |
| **4** | **Root Sudo** | `ssh xilinx@192.168.2.99 "sudo whoami"` | `root` | Follow Section 4.3 to grant NOPASSWD |
| **5** | **PYNQ Overlay** | `ssh xilinx@192.168.2.99 "sudo python3 -c 'import pynq'"` | Return code `0` | Reinstall or inspect `/dev/xlnk` permissions |

---

## <span style="color: #88C0D0;">**6. Troubleshooting Matrix**</span>

| Symptom | Probable Cause | Corrective Action |
| :--- | :--- | :--- |
| `ping: connect: Network is unreachable` | Host Ethernet interface has no IP assigned | Execute `sudo nmcli connection up PYNQ-Direct` or `sudo ip addr add 192.168.2.1/24 dev <eth_if>` |
| `Host interface has 169.254.x.x IP` | Interface fell back to link-local DHCP failure | PYNQ uses static `192.168.2.99`. Host MUST have static IP `192.168.2.1` on subnet `/24`. |
| `Permission denied (publickey,password)` | SSH key not transferred or permissions wrong | Run `ssh-copy-id xilinx@192.168.2.99` (password: `xilinx`). Ensure `~/.ssh` is `700` and `authorized_keys` is `600`. |
| `RuntimeError: Root permission needed` | Python script ran as non-root user `xilinx` | Run all PYNQ scripts with `sudo python3 <script.py>` |
| `sudo: no tty present and no askpass` | User `xilinx` prompts for password on `sudo` | Add `xilinx ALL=(ALL) NOPASSWD: ALL` to `/etc/sudoers.d/xilinx` (Section 4.3). |
| `WARNING: connection is not using post-quantum` | Standard OpenSSH notice on newer Linux kernels | Benign informational warning; does not affect functionality or latency. |
