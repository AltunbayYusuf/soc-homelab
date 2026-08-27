# DC01 — Domain Controller Setup

## Status: complete

## Specs

- OS: Windows Server 2022 (evaluation)
- Edition: Standard (Desktop Experience)
- Disk: 60 GB, dynamically allocated
- RAM: 4096 MB
- vCPU: 2
- Network adapters: `lab-net` (internal, domain traffic) + NAT (internet access for updates)
- Static IP: `10.10.10.10`
- Role: AD DS + DNS
- Domain: `lab.internal` (new forest)

## Steps

1. Created VM in VirtualBox with the specs above, attached Windows Server 2022 ISO
2. Installed Windows Server, Standard edition with Desktop Experience
3. Set static IP `10.10.10.10` on the `lab-net` adapter
4. Renamed the host and rebooted (required as a separate step before promotion — see troubleshooting)
5. Installed the AD DS role
6. Promoted the server to a new forest, domain `lab.internal`
7. Verified the promotion and ran `dcdiag`

## Troubleshooting

**ISO wouldn't mount / empty optical drive.** VirtualBox showed the virtual
optical drive as empty even after attaching the Windows Server ISO in the
storage settings. Re-checking the storage controller settings and
re-attaching the ISO resolved it — worth double-checking this before
booting the VM for the first install attempt.

**Hostname change needs its own reboot before AD DS promotion.** Renaming
the server and promoting it to a domain controller in the same session
doesn't work cleanly — the rename needs to be committed with a reboot
*before* running through the AD DS promotion wizard, otherwise the
promotion can behave unpredictably.

**Misleading "DomainNetbiosName was not recognized" error.** After running
the forest promotion, PowerShell/the wizard threw an error claiming the
`DomainNetbiosName` parameter wasn't recognized. This looked like a failed
promotion, but it was a known quirk of the AD DS deployment module — the
forest had actually already been created successfully. Confirmed with:

```powershell
Get-Service NTDS
Get-ADDomain
```

Both showed the domain controller and `lab.internal` domain were already
up before assuming the promotion had failed and re-running it.

## Verification

`dcdiag` results: all critical AD tests passed (Connectivity, Advertising,
FrsEvent, KccEvent, KnowsOfRoleHolders, MachineAccount, NCSecDesc,
NetLogons, ObjectsReplicated, Replications, RidManager, Services,
SystemLog baseline, VerifyReferences).

`DFSREvent` and `SystemLog` warnings were present — expected on a single,
isolated DC with no replication partner, not treated as a real issue.

## Screenshots

_Shared during the build but not yet added to this repo — add relevant
ones under `docs/screenshots/` if useful for the portfolio._
