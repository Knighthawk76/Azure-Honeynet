# Constructing a SOC + Honeynet in Azure With Live Traffic and Real Time Analysis
![Cloud Honeynet / SOC](https://imgur.com/weuCcuE.jpeg)
## Introduction and Project Purpose

In this project, I constructed a mini SOC (security operations center) and Honeynet powered by Azure Cloud Services. I deployed multiple resources including Linux and Windows VM's and ingested logs from the various resources into a Log Analytics workspace, which is then used by SIEM tools including Microsoft Sentinel to build attack maps, trigger alerts, and respond to incidents. After allowing the unsecured environment to accumulate incidents. I utilized Sentinel to measure security metrics in that insecure environment for 24 hours.  After the 24 hour period, I utilized NIST best practices to apply security controls with the intention to harden the environment, and improve security posture. I then observed and logged metrics for another 24 hours and published the results below. The metrics we observed are as follows...

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening and Implementing The Nist Recommended Security Controls
![Architecture Diagram](https://github.com/Knighthawk76/Azure-Honeynet/assets/152114740/76da37c9-f4fd-47cd-a6e9-37ef0f349e05)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/Knighthawk76/Azure-Honeynet/assets/152114740/42704021-d425-4223-877f-d10ec5c77d68)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

BEFORE the hardening stage of the project, I deployed all the resources into a virtual network with no NSGs or firewalls enabled. The resources were left vulnerable to network traffic in order to log metrics, and to guage just how fast, and how hard my resources would be hit with malicious attempts to breach. The Virtual Machines had both their Network Security Groups, ACLs and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

After the 24 hour period, I hardened the Network Security Groups by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/Knighthawk76/Azure-Honeynet/assets/152114740/d1e2cedb-aa98-4e6e-aa29-48807bf69ee9)<br>
![Linux Syslog Auth Failures](https://github.com/Knighthawk76/Azure-Honeynet/assets/152114740/26cdde89-ab68-48b6-a9fe-d5cd21be4558)<br>
![Windows RDP/SMB Auth Failures](https://github.com/Knighthawk76/Azure-Honeynet/assets/152114740/32d25a37-0b65-4409-9a67-68e2e3209b32)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 11/23/2023 23:15:28
Stop Time 11/24/2023 23:15:28

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2500
| Syslog                   | 3773
| SecurityAlert            | 0
| SecurityIncident         | 145
| AzureNetworkAnalytics_CL | 103

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 11/25/2023 17:57:56
Stop Time	11/25/2023 17:57:56

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 300
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, I was successful in constructing a home SOC, and a mini honeynet in Microsoft Azure. I leveraged Azure Log Analytics Workspace to ingest logs and KQL queries to create and develop rules for my SIEM tools. I then used Siem tools like Microsoft Sentinel  to create alerts for incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. The data suggests that the number of security events and incidents drastically diminished after the security controls were applied. 

After hardening, the Linux machine saw a 99.97% reduction in incidents, while the Windows machine saw a 88% decrease in security events. Inbound Malicious flows dropped to 0, and Sentinel events dropped to 0 as well.  I was dissapointed to see that my SQL instance did not see any attepts at brute force, however I believe that given more time I would have seen significant activity. It is also worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.  This project illustrates just how critical it is to utilize security controls in order to minimize vulnerabilities and secure and protect important data.

