# Network Design

## 1. Overview

The homelab uses an isolated virtual network behind an OPNsense firewall.

The network design separates the lab environment from the home network and provides controlled internet connectivity for lab systems.

VLAN-based segmentation is planned for a later phase.

## 2. Physical and Virtual Network Architecture

The Proxmox host has one physical network interface and uses two Linux bridges.

| Bridge | Role | Connection |
|---|---|---|
| vmbr0 | WAN | Physical network / home router |
| vmbr1 | Lab LAN | Isolated virtual network |

OPNsense is connected to both bridges:

- WAN interface connected to `vmbr0`.
- LAN interface connected to `vmbr1`.

OPNsense acts as the central gateway and firewall for the lab network.

## 3. Current IP Addressing

| Component | Interface | IP Address |
|---|---|---|
| OPNsense | LAN | 10.10.10.1/24 |
| Lab Network | LAN subnet | 10.10.10.0/24 |
| Lab Clients | LAN | DHCP assigned |

The WAN interface receives connectivity through the home network.

## 4. Network Services

OPNsense provides the following services to the lab network:

- DHCP for dynamic IP address assignment.
- DNS for name resolution.
- NAT for outbound internet connectivity.
- Firewall rules to control network traffic.

## 5. Network Isolation

The lab network is separated from the home network using OPNsense firewall rules.

The current configuration is intended to:

- Allow controlled outbound internet connectivity.
- Restrict access from lab systems to private network ranges.
- Prevent lab systems from accessing the Proxmox management interface.
- Keep lab workloads isolated from the home environment.

Network isolation is enforced through firewall policy rather than VLAN segmentation.

## 6. Remote Management

Tailscale is configured for secure remote access to the Proxmox host.

This management path is separate from the lab network's normal internet connectivity.

## 7. Planned Network Segmentation

VLAN-based segmentation will be introduced in a future phase.

| VLAN | Purpose | Planned Subnet |
|---|---|---|
| 10 | Management | 10.10.10.0/24 |
| 20 | Servers | 10.10.20.0/24 |
| 30 | Workstations | 10.10.30.0/24 |
| 40 | Security Monitoring | 10.10.40.0/24 |
| 50 | DMZ | 10.10.50.0/24 |
| 60 | VPN Clients | 10.10.60.0/24 |

The future design will use firewall policies to control communication between VLANs.

## 8. Future Considerations

- Implement VLAN tagging and trunking.
- Define inter-VLAN firewall policies.
- Separate management, server, workstation and monitoring traffic.
- Deploy WireGuard remote access.
- Configure an IPsec site-to-site VPN.
- Evaluate network traffic visibility for Suricata and Zeek.
