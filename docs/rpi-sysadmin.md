---
layout: default
title: "Raspberry Pi RAID SysAdmin"
permalink: /docs/rpi-sysadmin
---

[← Back to Home](../index.md)

# 🍓 Raspberry Pi RAID SysAdmin — Remote Debugging with Antigravity

> **Prompt:** *"SSH to my RPi and check my RAID because it keeps sending me email alerts."*

Antigravity SSHed autonomously into the Raspberry Pi (`mathis@192.168.1.30`), diagnosed the root cause of a persistent RAID failure alert, and applied a kernel-level fix — all without leaving the chat.

---

## 🔍 Diagnosis

### Step 1 — Check RAID status

```bash
ssh rpi "cat /proc/mdstat"
```

Output looked healthy at first glance:
```
md0 : active raid1 sdb1[0] sdc1[1]
      1953382400 blocks [2/2] [UU]
```

`[UU]` = both drives up. But the alerts kept coming. Antigravity dug deeper.

### Step 2 — Check kernel logs

```bash
ssh rpi "dmesg | grep -i 'error\|uas\|usb\|md0' | tail -40"
```

**Root cause found:**

```
usb 4-1: cmd cmplt err -71
md/raid1:md0: Disk failure on sdc1, disabling device.
md/raid1:md0: Operation continuing on 1 devices.
```

The drives (`sdb`, `sdc`) were USB-connected JMicron JMS578 adapters (`152d:0578`) running on the **UAS (USB Attached Storage)** driver — a well-known problematic combination on Raspberry Pi. The UAS driver was throwing command errors that the RAID driver interpreted as disk failures.

---

## 🔧 Fix Applied

### Step 1 — Identify USB adapter IDs

```bash
ssh rpi "lsusb | grep -i storage"
# Bus 004 Device 002: ID 152d:0578 JMicron Technology Corp. JMS578 SATA 6Gb/s bridge
# Bus 002 Device 002: ID 152d:0578 JMicron Technology Corp. JMS578 SATA 6Gb/s bridge
```

Both adapters are the same chipset: `152d:0578`.

### Step 2 — Check existing quirk

```bash
ssh rpi "cat /boot/cmdline.txt"
```

The quirk `usb-storage.quirks=152d:0578:u` was **already present** — but the drives were still bound to UAS at runtime. The parameter was present but not taking effect.

### Step 3 — Create a persistent udev rule

```bash
ssh rpi "sudo tee /etc/udev/rules.d/99-usb-storage-quirks.rules" << 'EOF'
# Disable UAS for JMicron JMS578 — force usb-storage driver
ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="152d", ATTR{idProduct}=="0578", \
    RUN+="/sbin/modprobe -r uas", RUN+="/sbin/modprobe usb-storage"
EOF
```

### Step 4 — Reboot and verify

```bash
ssh rpi "sudo reboot"
# ... (wait for RPi to come back up)
ssh rpi "lsmod | grep uas && echo UAS still loaded || echo UAS not loaded"
ssh rpi "cat /proc/mdstat"
```

After reboot:
```
md0 : active raid1 sdb1[0] sdc1[1]
      1953382400 blocks [2/2] [UU]
```

No more UAS errors in `dmesg`. RAID array stable. Alert emails stopped.

---

## 🧭 How the Session Was Guided

This wasn't a single "fix my RAID" command that magically resolved everything. The session was an **iterative collaboration** where I steered Antigravity through a series of decisions and dead ends:

- **Started with the obvious:** I asked Antigravity to check the RAID status — it looked healthy at first glance (`[UU]`), which would have been the end of the investigation for a simpler tool.
- **Pushed deeper:** I asked it to dig into kernel logs, which revealed the real culprit — USB UAS errors — something that wouldn't have been obvious without understanding the stack.
- **Tried the expected fix first:** The `usb-storage.quirks` parameter was already in `/boot/cmdline.txt`. Rather than stopping there, I asked Antigravity to check *why* it wasn't working — and it found the drives were still bound to the UAS driver at runtime.
- **Navigated a risky moment:** Attempting to rebind the USB drivers live (without a reboot) caused one drive to drop out of the array temporarily — putting the RAID in a degraded `[_U]` state. I chose to proceed with a clean reboot rather than try to recover the live binding.
- **Verified the outcome:** After reboot, I had Antigravity confirm the array was healthy, UAS was no longer loaded, and no new errors appeared in `dmesg`.

The key insight is that **I provided the direction and judgment** (what to investigate next, whether a risk was acceptable, when to reboot) while **Antigravity handled all the execution** (SSH commands, log parsing, writing config files, udev rules).

---

## 💡 What This Showed About Antigravity

| Capability | How It Was Used |
|---|---|
| **SSH into live system** | Directly ran commands on the RPi over SSH |
| **Log analysis** | Parsed `dmesg`, `mdstat`, `lsusb` output autonomously |
| **Root cause diagnosis** | Identified the UAS/JMicron combination as root cause |
| **Kernel-level fix** | Wrote a `udev` rule and updated `cmdline.txt` |
| **Autonomous recovery** | Triggered reboot and verified array health after |

---

[← Back to ChipGPT](./chipgpt.md) | [Next: About Antigravity →](./about-antigravity.md)
