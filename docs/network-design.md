# Network Design

[Back to project overview](../README.md)

The lab uses five functional VLANs behind OPNsense. Proxmox provides virtual switching; OPNsense provides inter-VLAN routing, firewall controls and outbound NAT.

## Virtual Switching

| Component | Function |
|---|---|
| vmbr0 | Connects the physical interface, OPNsense WAN and Proxmox host management to the upstream network |
| vmbr1 | Internal VLAN-aware bridge with no physical port or Proxmox host IP |
| OPNsense WAN interface | Attached to vmbr0 |
| OPNsense internal interface | Attached to vmbr1 as the VLAN trunk |
| Endpoint interfaces | Attached to vmbr1 with their assigned VLAN tag |

The original untagged LAN no longer has an IPv4 address. Its parent interface remains enabled to carry the VLAN interfaces.

Traffic between different VLANs passes through OPNsense. Traffic within the same VLAN remains within that broadcast domain.

## VLANs and Addressing

| VLAN | Zone | Subnet | Gateway |
|---|---|---|---|
| 10 | Management | 10.10.10.0/24 | 10.10.10.1 |
| 20 | Servers | 10.10.20.0/24 | 10.10.20.1 |
| 30 | Workstations | 10.10.30.0/24 | 10.10.30.1 |
| 40 | Security | 10.10.40.0/24 | 10.10.40.1 |
| 50 | DMZ | 10.10.50.0/24 | 10.10.50.1 |

Infrastructure hosts use static addresses. Windows workstations receive DHCP leases.

VLAN 60 was reserved during planning and is outside the implemented baseline.

## DHCP

Dnsmasq on OPNsense provides DHCP for the workstation VLAN.

| Setting | Value |
|---|---|
| Address pool | 10.10.30.100–10.10.30.200 |
| Default gateway | 10.10.30.1 |
| DNS server | 10.10.20.10 — DC01 |
| Domain suffix | ad.kcprojects.test |

Clients use AD DNS to support domain discovery, authentication and Group Policy.

## DNS Resolution

| Source or query | Resolution path |
|---|---|
| Windows workstations | DC01 at 10.10.20.10 |
| DC01 external queries | Forwarded to OPNsense at 10.10.20.1 |
| Management, security and DMZ hosts | Their local OPNsense gateway |
| AD-zone queries received by OPNsense | Forwarded specifically to DC01 |

Unbound provides DNS on port 53. Dnsmasq supplies local DNS registration on port 53053 behind Unbound; endpoints do not query that port directly.

Specific forwarding for `ad.kcprojects.test` lets non-domain hosts resolve AD records without forwarding all queries back to DC01.

The internal record `lab.ad.kcprojects.test` resolves to the Nginx server at `10.10.50.20`. Successful DNS resolution and application reachability are separate checks.

## Network Access Boundaries

OPNsense permits required service paths between zones and restricts other private-network access. Selected outbound internet access uses NAT through the WAN interface.

The DMZ hosts an internal web service. No public inbound port forward is configured for Nginx or Wazuh.

VLAN separation establishes network boundaries; firewall policy determines which routed connections are permitted. Endpoint firewalls remain relevant within each VLAN.

The documented validation covers IPv4. IPv6 isolation has not been established by the available evidence.

## Management and Recovery

The Xubuntu administration workstation resides in VLAN 10. Proxmox host management remains on the upstream network, with host firewall controls and Tailscale for remote administration.

Hypervisor console access provides a recovery path when guest networking or OPNsense management becomes unavailable.

## Migration Approach

The original flat network was migrated in stages:

1. Enable VLAN-aware switching and configure OPNsense VLAN interfaces.
2. Establish gateways, firewall exceptions and workstation DHCP.
3. Move workloads individually and update addressing, DNS and agent settings.
4. Validate required services and selected blocked connections.
5. Complete management cutover and verify connectivity after reboot.

This sequence helped isolate failures while retaining a recovery path.

## Related Documentation

- [Infrastructure Architecture](architecture.md)
- [Firewall Configuration](firewall-configuration.md)
