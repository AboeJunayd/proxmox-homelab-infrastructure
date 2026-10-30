# Firewall Configuration

## 1. Overview

OPNsense acts as the central firewall and router for the Proxmox homelab.

It controls traffic between the WAN and the isolated lab network and enforces network access policies.

## 2. Firewall Objectives

The firewall configuration aims to:

- Provide controlled outbound internet access for lab systems.
- Restrict access from the lab network to private network ranges.
- Protect the Proxmox management interface.
- Allow required network services such as DNS.
- Provide a foundation for future network segmentation.

## 3. Network Interfaces

| Interface | Connected Network | Purpose |
|---|---|---|
| WAN | vmbr0 | Connectivity through the home network |
| LAN | vmbr1 | Isolated lab network |

## 4. LAN Firewall Rules

The following rules represent the documented LAN firewall policy.

| Rule | Source | Destination | Action |
|---|---|---|---|
| Allow OPNsense GUI Management | LAN net | 10.10.10.1:HTTPS | Pass |
| Allow Lab DNS | LAN net | 10.10.10.1:DNS | Pass |
| Block Lab to Private Networks | LAN net | PRIVATE_NETWORKS | Block |
| Default allow LAN to any | LAN net | Any | Pass |

## 5. Rule Processing

OPNsense evaluates firewall rules in order.

The private network block rule must be evaluated before the general LAN allow rule.

This prevents matching traffic from the lab network to the private network ranges defined in the `PRIVATE_NETWORKS` alias from reaching the default allow rule.

The default LAN allow rule permits other traffic that is not blocked by an earlier rule.

## 6. Management Interface Protection

Access to the Proxmox management interface (TCP port 8006) is restricted from the lab network.

Remote administration is performed through the configured Tailscale management connection.

## 7. Validation

The following checks have been performed during network configuration:

- Lab internet connectivity.
- DNS resolution from the lab network.
- ICMP connectivity testing.
- Verification that TCP access to the Proxmox GUI (port 8006) is blocked from the lab network.

## 8. Security Considerations

The current configuration provides network isolation through firewall rules, but it is not a default-deny policy.

Further hardening opportunities include:

- Replacing the general LAN allow rule with explicitly permitted services.
- Reviewing the contents of the `PRIVATE_NETWORKS` alias.
- Reviewing IPv6 firewall policy.
- Implementing dedicated VLAN firewall policies.
- Logging and monitoring relevant blocked traffic.
