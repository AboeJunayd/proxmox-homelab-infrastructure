# Network Design

## Overview
The homelab uses a segmented network architecture to isolate lab systems from the home network while allowing controlled internet access.

## Network Architecture

Two Proxmox Linux bridges are configured:

| Bridge | Purpose |
|---|---|
| vmbr0 | WAN connectivity via the home router |
| vmbr1 | Isolated internal lab network |

OPNsense connects both networks and acts as the central firewall and router.

## IP Addressing

| Interface | Network | Purpose |
|---|---|---|
| WAN | Home network | External connectivity |
| LAN | 10.10.10.0/24 | Internal lab network |
| OPNsense LAN | 10.10.10.1 | Default gateway |


## Network Services

The following services are configured on OPNsense:

- DHCP – Automatic IP address assignment.
- DNS – DNS resolution for lab systems.
- NAT – Outbound internet connectivity.

## Network Isolation

The internal lab network is separated from the home network.

Firewall rules restrict access between network segments to prevent lab systems from accessing protected infrastructure.

## Validation

The following connectivity tests have been performed:

- Internet connectivity from the lab.
- DNS resolution.
- ICMP connectivity and isolation checks.
- TCP access to the Proxmox management interface (port 8006) blocked from the lab network.
