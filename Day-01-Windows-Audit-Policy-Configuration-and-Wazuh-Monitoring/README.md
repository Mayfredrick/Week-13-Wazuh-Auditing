# Week 13 – Day 01: Windows Audit Policy Configuration and Wazuh Monitoring

## Objective
Review Windows Server 2022 audit policies, enable Group Membership and Process Creation auditing, and verify that Wazuh collects the resulting security events.

## Lab Environment
- SIEM: Wazuh
- Endpoint: Windows Server 2022
- Agent: WinServ22
- Domain: HOMELAB.LOCAL
- PowerShell: Administrator

## Part 1 – Initial Audit Policy Review

Used the following command:
powershell
auditpol /get /category:*

Initial findings:
- Group Membership: No Auditing
- Process Creation: No Auditing
- Logon: Success and Failure
- Audit Policy Change: Success
- Security Group Management: Success
- User Account Management: Success

## Part 2 – Initial Wazuh Investigation

Searched for the following Windows events over the previous 90 days:

| Event ID | Description | Result |
|---|---|---|
| 4719 | System audit policy changed | No matching events |
| 4902 | Per-user audit policy table created | No matching events |
| 4907 | Object auditing settings changed | No matching events |

These results reflect the available Wazuh data and selected search period.

## Part 3 – Enable Windows Audit Settings

Enabled successful Group Membership auditing:
powershell
auditpol /set /subcategory:"Group Membership" /success:enable

Enabled successful Process Creation auditing:
powershell
auditpol /set /subcategory:"Process Creation" /success:enable

Verified both settings using:
powershell
auditpol /get /subcategory:"Group Membership","Process Creation"

Both returned Success.

## Part 4 – Group Membership Test
Created an Active Directory security group:
- Name: AuditLab-Test
- Scope: DomainLocal
- Category: Security

Created a disabled test account:
- Name: AuditLabUser
- SamAccountName: AuditLabUser

Added the test account to the test group:
powershell
Add-ADGroupMember -Identity "AuditLab-Test" -Members "AuditLabUser"

### Wazuh Result

Event ID 4732 was successfully collected.
Observed details:
- Group: AuditLab-Test
- Member: AuditLabUser
- Description: A member was added to a security-enabled local group.

## Part 5 – Process Creation Test
Executed:
powershell
Start-Process -FilePath "$env:SystemRoot\System32\whoami.exe" -Wait -NoNewWindow

Searched Wazuh for:
agent.name:WinServ22 AND data.win.system.eventID:4688

### Wazuh Result

- Six matching Event ID 4688 records were returned.
- One event involved the whoami.exe test process.

This confirmed that Process Creation auditing was working and Wazuh collected the test process event.

## Conclusion

Successfully enabled and verified Group Membership and Process Creation auditing on Windows Server 2022.

Generated controlled test activity and confirmed that Wazuh collected Event ID 4732 for the group membership change and Event ID 4688 for the whoami.exe process.

This exercise demonstrated the importance of Windows audit policy configuration when monitoring security events through a SIEM.

## Skills Practiced
- Windows Security Auditing
- PowerShell Audit Policy Management
- Active Directory Group Management
- Windows Event ID Analysis
- Wazuh Threat Hunting
- Process Monitoring
- Security Event Verification
