# Infrastructure Architecture

## Overview
The homelab runs on a bare-metal Proxmox VE hypervisor, providing the foundation for isolated cybersecurity lab environments.

## Hardware

| Component | Specification |
|---|---|
| CPU | Intel Core Ultra 7 265K |
| Memory | 64 GB RAM |
| Storage | 1 TB Kingston NVMe SSD |
| Network | 1 physical network interface |

## Software

| Component | Version |
|---|---|
| Hypervisor | Proxmox VE 9.2.24 |
| Kernel | 7.0.14-24-pve |
| Firewall | OPNsense 26.7 |
| Management VM | Ubuntu |

## Virtual Network Architecture

The infrastructure uses two Linux bridges:

- **vmbr0:** WAN connectivity.
- **vmbr1:** Isolated internal lab network.

OPNsense acts as the firewall and router between the WAN and isolated lab network.

## Future Expansion

The infrastructure will be extended with Active Directory, Windows workstations, a web server and Wazuh security monitoring.
