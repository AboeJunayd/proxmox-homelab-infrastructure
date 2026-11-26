# Troubleshooting

[Back to project overview](../README.md)

These cases summarize issues investigated during the lab build. Results reflect the recorded checkpoints.

## 1. AD DNS Queries Returned NXDOMAIN Through Unbound

### Symptom

AD DNS queries succeeded when sent directly to DC01, but returned NXDOMAIN through OPNsense Unbound.

### Investigation

The zone forwarder pointed to DC01, and clearing the cache did not resolve the issue. Inspecting the DNS authority response revealed a local `test.` SOA, indicating that resolver-local zone handling was affecting the query.

### Resolution

Added explicit transparent handling for `ad.kcprojects.test` in the persistent Unbound configuration while retaining the zone-specific forwarder to DC01.

### Result and Lesson

AD host and LDAP SRV queries subsequently resolved through Unbound.

Comparing direct and recursive DNS responses helped identify the failing layer before changing network access rules.

## 2. Management GUI Unreachable During VLAN Cutover

### Symptom

OPNsense management access failed while moving the administration workstation from the original untagged LAN to VLAN 10.

### Investigation

The initial cutover required console recovery to restore a known configuration.

During the controlled retry, packet capture showed incoming TCP SYN packets without a successful handshake. Firewall logs identified a private-network block matching traffic before the intended HTTPS exception.

### Resolution

Moved the required management exception ahead of the private-network block and checked the active firewall rules.

A temporary second management interface supported the transition. It was removed after connectivity checks.

### Result and Lesson

HTTPS management access was restored.

An allow rule only helps when traffic reaches it. Matching firewall logs and an independent console path were essential to diagnosing and recovering from the failure.

## 3. Management Networking Failed After Reboot

### Symptom

Xubuntu management connectivity worked temporarily but failed to remain reliable after reboot.

### Investigation

Runtime addressing did not match the intended persistent configuration.

Inspection of the merged Netplan configuration revealed overlapping definitions for the same interface. An installer configuration retained `dhcp4: true` alongside static settings introduced through NetworkManager.

### Resolution

Backed up the source configuration, disabled DHCP in the conflicting installer definition and inspected the merged configuration before applying it.

After reboot validation, the temporary network profile and second virtual NIC were removed.

### Result and Lesson

The management interface retained its static address and default route after reboot. Gateway, dashboard and internet access succeeded; continued access after removing the temporary NIC was confirmed.

Validate persistent configuration and reboot behavior before declaring a network change complete.

## Related Documentation

- [Network Design](network-design.md)
- [Firewall Configuration](firewall-configuration.md)
- [Validation](validation.md)
