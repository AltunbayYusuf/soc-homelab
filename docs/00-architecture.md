# Architecture

## Status: not started

## Goal

A small, isolated AD environment that generates realistic Windows security
telemetry, monitored by a SIEM, attacked from a dedicated Kali box.

## VMs

| VM | OS | vCPU | RAM | Disk | Network |
|----|----|------|-----|------|---------|
| DC01 | Windows Server 2022 | 2 | 4096 MB | dynamic | lab-net (internal) |
| WS01 | Windows 11 | 2 | 4096 MB | dynamic | lab-net (internal) |
| SIEM01 | Ubuntu Server 24.04 | 2 | 8192 MB | dynamic | lab-net (internal) |
| ATK01 | Kali Linux | 2 | 4096 MB | dynamic | lab-net (internal) |

All VMs also get a NAT adapter for internet access (Windows updates, apt/
package installs), separate from the internal `lab-net` used for
domain/attack traffic. Isolating the internal network keeps attack traffic
off the host's real LAN.

## IP plan (lab-net)

| Host | IP | Notes |
|------|----|----|
| DC01 | 10.10.10.10 | static, also the DNS server for the domain |
| WS01 | 10.10.10.20 | static, domain-joined to lab.internal |
| SIEM01 | TBD | static |
| ATK01 | TBD | static |

Domain: `lab.internal`

## Network diagram

TBD — add once VirtualBox networking (internal network + NAT) is confirmed
working end-to-end.

## Hypervisor notes

- VirtualBox, Hyper-V/WSL2 platform features must be disabled on the host
  or VirtualBox falls back to slow emulation.
- Extension Pack installed for USB passthrough and better guest resolution.
