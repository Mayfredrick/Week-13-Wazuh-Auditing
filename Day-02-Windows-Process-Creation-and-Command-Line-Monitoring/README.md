# Week 13 – Day 2: Windows Process Creation and Command-Line Monitoring

## Objective
Investigate Windows process creation events using Wazuh and enable command-line auditing to capture additional details about executed programs.

## 1. Initial Investigation
Searched Wazuh Threat Hunting for Windows Event ID 4688 on the WinServ22 agent.

Query:
agent.name:WinServ22 AND data.win.system.eventID:4688

The search returned six events. One event involved whoami.exe.

Observed event details:

| Field | Value |
|---|---|
| Event ID | 4688 |
| Process name | C:\Windows\System32\whoami.exe |
| Process ID | 0x1350 (4944) |
| Parent process | PowerShell 7 (pwsh.exe) |
| Creator process ID | 0x584 (1412) |
| User | Administrator |

The initial event's command-line field was empty.

## 2. Enable Command-Line Auditing
Checked the Windows registry setting:
powershell
Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Audit" -Name ProcessCreationIncludeCmdLine_Enabled -ErrorAction SilentlyContinue

The value was not initially displayed.
Created the registry setting:
powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Audit" -Name "ProcessCreationIncludeCmdLine_Enabled" -Value 1 -PropertyType DWORD -Force

Verified the setting. The value returned was 1.

This enables Windows to include command-line information in process-creation audit events.

## 3. Controlled Process-Creation Test
Executed the following PowerShell command:
powershell
Start-Process -FilePath "$env:SystemRoot\System32\whoami.exe" -ArgumentList "/all" -Wait -NoNewWindow

Returned to Wazuh and searched for Event ID 4688.

Command line captured by Wazuh:

"C:\Windows\System32\whoami.exe" /all

This confirmed that the new test event included the command-line information.

## 4. Findings

- Windows Event ID 4688 records process creation.
- The initial event identified whoami.exe and its PowerShell 7 parent process.
- The initial command-line field was empty.
- Enabling ProcessCreationIncludeCmdLine_Enabled with a value of 1 allowed command-line information to be included.
- Wazuh successfully displayed the command line from the follow-up test.

## 5. Skills Practiced

- Windows process-creation auditing
- Event ID 4688 investigation
- PowerShell process execution
- Windows registry configuration
- Wazuh Threat Hunting
- Process ID and parent process identification
- Command-line monitoring

## Conclusion

This lab demonstrated how to investigate Windows process creation events and configure command-line auditing. The follow-up test confirmed that Wazuh could collect the command line used to execute whoami.exe.

Process creation and command-line information can help an analyst understand which programs were executed, under which account, and with what arguments. These events provide useful investigation evidence but do not independently establish malicious activity.
