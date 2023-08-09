#Project Logs

Azure Intro
- v1-june27: Signed up for Microsoft Azure free trial, and set up a subscription.
- v2-june29: Created Honeynet (vm with no firewall) and SQL server.
- v3-june30: Created vm to fail logins into Honeynet and SQL. Tracked attempts through Windows event log.
- v4-july2: Experimented with Azure Active Directory; added new users with various assigned roles simulating actual practice (rg contributor, global reader, etc.).

Logging and Monitoring
- v1-july5: Learned about Azure logs and different log planes (AAD/Tenant Plane, Management Plan, Data Plane)
- v2-july9: Created LAW and enabled Microsoft Defender for Cloud to export collected data/logs back to LAW. Configured VMs (1 Windows, 1 Linux) installed with Log Analytics Agent to allow all inbound NSG settings, and to send collected flow logs back to LAW. 
- v3-july20: Created Storage account container and uploaded ip geo-data CSV files. Created Log Analytics Workspace within Azure Sentinel. Used Sentinel to ingest geo-data from Azure storage container through created watchlist, while LAW was used to aggregate logs. (Watchlist Upload Completed on 2023-07-18)
- v4-july22: Learned about KQL (Kusto Query Language) 
- v5-july 22: Created a set of Data Collection Rules (VM-DCR) to ingest Azure AD logs, tested through a dummy-user. Experimented with KQL log collection of mass sign-in failures through assigned global administrator user (attacker).
- v6-july23: Created a break-glass account in case of an emergency scenario.
- v7-july23: Created Activity Logs for the management plane. Tested log collection through the creation and deletion of dummy resource groups.
- v8-july23: Configured logging for Azure Storage and Key Vault. Tested log collection for both.

Microsoft Sentinel (SIEM)
- v1-july23: Introduced to Microsoft Sentinel.
- v2-july24: Created World Attack Maps using Sentinel Workbooks.
- v3-july26: Learned about Sentinel Analytics and created multiple scheduled query incident alerts.
- v4-july26: Tested Sentinel incident alerts through artificial traffic generation and manual tempering.
- v5-july26: Prepare for 24-hour insecure environment data capture period.
- v6-july27: Record 24-hour insecure environment captured graph and data.
- v7-july27: Introduced to incident closing and practicing incident response. Overall hardening of the environment in accordance with the NIST 800-61: Incident Management Lifecycle. 

Secure Cloud Configuration:
- v1-july27: Introduced to Regulatory Compliance in Microsoft Defender for Cloud(NIST 800-53, PCI DSS, CIS) and MDC Recommendations. Worked towards improving overall security posture and secure score
- V2-july27: Implement SC-7. Configure private line and firewall for Azure Key Vault and Storage Account. Create NSG for subnet 
- v3-july27: Record 24-hour secure environment data capture period.
