### Splunk showed "No users are created" after launching
The initial Splunk installation entered a broken bootstrap/authentication state after incomplete initialization. 

**Resolution**
  - Performed a clean install of Splunk Enterprise on Ubuntu Server and recreated the admin account during first-run setup.

**Lessons Learned**
  - Learned the importance of clean service initialization and proper shutdown procedures for Splunk services. 
---
### VirtualBox Networking
The initial VirutalBox configuration used standard NAT mode, which isolated the VMs from one another and resulted in overlapping IP addressing that prevented direct VM-to-VM communication. 
#### Resolution
Reconfigured the VMs to use a shared network configuration (Bridged Networking) to enable communication between:
  - The Windows endpoint VM
  - The Ubuntu Splunk Server VM
  - The Kali Linux VM
  - and the Host system
#### Lessons Learned
  - Explored the differences between NAT, NAT network, and bridged adapter modes in VirtualBox
  - Gained hands-on experience troubleshooting VM connectivity and network segmentation issues
  - Better understood how SIEM infrastructure depends on reliable Layer 3 (network layer) connectivity between endpoints and collectors.
---
### Forwarder Connection Troubleshooting
The initial Universal Forwarder setup appeared connected, but Windows event logs were not visible within Splunk searches. 
#### Resolution
 - Verified that the Splunk server was listening on TCP port 9997
 - Confirmed successful forwarder registration and active connection from the Windows VM
 - Tested the VM connectivity between Ubuntu Server and Windows 11
 - Reviewed Splunk's internal logs (index=_internal) to validate inbound forwarder communication
 - Identified that the forwarder connection was operational, but Windows Event Log inputs had not yet been configured
After configuring Windows event log monitoring and restarting the forwarder service, log data began successfully flowing into Splunk.
#### Lessons Learned
  - Learned how to validate SIEM ingestion pipelines layer-by-layer rather than assuming a single point of failure
  - Became familiar with troubleshooting Splunk forwarder connectivity and ingestion workflows
  - Better understood the distinction between:
      - Forwarder connectivity
      - Listening ports
      - and actual event collection/input configuration
  - Learned to use Splunk internal logs as a diagnostic source for troubleshooting ingestion issues
---
### Network Stablization and Static IP Configuration
**Changes Made**
- Configured a permanent static IP address on the Ubuntu Server: 192.168.68.61
- Standardized internal IP scheme across the lab:
- Ubuntu (Splunk Server): 192.168.68.61
- Windows Victim: 192.168.68.62
- Kali Attack Machine: 192.168.68.63
- Fixed Netplan configuration issues and resolved YAML indentation errors
- Verified correct routing and internet connectivity across all systems
  
**Outcome**
- All virtual machines can now communicate reliably over the internal network
- Stable addressing enables consistent Splunk forwarding and log ingestion
- Improved reproducibility of the lab environment for future testing and documentation

**Lessons Learned**
- When utlizing multiple hardware solutions with multiple VMs Bridged connections are best
- YAML Formatting is strict; indentation breaks configuration
- Static IP consistency is essential for log forwarding and SIEM function

---

### Logs showing raw hex
**Changes Made**
- Logs being ingested by Splunk were showing raw hex due to the fact that they were added to the forwarder incorrectly. 
- Fixed the issue by adding the Log files to input.conf and removing them from the monitor

**Outcome**
- All log files now show actionable information instead of hex.

**Lessons Learned**
- Add files for ingestion through the input.conf

---
### Added Sysmon and Sysmon addon 
**Changes Made**
- Downloaded Sysmon64, Sysmon-config by SwiftOnSecurity, and the Sysmon addon from Splunk
- Added WinEventLog://Microsoft-Windows-Sysmon/Operational to inputs.conf on the forwarder
- Extracted Sysmon 64 and Sysmon-config onto the Forwader, Windows 11 (victim) 
- Sent the addon to the Ubuntu server via scp
- Extracted the addon, transferred ownership to splunk user on the Ubuntu server

**Outcome**
- XML logs now parse in Splunk

**Lessons Learned**
- Splunk doesn't hide information, all logs are parsed in xml, making them difficult to read from the index, but easier to search for useful information

