## Mimikatz Detection Using Sysmon and Windows PowerShell Event in Splunk
### Table of Contents
- [Overview](#overview)  
- [Requirements](#requirements)  
- [Mimikatz Behavior](#mimikatz-behavior)  
- [Mimikatz Detection](#mimikatz-detection)
- [Preparation](#preparation)
- [References](#references)
    
### Overview
This project demonstrates how to detect credential dumping attacks using Mimikatz. Mimikatz is a powerful open-source tool widely used by attackers to extract plaintext passwords, hashes, PIN codes, and Kerberos tickets from Windows memory, enabling credential dumping attacks. Detecting Mimikatz activity is crucial for identifying advanced intrusions and preventing lateral movement within networks.  

### Requirements
- Windows VM
- Splunk Enterprise server
- Splunk Universal Forwarder installed on Windows host
> **ℹ️ Info:** If you haven’t ingested Windows logs into Splunk yet, check out my [Windows Logs Ingestion project](https://github.com/elvanalandi/Home-Cybersecurity-Lab/tree/main/Splunk/Windows%20Logs%20Ingestion).

### Mimikatz Behavior
Mimikatz can perform various credential dumping techniques, such as Pass-the-Hash and Pass-the-Ticket attacks.  
Below are some of its key functionalities:  
- Dumps login credentials from LSASS memory
- Performs Pass-the-Hash attacks on NTLM hashes
- Executes Pass-the-Ticket attacks using Kerberos tickets
- Dumps SAM database hashed
- Manipulates tokens for privilege escalation or impersonation
- Simulates a domain controller to replicate account credentials (DCSync attack)
- Loads and injects DLLs to interact with system security components and extract credentials
With those capabilities, Mimikatz frequently interacts with `lsass.exe`, loads sensitive DLLs, and carries out operations that require elevated memory access rights.  
Mimikatz is categorized as a post-exploitation tool because it requires prior access to the system. It is often combined with other tools and techniques like Cobalt Strike to bypass security defenses and evade detection.

### Mimikatz Detection
To detect these behaviors, we can focus on the following patterns:  
- **Suspicious memory access to LSASS** (`lsass.exe`)  
  Detected via Sysmon Event ID 10  (Process Access) or Windows Security Event ID 4688 (Process Creation).  
- **DLL injection or loading**  
  Detected via Sysmon Event ID 7 (Image Loaded).  
- **Suspicious command-line activity**  
  Detected via Sysmon Event ID 1 (Process Creation) or Windows Security Event ID 4656 (Handle Manipulation).  
- **PowerShell script execution and logging**  
  Detected via PowerShell Script Block Logging — Event ID 4104.  
- **Token manipulation**  
  Detected via Windows Security Event ID 4703 (Token Right Adjusted).
- **Pass-the-Hash attack**  
  Detected via Windows Security Event ID 4624 (Logon Successful).
- **Pass-the-Ticket attack on Kerberos**  
  Detected visa Windows Security Event ID 4769 (A Kerberos service ticket was requested).

### Preparation
> **ℹ️ Info:** For now, I use only Sysmon and PowerShell Script Block Logging to detect Mimikatz. I plan to add more Windows Security Event IDs in the future to reduce false positives.

### References
- [Detect Mimikatz using PowerShell Script Block Logging](https://research.splunk.com/endpoint/8148c29c-c952-11eb-9255-acde48001122/)
- [Detect Mimikatz using Loaded Images](https://research.splunk.com/deprecated/29e307ba-40af-4ab2-91b2-3c6b392bbba0/)
- [Mimikatz Detection using Windows Events](https://github.com/Th1ru-M/Windows-Threat-Hunting/blob/master/Mimikatz%20Detection)
- [Detect Pass the Hash Attack](https://blog.netwrix.com/2021/11/30/how-to-detect-pass-the-hash-attacks/)
- [Windows Security Event ID 4769](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4769)
- [MITRE ATT&CK Pass the Ticket](https://attack.mitre.org/techniques/T1550/003/)
