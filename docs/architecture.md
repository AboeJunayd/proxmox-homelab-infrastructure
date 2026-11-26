# Infrastructure Architecture

[Back to project overview](../README.md)

The lab provides identity, application and monitoring services for controlled security investigations. It runs on a single Proxmox VE host with 64 GB RAM, 1 TB NVMe storage and one physical network interface.

## Virtual Machines

| VM | Operating system / role | VLAN | Address |
|---|---|---|---|
| ubuntu-mgmt | Xubuntu administration workstation | 10 | 10.10.10.146 |
| opnsense-fw | Firewall, routing, DNS and DHCP | Trunk | .1 gateway in each lab subnet |
| DC01 | Windows Server 2025 — AD DS and DNS | 20 | 10.10.20.10 |
| WIN11-01 | Domain-joined Windows 11 workstation | 30 | DHCP |
| WIN11-02 | Domain-joined Windows 11 workstation | 30 | DHCP |
| wazuh01 | Ubuntu Server — Wazuh all-in-one | 40 | 10.10.40.40 |
| ubuntu-web01 | Ubuntu Server — Nginx | 50 | 10.10.50.20 |

The management VM appears as `xubuntu-vm` in Wazuh. Windows workstation addresses are DHCP leases and may change.

## Network Boundaries

- **vmbr0** connects the physical interface, OPNsense WAN and Proxmox host management to the upstream network.
- **vmbr1** is an internal VLAN-aware bridge with no physical port or Proxmox host IP.
- OPNsense carries the five lab VLANs through its internal virtual interface and controls routed traffic between them.
- Endpoint virtual interfaces use the VLAN tag assigned to their workload.

VLAN 10 contains the administration workstation. The hypervisor's own management interface remains on the upstream network, with host firewall controls and Tailscale for remote administration.

VLANs separate broadcast domains. OPNsense enforces policy between routed zones; traffic within a VLAN still requires endpoint controls.

## Identity Services

DC01 hosts the `ad.kcprojects.test` forest and provides Active Directory DNS.

Two Windows 11 clients are domain joined. Organizational units separate lab users, administrators, workstations and servers. Standard and administrative accounts are distinct.

Windows clients use DC01 for DNS. External queries are forwarded from DC01 to OPNsense.

## Application and Monitoring Services

Nginx provides an internal web service in the DMZ, allowing client-to-server connectivity and logging to be tested without public exposure.

Wazuh runs its manager, indexer and dashboard on one VM. Five agents cover DC01, both Windows workstations, the management workstation and the web server.

Agent connectivity is established. Specific log-source ingestion and detection behavior require separate validation.

## Design Decisions

| Decision | Reason |
|---|---|
| Separate workloads by function | Make required service paths and network boundaries explicit |
| Centralize routing through OPNsense | Apply and investigate inter-VLAN firewall policy |
| Keep console recovery available | Recover guest networking without depending on the affected management path |
| Use one Wazuh VM | Limit operational complexity while establishing telemetry |
| Keep Nginx internal | Provide a repeatable application test target without public publishing |

## Constraints

The physical host, storage and sole domain controller are single points of failure. Wazuh components share one VM and have no independent availability boundary.

Startup priorities help order infrastructure boot, but delays do not establish service readiness.

Backup archive integrity has been checked. An isolated restore and application recovery have not yet been demonstrated.

## Related Documentation

- [Network Design](network-design.md)
- [Firewall Configuration](firewall-configuration.md)
