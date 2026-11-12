# Week 13 – Day 3: Process Termination Monitoring

## Objective
Enable Windows Process Termination auditing and investigate whether Wazuh records process termination events.

## Lab Environment
- Windows Server 2022
- Wazuh Manager: Ubuntu Server
- Wazuh Agent: WinServ22 (Agent ID 003)

## Activities Performed

1. Checked the Windows Process Termination audit policy.
2. Enabled successful Process Termination auditing.
3. Verified that the setting was enabled.
4. Started and closed Notepad to generate test events.
5. Checked Windows Security logs for Event ID 4689.
6. Searched Wazuh Threat Hunting for Event ID 4689.
7. Checked the Wazuh agent configuration and manager archive file.

## Findings

Windows successfully recorded two Notepad termination events:

| Event ID | Process | Process ID |
|---|---|---|
| 4689 | notepad.exe | 0xfa8 |
| 4689 | notepad.exe | 0x17e8 |

Both events were visible in the Windows Security log.

However, the corresponding events were not found in Wazuh Threat Hunting during the investigation.

The Wazuh agent was connected, and its Security event channel was configured. The manager's `archives.log` file existed but had a size of 0 bytes.

## Conclusion

This lab demonstrated how to enable and verify Windows Process Termination auditing.

Windows generated and stored the expected Event ID 4689 records. However, I could not confirm that these specific events reached the Wazuh manager.

The reason they were not visible in Threat Hunting remains undetermined.

## Skills Practiced
- Windows audit policy configuration
- Event ID 4689 investigation
- PowerShell event log queries
- Wazuh agent configuration review
- SIEM event collection troubleshooting
- Evidence-based security reporting
