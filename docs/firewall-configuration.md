# Firewall Configuration

## Overview
OPNsense is used as the central firewall and router for the homelab. It controls traffic between the home network, the WAN interface and the isolated lab network.

## Firewall Objectives
- Restrict access between network segments.
- Prevent unauthorized access to the Proxmox management interface.
- Allow controlled outbound internet connectivity.
- Restrict access to private network ranges where required.

## Network Protection
Firewall rules are implemented to control traffic between the WAN and LAN interfaces.

Access to the Proxmox management interface (TCP/8006) is restricted from the isolated lab network.

## NAT Configuration
Outbound NAT provides internet connectivity for systems within the isolated lab network.

## Validation
Firewall behavior is validated through connectivity tests, port access checks and firewall logs.

## Future Improvements
- Review and refine firewall rules.
- Document individual rule configurations.
- Expand network monitoring and logging.
