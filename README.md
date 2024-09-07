# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC + Honeynet in Azure](https://github.com/user-attachments/assets/d6b0d64f-ab87-4ae3-89ae-48598c936885)




## Introduction


In this project, I set up a controlled environment in Azure to attract and observe potential cyber threats. I collected data from different parts of this environment and sent it to a central system where it could be analyzed. Using Microsoft Sentinel, I created visual maps of the attacks, set up alerts for suspicious activity, and tracked incidents.

To measure the effectiveness of security measures, I first recorded some security metrics in an unsecured environment over 24 hours. Then, I applied security enhancements, measured the metrics again for another 24 hours, and compared the results. The metrics we'll look at include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening _ Security Controls ](https://github.com/user-attachments/assets/4ce902f5-92dd-4422-b418-77ac20c1694e)



## Architecture After Hardening / Security Controls
![Architecture After Hardening _ Security Controls](https://github.com/user-attachments/assets/bb7cfc48-1ec9-4193-b9ee-9157e4094687)


The setup of the mini honeynet in Azure included several key components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

**Before:** Initially, all resources were deployed and exposed to the internet. The Virtual Machines had their security settings wide open, allowing unrestricted access. Additionally, all resources were accessible through public internet connections, with no protection from private endpoints.

**After:** To improve security, I tightened the Network Security Groups by blocking all traffic except from my admin workstation. I also secured other resources using their built-in firewalls and enabled Private Endpoints to ensure they were no longer exposed to the public internet.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
