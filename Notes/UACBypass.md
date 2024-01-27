# UAC Bypass

## Table of Contents

- [UAC Bypass](#uac-bypass)
  - [Table of Contents](#table-of-contents)
  - [Description](#description)
  - [Lab Setup](#lab-setup)
    - [Manual Lab Setup](#manual-lab-setup)
    - [PowerShell Script Lab Setup](#powershell-script-lab-setup) 
  - [Enumeration](#enumeration)
    - [Manual Enumeration](#manual-enumeration)
    - [Tool Enumeration](#tool-enumeration)
  - [Exploitation](#exploitation)
  - [Mitigation](#mitigation)
  - [References](#references)

## Description

UAC, or User Account Control, is a security feature implemented in Microsoft Windows operating systems, starting with Windows Vista. Its primary purpose is to enhance the security of the operating system by prompting users for permission or confirmation before allowing certain actions that may require administrative privileges.

When an application or a user attempts to perform tasks that require elevated permissions, such as installing software, making system changes, or modifying certain system settings, UAC prompts the user with a dialog box asking for confirmation or prompting the entry of administrator credentials. 

All protected objects in Windows are labeled with an integrity level. The default integrity level is medium, which is assigned to most users, system files, and registry keys on a system.

The following table illustrates the different levels of integrity:

| Integrity Name | Details |
|:-----------:|:-----------:|
| **Untrusted** | Processes logged in anonymously are automatically categorized as Untrusted. For instance, consider Google Chrome. |
| **Low** | The Low integrity level is the level used by default for interaction with the Internet. It's important to note that a process with a lower integrity level is restricted from writing to an object with a higher integrity level.|
| **Medium** | Processes initiated by standard users typically carry a medium integrity label, including those started by users within the administrators group. |
| **High** | Administrators are granted the High integrity level. Moreover, the root directory is safeguarded with a high-integrity label. |
| **System** | Most services are designated with System integrity. |
| **Installer** | The Installer integrity level is a unique case and represents the highest of all integrity levels. |

The default configuration for UAC is **Prompt for consent for non-Windows binaries**, but can also have different configuration settings.

The following table illustrates various configuration settings of UAC in a system:

| Prompt Name | Details |
| **Prompt for consent for non-Windows binaries** | This is the default. When an operation for a non-Microsoft application requires elevation of privilege, the user is prompted on the secure desktop to choose between **Permit** or **Deny**. If the user selects **Permit**, the operation continues with the user's highest available privilege. |
| **Prompt for credentials:** | An operation that requires elevation of privilege prompts the administrator to enter the user name and password. If the administrator enters valid credentials, the operation proceeds with the appropriate privilege. |
| **Prompt for consent:** | An operation that requires elevation of privilege prompts the administrator to select **Permit** or **Deny**. If the administrator selects **Permit**, the operation continues with the administrator's highest available privilege. |
| **Elevate without prompting** | Assumes that the administrator will permit an operation that requires elevation, and additional consent or credentials are not required. |

A UAC bypass refers a technique that allows a medium integrity process to elevate itself or spawn a new process in high integrity, without prompting the user for consent. 

:information_source: In this section, I will present only one example of a UAC bypass, as there are numerous different UAC bypass techniques. 

## Lab Setup

### Manual Lab Setup

Open a cmd with local Administrator privileges and use the following command:

```
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v EnableLUA /t REG_DWORD /d 0x1 /f
```

Outcome:

![Enable-UAC](/Pictures/Enable-UAC.png)

### PowerShell Script Lab Setup

To set up the lab with the 'UAC' feature is by using the custom PowerShell script named [EnableUAC.ps1](/Lab-Setup-Scripts/EnableUAC.ps1).

Open a PowerShelll with local Administrator privileges and run the script:

```
.\EnableUAC.ps1
```

Outcome:

![UAC-Enable-Script](/Pictures/UAC-Enable-Script.png)

## Enumeration

### Manual Enumeration

To perform manual enumeration and identify whether a Windows workstation has enabled UAC, you can use the following command from a command prompt:

```
reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```

Outcome:

![UAC-Manual-Enumeration](/Pictures/UAC-Manual-Enumeration.png)

### Tool Enumeration

To run the SharpUp tool and perform an enumeration if the `UAC` feature is enabled, you can execute the following command with appropriate argument:

```
SharpUp.exe audit
```

Outcome:

![UAC-Tool-Enumeration](/Pictures/UAC-Tool-Enumeration.png)

## Exploitation

## References

- [Integrity Levels by HackTricks](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/integrity-levels)
- [UAC by HackTricks](https://book.hacktricks.xyz/windows-hardening/authentication-credentials-uac-and-efs/uac-user-account-control)
- [User Account Control settings and configuration Microsoft](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/settings-and-configuration?tabs=intune)