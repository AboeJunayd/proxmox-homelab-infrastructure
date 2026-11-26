# Firewall Configuration

[Back to project overview](../README.md)

OPNsense controls routed traffic between the five lab VLANs and provides restricted outbound connectivity.

## Policy Structure

The documented IPv4 policy follows this order on each source interface:

1. Permit required gateway and inter-VLAN services.
2. Block other traffic to private networks.
3. Permit selected internet access.
4. Deny unmatched traffic.

Required exceptions must precede the private-network block. Otherwise, that block can prevent legitimate traffic from reaching its allow rule.

Rules are stateful: return traffic matching an established state does not require a broad reverse allow rule.

## Required Traffic Flows

| Source | Destination | Service | Purpose |
|---|---|---|---|
| Management workstation | Management gateway | HTTPS, DNS and ICMP | Firewall administration, name resolution and connectivity checks |
| Management workstation | Wazuh | HTTPS and SSH | Dashboard access and host administration |
| Management workstation | Web01 | SSH | Web server administration |
| Windows workstations | DC01 | DNS and required AD services | Domain discovery, authentication and Group Policy |
| Windows workstations | Web01 | HTTP | Internal web service testing |
| Five monitored endpoints | Wazuh | TCP 1514 | Agent communication |
| Servers, security and DMZ hosts | Their local gateway | DNS and ICMP | Name resolution and gateway checks |
| Lab zones | Internet destinations | TCP 443, after preceding restrictions | Restricted outbound connectivity |

This is a service-level summary. It does not reproduce every rule, alias or AD port definition.

Agent enrollment is separate from ongoing Wazuh agent communication.

## Private Network Restrictions

The `PRIVATE_NETWORKS` alias is used to restrict access to private destinations, including the upstream home network.

Explicit service exceptions allow necessary internal connections before this block is evaluated. The alias contents determine which destinations the block covers.

Proxmox host management remains on the upstream network. Guest access is restricted through OPNsense, while the host firewall separately limits management access.

No public inbound port forward is configured for Nginx or Wazuh.

## Troubleshooting: Management Access

During management VLAN cutover, an intended HTTPS exception was placed after the private-network block.

Packet capture showed incoming TCP SYN packets without a successful handshake. Firewall logging identified the matching block rule.

Correcting the rule order restored GUI access. This demonstrated why troubleshooting must establish which rule actually processed the traffic.

## Validation

The build notes record:

- Selected DNS, HTTPS and internal application connections.
- Selected blocked connections to private destinations.
- Management access after the final network correction and reboot.
- Five active Wazuh agents at the final checkpoint.

These observations support the tested paths. They do not establish that every source, destination and protocol combination has been validated.

A rule-table screenshot shows configuration intent. Endpoint results and matching firewall logs provide stronger evidence of enforcement.

## Validation Gaps

- Retain a current capture of the applied rules and alias definitions.
- Document a compact matrix of allowed and denied test flows.
- Review remaining rules on the former untagged LAN.
- Establish the IPv6 configuration and validate its isolation separately.

The historical flat-LAN screenshot is excluded because it does not represent the segmented baseline.

## Related Documentation

- [Infrastructure Architecture](architecture.md)
- [Network Design](network-design.md)
