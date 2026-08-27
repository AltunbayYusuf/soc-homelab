# WS01 — Domain-Joined Client Setup

## Status: domain join complete

## Specs

- OS: Windows 11 Enterprise Evaluation (build 26100.6584)
- Disk: 80 GB, dynamically allocated
- RAM: 4096 MB
- vCPU: 2
- Network adapters: `lab-net` (internal, domain traffic) + NAT (internet access)
- Static IP: `10.10.10.20`
- DNS: `10.10.10.10` (DC01)
- Domain: joined to `lab.internal`

## Steps

1. Created VM in VirtualBox with the specs above, manual (non-unattended) install from ISO
2. Installed Windows 11 Enterprise Evaluation
3. Set Belgian keyboard layout
4. Set static IP `10.10.10.20` and DNS `10.10.10.10` on the `lab-net` adapter
5. Verified connectivity to DC01: `ping 10.10.10.10` succeeded, `nslookup lab.internal 10.10.10.10` resolved correctly
6. Joined the `lab.internal` domain via `sysdm.cpl` → Change → Domain
7. Rebooted, logged in with `LAB\administrator`
8. Verified on DC01 (Active Directory Users and Computers → Computers) that the `WS01` computer object was created

## Notes

- DNS lookup for `lab.internal` also returned `10.0.2.15` (VirtualBox's default NAT
  address) alongside `10.10.10.10` — DC01 registered its NAT adapter in DNS too.
  Not currently causing issues, but worth cleaning up later (e.g. disabling
  dynamic DNS registration on the NAT adapter) if it gets in the way.

## Next

- Install Sysmon for process/logon telemetry
- Install VirtualBox Guest Additions (done, for clipboard/usability — not
  part of the detection pipeline)
- Enroll as a Wazuh agent once SIEM01 exists
