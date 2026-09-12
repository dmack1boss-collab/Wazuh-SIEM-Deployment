# Wazuh SIEM Deployment & Endpoint Threat Monitoring

## Overview
Deployed a Wazuh Cloud SIEM/XDR environment and enrolled a Windows endpoint 
agent to monitor for security events in real time.

## What I Did
- Set up a Wazuh Cloud trial environment
- Deployed the Wazuh agent on a Windows endpoint via PowerShell/MSI installer
- Configured the dashboard to monitor authentication events and MITRE ATT&CK techniques

## The Problem
After installation, the agent failed to connect to the manager. Logs showed:
ERROR: (4112): Invalid server address found: '0.0.0.0'
ERROR: (1215): No client configured. Exiting.

## Root Cause
The MSI install parameters (`WAZUH_MANAGER`, `WAZUH_REGISTRATION_PASSWORD`) 
were not being applied correctly, leaving the agent's config file (`ossec.conf`) 
with a blank/default manager address and no enrollment block.

## How I Fixed It
1. Verified the issue by inspecting `ossec.conf` directly
2. Manually corrected the `<address>` field with the correct manager hostname
3. Manually added the missing `<enrollment>` block, including the manager 
   address and path to a password file
4. Created the missing `etc` directory and `authd.pass` file referenced 
   in the config
5. Restarted the service and confirmed successful enrollment via logs:

   
## Result
The agent successfully connected and began reporting events. The dashboard 
below shows detected authentication failures mapped to the MITRE ATT&CK 
"Account Access Removal" technique.

![Dashboard Overview](screenshots/01-dashboard-overview.png)
![Detected Events](screenshots/02-detected-events.png)
![Troubleshooting Commands](screenshots/03-troubleshooting-commands.png)

## Skills Demonstrated
- SIEM/XDR deployment and configuration (Wazuh)
- Windows agent installation and troubleshooting
- PowerShell scripting and log analysis
- XML configuration file editing
- Security event analysis and MITRE ATT&CK framework familiarity
