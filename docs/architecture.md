# Infrastructure Architecture

## Target Architecture

### Network Infrastructure

Internet  
↓  
Home Router  
↓  
Proxmox VE  
↓  
OPNsense Firewall  
↓  
Isolated Lab Network

### Identity and Enterprise Services

- Windows Server 2025 - Active Directory, DNS and GPO
- Windows Server - AD CS / PKI
- Windows 11 Workstation 1
- Windows 11 Workstation 2
- Ubuntu Server - Nginx Webserver

### Security Monitoring

- Wazuh - Endpoint Security Monitoring
- Suricata - Network Intrusion Detection
- Zeek - Network Security Monitoring

### Security Operations

- Xubuntu SOC Workstation
- Kali Linux - Offensive Security

### VPN Connectivity

- WireGuard - Remote Access
- IPsec - Site-to-Site VPN
- Second OPNsense - Simulated Remote Site

### Future Network Segmentation

- VLAN 10 - Management
- VLAN 20 - Servers
- VLAN 30 - Workstations
- VLAN 40 - Security Monitoring
- VLAN 50 - DMZ
- VLAN 60 - VPN Clients
