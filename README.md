# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/Adonis-TG1/Cloud-SOC/assets/142800366/a4604463-d3b0-4e6f-8bf1-91aa421ab93e)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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

## nsg-malicious-allowed-in 
![image](https://github.com/Adonis-TG1/Cloud-SOC/assets/142800366/429e0636-8c06-4d1f-bf3d-652f15d155f7)
<br>
## syslog-ssh-auth-fail 
![image](https://github.com/Adonis-TG1/Cloud-SOC/assets/142800366/95965347-8e90-47d6-829d-a7d8a79722b4)
)<br>
## windows-rdp-auth-fail 
![image](https://github.com/Adonis-TG1/Cloud-SOC/assets/142800366/efd950aa-2a0e-439b-9dd0-49fc84df2e96)
<br>



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-17 11:58:25
Stop Time 2023-08-18 11:58:25

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 24899
| Syslog                   | 3842
| SecurityAlert            | 19
| SecurityIncident         | 289
| AzureNetworkAnalytics_CL | 1803

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-08-20 10:45:11
Stop Time	2023-08-21 10:45:11

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1978
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness. In particular, there was a %92.06 reduction in Security event, %99.97% reduction in Syslog events and a %100 reduction for the remainders. 

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
