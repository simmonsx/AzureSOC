# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Table of Contents

- [Introduction](#introduction)
- [BEFORE Security Controls](#before-security-controls)
- [AFTER Security Controls](#after-security-controls)
- [Conclusion](#conclusion)
- [Contributing and Acknowledgement](#contributing-and-acknowledgement)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measured metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

### Architecture Before Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

### Architecture After Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Before Security Controls
### Attack Maps 
  
NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](CyberLab/before/nsgM-W.png)<br>
Linux Syslog Auth Failures
![Linux Syslog Auth Failures](CyberLab/before/ssh-W.png)<br>
Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](CyberLab/before/nsgM-W.png)<br>

### Log Metrics 

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 7/26/2023, 5:18:40 PM <br>
Stop Time 7/27/2023, 5:18:40 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12503
| Syslog                   | 30403
| SecurityAlert            | 9
| SecurityIncident         | 382
| AzureNetworkAnalytics_CL | 2007

See KQL Queries used to collect logs
    
Start & Stop Time:    
```Kusto
range x from 1 to 1 step 1
| project StartTime = ago(24h), StopTime = now()    
```
Security Event (Windows VM):
```Kusto
SecurityEvent
| where TimeGenerated >= ago(24h)
| count   
```
Syslog (Linux VMs):
```Kusto
Syslog
| where TimeGenerated >= ago(24h)
| count  
```
SecurityAlert (Microsoft Defender for Cloud):
```Kusto
SecurityAlert
| where DisplayName !startswith "CUSTOM" and DisplayName !startswith "TEST"
| where TimeGenerated >= ago(24h)
| count  
```
Security Incident (Sentinel Incidents)
```Kusto
SecurityIncident
| where TimeGenerated >= ago(24h)
| count  
```
NSG Inbound Malicious Flows Allowed
```Kusto
AzureNetworkAnalytics_CL 
| where FlowType_s == "MaliciousFlow" and AllowedInFlows_d > 0
| where TimeGenerated >= ago(24h)
| count    
```
NSG Inbound Malicious Flows Blocked
```Kusto
AzureNetworkAnalytics_CL 
| where FlowType_s == "MaliciousFlow" and DeniedInFlows_d > 0
| where TimeGenerated >= ago(24h)
| count   
```
  </details>

## After Security Controls
### Attack Maps

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

### Log Metrics

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 7/28/2023, 12:26:45 AM <br>
Stop Time	7/29/2023, 12:26:45 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 688
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

| Metric                   | BEFORE | AFTER | Percent of Reduction
| ------------------------ | ------ | ----- | --------------------
| SecurityEvent            | 12503  | 688   | -98.48%
| Syslog                   | 30403  | 25    | -99.94%
| SecurityAlert            | 9      | 0     | -100.00%
| SecurityIncident         | 382    | 0     | -100.00%
| AzureNetworkAnalytics_CL | 2007   | 0     | -100.00%

It is worth noting that if regular users heavily utilized the resources within the network, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.


## Contributing and Acknowledgement

Sincere thanks to all that offered their advice and support throughout the project. Contributions to this project are welcome! Feel free to submit a pull request if you find any issues or want to enhance the project.
