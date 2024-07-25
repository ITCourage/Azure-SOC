# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I created a mini honeynet within Azure and collected log data from various resources into a Log Analytics workspace. This workspace was then utilized by Microsoft Sentinel to construct attack maps, trigger alerts, and generate incidents. Initially, I measured specific security metrics in an unprotected environment for 24 hours. Afterward, I implemented security controls to strengthen the environment and recorded the metrics again for another 24 hours. The metrics presented below include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Alerts triggered within Log Analytics)
- SecurityIncident (Incidents generated by Sentinel)
- AzureNetworkAnalytics_CL (Malicious traffic allowed into the honeynet)

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

"BEFORE" metrics: all resources were deployed and exposed to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, and all other resources were deployed with public endpoints accessible from the Internet. Private Endpoints were not utilized.

"AFTER" metrics: to improve security, Network Security Groups were configured to block all traffic except from my admin workstation. Additionally, all other resources were secured using their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
 ![nsg-malicious-allowed-in-(before)](https://github.com/user-attachments/assets/e561c8bf-0e99-46cd-a72f-f43f65757433)<br>

 ![linux-ssh-auth-fail(before)](https://github.com/user-attachments/assets/acb732c3-bc2e-4750-9925-ede75b7a2cad)<br>

![windows-rdp-auth-fail-before](https://github.com/user-attachments/assets/40f4a94c-a591-4dc0-9b55-ce7157b67b3c)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-15 17:00:21 
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21313
| Syslog                   | 14382
| SecurityAlert            | 10
| SecurityIncident         | 219
| AzureNetworkAnalytics_CL | 2207

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-07-19 00:49:36
Stop Time  2024-07-20 00:49:36

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 20889
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment before and after the implementation of security measures. The results showed a significant reduction in security events and incidents after applying the security controls, demonstrating their effectiveness.

It is important to note that if the network resources had been heavily utilized by regular users, more security events and alerts might have been generated within the 24-hour period following the implementation of the security controls.
