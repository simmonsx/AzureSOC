Microsoft Sentinel - Threat Management
---
2023-07-27 - 10:20 PM <br>
Response to Numerous Brute Force Attempts Incident Alerts <br>
Status: CLOSED - True Positive: Suspicious Activity <br>
Desc: Poor NSG inbound rules allowed unwanted activity <br>

Solution:
- Enabled AAD MFA 
- Windows VM: Deleted ALLOW-ALL inbound network traffic NSG rule
- Windows VM: Deleted ALLOW-ALL RDC attempts NSG rule
- Windows VM: Created NSG In-bound of highest priority; restricted all inbound network traffic to that VM/Subnet to only personal IP 
- Linux VM: Deleted ALLOW-ALL inbound network traffic NSG rule
- Linux VM: Deleted ALLOW-ALL SSH attempts NSG rule
- Linux VM: Created NSG In-bound of highest priority; restricted all inbound network traffic to that VM/Subnet to only personal IP
---
2023-07-27 - 10:40 PM <br>
Response to the following alerts: 
- Possible Privilege Escalation (Azure Key Vault Critical Credential Retrieval or Update)
- Malware Detected
- Brute Force SUCCESS - Azure Active Directory
- Malware Detected

Status: CLOSED - Benign positive - Suspicious but Expected <br>
Desc: Byproduct of testing <br>

Solution: Alerts closed for ‘Benign positive - Suspicious but Expected’. Alerts were attentional activated for testing purposes

