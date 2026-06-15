# Proxmox Attack/Defense Lab — Segmented Network Pentesting & Detection Environment

A hands-on cybersecurity lab built on Proxmox VE, simulating a segmented enterprise network (DMZ + Internal) with a dedicated attacker host, vulnerable targets, and a SIEM for detection. The project covers the full offense → defense → detection cycle: exploit, observe, patch, and re-test.

---

## 📌 Project Overview

| | |
|---|---|
| **Platform** | Proxmox VE (type-1 hypervisor) |
| **Duration** | 7-15 days |
| **Focus areas** | Network segmentation, web app exploitation, lateral movement, SIEM/IDS detection, hardening |
| **Skill level** | Beginner → Intermediate cybersecurity |

---

## 🎯 Goals

1. Build a realistic, segmented virtual network (WAN / DMZ / Internal) using a virtual firewall.
2. Deploy intentionally vulnerable applications and a weakly configured internal host.
3. Perform reconnaissance, exploitation, and lateral movement from a dedicated attacker VM.
4. Stand up a SIEM (Wazuh/ELK) to detect the attacks performed in real time.
5. Harden the environment based on findings, then re-run attacks to validate fixes.
6. Document the entire attack chain — vulnerabilities, exploits, detections, and remediations — as a portfolio-ready writeup.

---

## 🏗️ Architecture

```
                    ┌──────────────────┐
                    │   kali-attacker   │
                    │  (External/WAN)   │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │     OPNsense-fw     │
                    │  (Firewall/Router) │
                    └──┬─────────────┬──┘
                       │             │
              ┌────────▼───┐   ┌─────▼────────┐
              │ dmz-target  │   │ internal-host │
              │ (Juice Shop │   │ (Samba/SSH,   │
              │  + DVWA)    │   │  weak config) │
              └─────┬───────┘   └──────┬────────┘
                    │                   │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │    siem-server     │
                    │   (Wazuh / ELK)    │
                    └────────────────────┘
```

### VM Inventory

| VM Name | OS | Role | Network Segment |
|---|---|---|---|
| `pfsense-fw` | pfSense | Firewall/router, traffic segmentation | WAN ↔ DMZ ↔ Internal |
| `dmz-target` | Ubuntu Server | Hosts vulnerable web apps (Docker) | DMZ |
| `internal-host` | Ubuntu/Windows | Simulated internal asset (lateral movement target) | Internal |
| `kali-attacker` | Kali Linux | Offensive testing platform | External |
| `siem-server` | Ubuntu Server | Log aggregation, detection, alerting | Internal |

### Proxmox Virtual Networks

- `vmbr0` — WAN/External (Kali ↔ OPNSense)
- `vmbr1` — DMZ (OPNSense ↔ dmz-target)
- `vmbr2` — Internal (OPNSense ↔ internal-host, siem-server)

---

## 🛠️ Tech Stack

- **Hypervisor:** Proxmox VE
- **Firewall/Router:** OPNSense
- **Vulnerable apps:** OWASP Juice Shop, DVWA (Docker Compose)
- **Attacker tools:** Kali Linux (nmap, sqlmap, Burp Suite/ZAP, Metasploit)
- **SIEM/Detection:** Wazuh (or ELK stack), Suricata IDS
- **Hardening:** fail2ban, OPNSense ACLs, input validation/patching


---


## 🚀 Possible Extensions

- Add Active Directory to `internal-host` for AD attack paths (Kerberoasting, BloodHound).
- Automate VM provisioning with Terraform + Proxmox provider and Ansible for configuration.
- Add a second attacker scenario (phishing simulation, malware sandbox).

---

## ⚠️ Disclaimer

This lab is for educational purposes in an isolated, self-hosted environment only. All vulnerable applications and weak configurations are intentional and contained within virtual networks with no exposure to production systems or the public internet.
