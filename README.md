# SOC Homelab — Active Directory Detection Engineering

A self-hosted lab for practicing detection engineering: attacking a small
Active Directory environment, collecting the resulting telemetry in a SIEM,
and writing detections mapped to [MITRE ATT&CK](https://attack.mitre.org/).

Built as a portfolio project while finishing my cybersecurity studies —
the goal is to document the full loop (attack → log → detect → tune), not
just list VMs.

## Status

🚧 Early build-out — DC01 (domain controller) is up. See the detection
table below for current coverage.

## Architecture

| VM | OS | RAM | Role |
|----|----|----|------|
| DC01 | Windows Server 2022 | 4 GB | AD DS + DNS, domain `lab.internal` |
| WS01 | Windows 11 | 4 GB | Domain-joined client, Sysmon + Wazuh agent |
| SIEM01 | Ubuntu Server 24.04 | 8 GB | Wazuh (all-in-one) |
| ATK01 | Kali Linux | 4 GB | Attacker box |

Hypervisor: VirtualBox. Full network diagram and IP plan in
[docs/00-architecture.md](docs/00-architecture.md).

## Detection coverage

The table below is the actual deliverable of this project — each row goes
from `planned` to `validated` as an attack is simulated, logged, and
detected.

| # | ATT&CK Technique | Attack Simulated | Detection | Status |
|---|-------------------|-------------------|-----------|--------|
| 1 | T1110 – Brute Force | — | — | planned |
| 2 | T1078 – Valid Accounts | — | — | planned |
| 3 | T1003 – OS Credential Dumping | — | — | planned |
| 4 | T1021 – Remote Services (RDP/WinRM) | — | — | planned |
| 5 | T1547 – Boot or Logon Autostart Execution | — | — | planned |

## Build log

- [00-architecture.md](docs/00-architecture.md) — network design, IP plan, VM specs
- [01-dc01-setup.md](docs/01-dc01-setup.md) — domain controller build (AD DS, DNS, audit policy)

## Roadmap

1. **Phase 1 — Foundation**: hypervisor + networking, DC01 (AD DS/DNS) ✅
2. **Phase 2 — Client**: WS01 domain join, Sysmon
3. **Phase 3 — SIEM**: Wazuh all-in-one, agent enrollment
4. **Phase 4 — Attack + Detect**: ATK01, attack simulations mapped to ATT&CK, detection rules, false-positive/limitation notes
5. **Phase 5 — Network monitoring**: pfSense + Suricata (stretch goal)

## Why this repo exists

Anyone can spin up VMs. What's harder — and what this repo tries to show —
is being able to say: *I attacked X, this is what showed up in the logs,
this is the detection I wrote for it, and here's when it produces false
positives.* That's the actual day-to-day of a SOC L1/L2 analyst.
