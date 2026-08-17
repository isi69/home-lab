# Lab 04 - Network Migration: DSL → Cable

**Date:** June 2026
**Status:** ✅ Completed
**Environment:** Kali Linux · Home Network · VDSL 50 → Vodafone Cable

---

## Goal

Diagnose and document a severe Bufferbloat / high-jitter issue on a VDSL 50 (FTTC) connection, identify the root cause, migrate to cable internet, and verify the fix with reproducible before/after measurements.

---

## Phase 1 - Problem Identification

### Setup

| Detail | Value |
|---|---|
| Connection | VDSL 50 (FTTC / Vectoring) |
| Physical last mile | Fiber to cabinet → copper to apartment |
| Building type | Multi-tenant residential |
| OS | Kali Linux |

### Symptoms

Severe latency instability under load, making the connection unusable for anything latency-sensitive (VoIP, SSH, gaming, VPN).

### Measurement Tools

```bash
# Baseline latency
ping -c 100 8.8.8.8

# Path analysis with per-hop latency
mtr --report --report-cycles 60 8.8.8.8

# Latency under load (two terminals simultaneously)
# Terminal 1 - monitor latency:
ping -i 0.2 8.8.8.8

# Terminal 2 - saturate the line:
curl -o /dev/null https://speed.hetzner.de/1GB.bin
```

### DSL Baseline Results

```
speedtest-cli: 49 Mbit/s ↓ / 29 Mbit/s ↑

mtr --report (60 cycles to 8.8.8.8):
  Best:    23 ms
  Average: 101 ms
  Worst:   951 ms
  Loss:    0.0%
```

```bash
# ISP core network probes - same instability confirmed upstream
ping 212.18.0.5    # Vodafone core    → extreme variance
ping 217.237.150.205  # Telekom core  → extreme variance
```

**Key observation:** 0% packet loss but 951ms worst-case latency = classic Bufferbloat. The router's buffer fills completely under load, queueing packets for hundreds of milliseconds instead of dropping them.

---

## Phase 2 - Root Cause Analysis

### Why DSL Provider Switch Wouldn't Help

The instability appears at the **first hop** (ISP DSLAM), before traffic even reaches the broader internet. The DSLAM serves multiple apartments on shared copper infrastructure — Crosstalk (electromagnetic interference between parallel copper pairs) and DSLAM oversubscription cause the degradation. Switching DSL providers would still use the same physical copper and likely the same DSLAM.

### Technology Comparison

| Factor | VDSL (FTTC) | Cable (DOCSIS 3.1) |
|---|---|---|
| Last mile medium | Copper pair | Coaxial cable |
| Crosstalk risk | High (parallel pairs) | None |
| Bufferbloat risk | High (ISP-dependent) | Lower |
| Shared medium | Per-apartment pair | Shared node (neighborhood) |
| Typical jitter | Variable | Generally lower |

**Conclusion:** Physical migration to cable is the correct fix. The coaxial cable infrastructure is separate from the DSL copper plant — different medium, different DSLAM equivalents (CMTS), different congestion characteristics.

---

## Phase 3 - Migration

### Pre-Migration Checklist

- [ ] Confirm cable socket is active (contact building management — ISP needs basement access)
- [ ] Check Vodafone cable availability at address: `https://www.vodafone.de/hilfe/netzausbau.html`
- [ ] Schedule technician appointment (included with new contract)
- [ ] Note DSL contract end date / notice period to avoid paying both

### Infrastructure

In Germany, cable internet in multi-tenant buildings runs through a shared coaxial infrastructure managed either by Vodafone directly or via a building contract (Hausnetz). The building manager must confirm:

1. Whether the in-apartment TV socket is connected to an active cable node
2. Whether the basement distribution point (Hausverteiler) supports internet (Data over Cable)

### ISP Options (Pforzheim)

| Provider | Technology | Notes |
|---|---|---|
| Vodafone Kabel | DOCSIS 3.1 | Primary cable provider in region |
| NetCologne / local | Varies | Check local availability |

---

## Phase 4 - Post-Migration Verification

### Reproduce Baseline Tests

Run the exact same tests after migration to produce a direct comparison:

```bash
# Save results to file for comparison
mtr --report --report-cycles 60 8.8.8.8 > mtr_cable_baseline.txt

# Latency under load
ping -i 0.2 -c 150 8.8.8.8 > ping_cable_load.txt &
curl -o /dev/null https://speed.hetzner.de/1GB.bin
```

### Expected Results (Target)

| Metric | DSL (Before) | Cable (Target) |
|---|---|---|
| Download | ~49 Mbit/s | >100 Mbit/s |
| Latency idle | ~23 ms | <15 ms |
| Latency under load | ~951 ms | <50 ms |
| Packet loss | 0.0% | 0.0% |

### Kali Network Reconfiguration

After router swap, update any hardcoded references:

```bash
# Verify new default gateway (Vodafone routers typically use 192.168.0.1)
ip route show
ping $(ip route | grep default | awk '{print $3}')

# Update Lab VM routing if gateway IP changed
# On Kali (eth1 = internal lab network — unchanged):
ip addr show eth1
ip route show

# DNS check
resolvectl status
cat /etc/resolv.conf
```

If using NetworkManager profiles with static DNS or gateway entries, update via:

```bash
nmcli connection show
nmcli connection edit <profile-name>
```

---

## Results

> *To be completed after migration*

| Metric | DSL Baseline | Cable Result | Delta |
|---|---|---|---|
| Download | 49 Mbit/s | — | — |
| Upload | 29 Mbit/s | — | — |
| Latency (idle) | 23 ms | — | — |
| Latency (under load) | 951 ms | — | — |
| Packet loss | 0.0% | — | — |

---

## Key Learnings

- Bufferbloat (0% loss + extreme latency under load) is distinct from packet loss — the buffer fills instead of dropping
- Crosstalk on FTTC copper affects all providers using the same physical infrastructure
- `mtr --report` with 60 cycles gives a more reliable picture than short ping tests
- Worst-case latency matters more than average for interactive use (SSH, VoIP, gaming)
- Physical medium migration (copper → coax) is sometimes the only real fix

---

## Skills Practiced

- [x] Latency diagnosis with `ping`, `mtr`
- [x] Bufferbloat identification under synthetic load
- [x] ISP infrastructure analysis (FTTC vs DOCSIS)
- [x] Before/after benchmarking methodology
- [ ] Post-migration verification (pending)
- [ ] NetworkManager profile reconfiguration (pending)

---

## Tools Used

| Tool | Purpose |
|---|---|
| mtr | Path analysis and per-hop latency |
| ping | Baseline and under-load latency |
| speedtest-cli | Throughput measurement |
| curl | Synthetic load generation |
| ip / nmcli | Network interface and routing config |

---

*[← Back to README](../README.md)*
