# Azure-Project
Fun With Azure
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/QJlsQUX.jpeg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/B0id2MD.jpeg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/9qVTO5Q.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/VszKwA1.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/E7iO3zc.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/zubhygl.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-13T17:45:25.0287633Z
Stop Time 2024-07-14T17:45:25.0287633Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 36223
| Syslog                   | 1282
| SecurityAlert            | 1
| SecurityIncident         | 145
| AzureNetworkAnalytics_CL | 971

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 7/15/2024, 1:51:12.795 AM
Stop Time	7/16/2024, 1:51:12.795 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8774
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0


# The real-world implications  of this lab are the application of the NIST 800-61 Incident Management Lifecycle (response)
Preparation -> Detection & Analysis -> Containment, Eradication & Recovery -> Post-Incident Activity 

Preparation was initiated by ingestion of the logs into Log Analytics Workspace and Sentinel alert rules. Detection & Analysis was done via the attack map and logs. Containment, eradication and recovery were completed using an Incident Response Playbook. Documentation of findings and closure of incidents within Sentinel were completed as the last step. 


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.


