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

