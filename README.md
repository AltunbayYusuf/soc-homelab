# SOC Homelab — Active Directory Detection Engineering

A self-hosted lab for practicing detection engineering: attacking a small
Active Directory environment, collecting the resulting logs in a SIEM, and
writing detections for what I find.

Built as a portfolio project while finishing my cybersecurity studies.

## Status

🚧 Early build-out — DC01 (domain controller) is up, WS01 is domain-joined.

## Architecture

| VM | OS | RAM | Role |
|----|----|----|------|
| DC01 | Windows Server 2022 | 4 GB | AD DS + DNS, domain `lab.internal` |
| WS01 | Windows 11 | 4 GB | Domain-joined client, Sysmon + Wazuh agent |
| SIEM01 | Ubuntu Server 24.04 | 8 GB | Wazuh (all-in-one) |
| ATK01 | Kali Linux | 4 GB | Attacker box |

Hypervisor: VirtualBox. Full network diagram and IP plan in
[docs/00-architecture.md](docs/00-architecture.md).

## Build log

- [00-architecture.md](docs/00-architecture.md) — network design, IP plan, VM specs
- [01-dc01-setup.md](docs/01-dc01-setup.md) — domain controller build (AD DS, DNS, audit policy)
- [02-ws01-setup.md](docs/02-ws01-setup.md) — Windows 11 client build and domain join

## Roadmap

1. **Phase 1 — Foundation**: hypervisor + networking, DC01 (AD DS/DNS) ✅
2. **Phase 2 — Client**: WS01 domain join, Sysmon
3. **Phase 3 — SIEM**: Wazuh all-in-one, agent enrollment
4. **Phase 4 — Attack + Detect**: ATK01, attack simulations, detection rules
5. **Phase 5 — Network monitoring**: pfSense + Suricata (stretch goal)
