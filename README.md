# Proxmox Homelab Infrastructure

## Overview

This project documents the design, deployment and security configuration of my personal cybersecurity homelab, built on Proxmox VE and OPNsense.

The environment provides an isolated virtual infrastructure for enterprise security testing, defensive security operations, penetration testing and detection engineering.

## Objectives

- Build and maintain an isolated virtual environment.
- Implement firewall controls and network isolation.
- Deploy enterprise infrastructure using Active Directory and Windows clients.
- Implement centralized security monitoring and network detection.
- Perform controlled offensive security testing.
- Develop practical detection engineering and incident response capabilities.

## Current Environment

| Component | Technology |
|---|---|
| Hypervisor | Proxmox VE 9.2 |
| Firewall | OPNsense 26.7 |
| Management VM | Xubuntu |
| Networking | Proxmox Linux Bridges (vmbr0 / vmbr1) |
| Lab Network | 10.10.10.0/24 |
| Lab Gateway | 10.10.10.1 |

## Network Architecture

The current infrastructure uses an isolated internal lab network behind OPNsense.

Future development includes VLAN-based segmentation to separate management, servers, workstations, security monitoring and DMZ workloads.

## Planned Security Infrastructure

- Windows Server 2025 - Active Directory and Group Policy
- Windows 11 - Domain-joined workstations
- Ubuntu Server - Nginx webserver
- Windows Server - Active Directory Certificate Services (AD CS)
- Wazuh - Security monitoring
- Suricata - Network intrusion detection
- Zeek - Network traffic analysis
- Kali Linux - Offensive security testing

## Documentation

- [Infrastructure Architecture](docs/architecture.md)
- [Network Design](docs/network-design.md)
- [Firewall Configuration](docs/firewall-configuration.md)

## Project Scope

This repository documents the implementation process, technical configurations, security controls and validation results as the homelab evolves.
