# Week 13 – Day 5: Windows Account Management Monitoring

## Objective
Monitor Active Directory user account modifications and verify that Wazuh detects Event ID 4738.

## Lab Environment
- Windows Server 2022
- Wazuh Manager: Ubuntu Server
- Wazuh Agent: WinServ22 (Agent ID 003)
- Active Directory Domain: HOMELAB

## Activities Performed

1. Checked the User Account Management audit policy.
2. Confirmed successful auditing was enabled.
3. Inspected the existing john.admin account.
4. Confirmed its Description field was blank.
5. Temporarily changed the account description.
6. Searched Windows Security logs for Event ID 4738.
7. Searched Wazuh Threat Hunting for Event ID 4738.
8. Compared the Windows and Wazuh event fields.
9. Restored the original blank Description field.

## Windows Event Findings

**Event ID:** 4738 – A user account was changed

| Field | Value |
|---|---|
| Date | November 13, 2026 |
| Time | 6:08:10 AM |
| Subject Account | Administrator |
| Subject Domain | HOMELAB |
| Target Account | john.admin |
| Target Domain | HOMELAB |
| Target SID | S-1-5-21-2201603593-1195137882-3448451803-1107 |

The event recorded a modification to the john.admin account.

## Wazuh Findings

Wazuh Threat Hunting returned the matching Event ID 4738.

Verified fields:

- data.win.system.eventID: 4738
- data.win.eventdata.subjectUserName: Administrator
- data.win.eventdata.targetUserName: john.admin
- data.win.eventdata.targetSid: S-1-5-21-2201603593-1195137882-3448451803-1107

The selected fields matched the Windows Security event.

## Cleanup

The temporary account description was removed using:

powershell
Set-ADUser john.admin -Clear Description


The original blank Description field was restored.

## Conclusion

This lab demonstrated how Windows audits Active Directory user account modifications and how Wazuh detects Event ID 4738.

The controlled test successfully generated an account modification event, and Wazuh displayed the corresponding event details.

## Skills Practiced
- Active Directory account management
- Windows audit policy verification
- Event ID 4738 investigation
- PowerShell account administration
- Wazuh Threat Hunting
- SIEM event correlation and verification
