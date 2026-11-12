# Week 13 – Day 4: Audit Policy Change Monitoring

## Objective
Monitor Windows audit policy changes and verify that Wazuh detects Event ID 4719.

## Lab Environment
- Windows Server 2022
- Wazuh Manager: Ubuntu Server
- Wazuh Agent: WinServ22 (Agent ID 003)
- Active Directory Domain: HOMELAB

## Activities Performed

1. Reviewed the Windows system audit policy.
2. Confirmed Audit Policy Change was enabled for Success.
3. Temporarily disabled successful Process Termination auditing.
4. Checked Windows Security logs for Event ID 4719.
5. Examined the event details.
6. Restored Process Termination auditing.
7. Searched Wazuh Threat Hunting for Event ID 4719.
8. Compared the Windows event with the Wazuh record.

## Windows Event Findings

Event ID: 4719 – System audit policy was changed

| Field | Value |
|---|---|
| Date | November 12, 2026 |
| Time | 5:28:30 AM |
| Account | Administrator |
| Domain | HOMELAB |
| Logon ID | 0xCE551 |
| Category | Detailed Tracking |
| Subcategory | Process Termination |
| Change | Success removed |

The event recorded the removal of successful Process Termination auditing.

## Wazuh Findings

Wazuh Threat Hunting returned a matching event.

Verified fields:

- data.win.system.eventID: 4719
- data.win.eventdata.subjectUserName: Administrator
- data.win.eventdata.auditPolicyChanges: Success removed

The Windows Security event and Wazuh record matched on the key fields examined.

## Remediation and Verification

After the controlled test, successful Process Termination auditing was restored using:

powershell
auditpol /set /subcategory:"Process Termination" /success:enable


The setting was verified afterward.

## Conclusion

This lab demonstrated how Windows records system audit policy changes and how Wazuh can detect those changes.

The controlled test successfully generated Event ID 4719, and the corresponding event appeared in Wazuh Threat Hunting.

This exercise provided practical experience with Windows security auditing, event investigation, and SIEM monitoring.

## Skills Practiced
- Windows audit policy management
- Event ID 4719 investigation
- PowerShell security log queries
- Wazuh Threat Hunting
- SIEM event verification
- Security event documentation
