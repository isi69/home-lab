# Security Lab

> Offensive security through hands-on practice built on a foundation from *Technisches Gymnasium Informationstechnik* (digital electronics, microcontroller programming in C/C++, OOP, databases, networking,  OSI, TCP/IP, VLANs, subnetting, routing, firewalls, IoT).

All testing performed on personally owned and authorized equipment in isolated lab environments.

---

## Technical Background

```
Hardware       →  Digital electronics, microcontroller programming (C/C++)
Software       →  OOP, databases, embedded systems
Networking     →  OSI model, TCP/IP, VLANs, subnetting, routing, firewalls, IoT
Current focus  →  Offensive security, penetration testing, network analysis
```

---

## Labs

| # | Lab | Category | Status |
|---|---|---|---|
| 01 | [Home Network Assessment](labs/01-home-network-assessment.md) | Recon · MITM · Traffic Analysis | ✅ Completed |
| 02 | [Metasploitable 2 Setup](labs/02-metasploitable-setup.md) | Lab Infrastructure | ✅ Completed |
| 03 | [Metasploitable 2 Exploitation](labs/03-metasploitable-exploitation.md) | Exploitation · Post-Exploitation · SQLi | 🔄 In Progress |
| 04 | [Network Migration: DSL → Cable](labs/04-network-migration.md) | Network Diagnostics · Bufferbloat | ✅ Completed |

### What each lab documents

Every lab entry includes: objective, environment, methodology, tool output / terminal logs, findings, key learnings. No screenshots of success, raw command output only.

---

## Courses & Certifications

| Course | Provider | Modules Covered | Status |
|---|---|---|---|
| [Junior Ethical Hacker](certifications/CISCO_ETHICAL_HACKING.md) | Cisco Skills for All | Recon · GRC · Pentesting Methodology | 🔄 In Progress |

---

## HTB Academy

| Module | Status |
|---|---|
| [Learning Process](htb-academy/01-learning-process.md) | ✅ Completed |

---

## Tools

| Tool | Used for |
|---|---|
| Nmap | Host discovery, port scanning, OS/service fingerprinting |
| Wireshark | Packet capture, protocol analysis, traffic inspection |
| Ettercap | ARP spoofing, MITM execution |
| Metasploit | Exploitation framework, post-exploitation, payload delivery |
| John the Ripper | Password hash cracking |
| arp-scan | Layer 2 host discovery with MAC vendor resolution |
| mtr / ping | Latency diagnostics, Bufferbloat measurement |
| Scapy | Manual packet crafting and injection |
| dig / nslookup / whois | DNS enumeration and passive recon |

**Environment:** Kali Linux, VirtualBox (Internal Network / Bridged Adapter)

---

## Roadmap

**Completed**
- [x] Home network reconnaissance and device fingerprinting
- [x] Passive packet capture and protocol analysis (Wireshark)
- [x] ARP spoofing / MITM on personal device
- [x] Isolated lab setup (Metasploitable 2 + Kali on Internal Network)
- [x] Service exploitation with Metasploit (vsftpd 2.3.4 backdoor)
- [x] Password hash dumping and cracking (John the Ripper + rockyou.txt)
- [x] SQL injection basics and column enumeration (DVWA)
- [x] Network diagnostics, Bufferbloat measurement and root cause analysis
- [x] Network migration post-verification (DSL to Cable baseline comparison)

**In progress**
- [ ] SQL injection, UNION SELECT, hash extraction, Hashcat
- [ ] Cisco Ethical Hacking Course

**Planned**
- [ ] WiFi pentesting, Alfa AWUS036ACM, WPA2 handshake capture and cracking
- [ ] HackTheBox machines
- [ ] CompTIA Security+

---

*2026*
