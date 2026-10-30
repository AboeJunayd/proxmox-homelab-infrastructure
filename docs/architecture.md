# Infrastructure Architecture

## 1. Project Overview

This homelab is built on Proxmox VE and uses OPNsense as its central firewall and router.

The infrastructure provides an isolated environment for cybersecurity testing, enterprise infrastructure deployment, security monitoring and incident response.

## 2. Current Infrastructure

### Hardware

| Component | Specification |
|---|---|
| CPU | Intel Core Ultra 7 265K |
| CPU Cores | 20 |
| Memory | 64 GB RAM |
| Storage | 1 TB Kingston NVMe SSD |
| Physical Network Interfaces | 1 |

### Virtualization

| Component | Version / Configuration |
|---|---|
| Hypervisor | Proxmox VE 9.2.24 |
| Operating System | Debian GNU/Linux 13 |
| Kernel | 7.0.14-24-pve |
| Firewall | OPNsense 26.7 |

### Network Infrastructure

| Component | Configuration |
|---|---|
| WAN Bridge | vmbr0 |
| LAN Bridge | vmbr1 |
| Lab Network | 10.10.10.0/24 |
| OPNsense LAN Gateway | 10.10.10.1 |
| Management VM | Xubuntu |
| Remote Management | Tailscale |

### Current Network Design

The Proxmox host uses two Linux bridges:

- `vmbr0`: Connected to the physical network and used for WAN connectivity.
- `vmbr1`: Isolated virtual network for lab systems.

OPNsense connects both bridges and provides firewalling, routing, DHCP, DNS and NAT for the lab network.

The lab network is isolated from the home network through firewall rules.

## 3. Planned Enterprise Infrastructure

### Identity and Access Management

- Windows Server 2025 - Active Directory Domain Services
- DNS and Group Policy
- Two Windows 11 domain-joined workstations
- Separate Windows Server for Active Directory Certificate Services (AD CS)

### Enterprise Services

- Ubuntu Server LTS
- Nginx web server

## 4. Planned Security Monitoring

| Component | Purpose |
|---|---|
| Wazuh | Security monitoring and endpoint detection |
| Suricata | Network intrusion detection and prevention |
| Zeek | Network traffic analysis |

## 5. Planned Security Operations

- SOC workstation for security investigations
- Kali Linux for controlled penetration testing
- Wireshark for network analysis
- YARA for file and malware analysis
- Volatility for memory forensics

## 6. Planned Network Expansion

The current lab uses a single isolated LAN.

Future development includes VLAN-based segmentation:

| VLAN | Purpose | Planned Subnet |
|---|---|---|
| 10 | Management | 10.10.10.0/24 |
| 20 | Servers | 10.10.20.0/24 |
| 30 | Workstations | 10.10.30.0/24 |
| 40 | Security Monitoring | 10.10.40.0/24 |
| 50 | DMZ | 10.10.50.0/24 |
| 60 | VPN Clients | 10.10.60.0/24 |

## 7. Planned VPN Connectivity

- WireGuard for remote VPN access.
- IPsec site-to-site VPN using a second OPNsense instance.

Tailscale will remain available for secure Proxmox management.

## 8. Project Roadmap

1. Infrastructure and network isolation
2. Active Directory deployment
3. Enterprise services and PKI
4. Security monitoring deployment
5. SOC workstation configuration
6. Offensive security testing
7. VLAN segmentation and VPN connectivity
8. Detection engineering and DFIR
