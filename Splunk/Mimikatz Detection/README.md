## Mimikatz Detection Using Sysmon and Windows PowerShell Event in Splunk
### Table of Contents
- [Overview](#overview)  
- [Requirements](#requirements)  
- [Mimikatz Behavior](#mimikatz-behavior)  
- [Mimikatz Detection Strategy](#mimikatz-detection-strategy)
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

### Mimikatz Detection Strategy
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
1. **Configure Sysmon Event ID 10**  
   To collect Event ID 10 (Process Access), you need to modify the Sysmon configuration.  
   Add the following code inside the <ProcessAccess> tag to monitor access to lsass.exe:  
   ```
   <TargetImage condition="is">C:\Windows\System32\lsass.exe</TargetImage>
   ```
   ![Add TargetImage LSASS on Sysmon Event ID 10](images/add-lsass-sysmon.png)  
   Then, navigate to the Sysmon installation directory and run the following command to apply the updated configuration (replace the file name with your actual config file):  
   ```
   Sysmon64.exe -c sysmonconfig-export.xml
   ```
2. **Configure Sysmon Event ID 7**
   To collect Event ID 7 (Image Load), add the following entries inside the `<ImageLoad>` tag in your Sysmon configuration:
   ```
   <Image condition="end with">lsass.exe</Image>
   <ImageLoaded condition="contains">cryptbase.dll</ImageLoaded>
   <ImageLoaded condition="contains">hid.dll</ImageLoaded>
   <ImageLoaded condition="contains">imm32.dll</ImageLoaded>
   <ImageLoaded condition="contains">kernel32.dll</ImageLoaded>
   <ImageLoaded condition="contains">msctf.dll</ImageLoaded>
   <ImageLoaded condition="contains">ntdll.dll</ImageLoaded>
   <ImageLoaded condition="contains">sechost.dll</ImageLoaded>
   <ImageLoaded condition="contains">shell32.dll</ImageLoaded>
   <ImageLoaded condition="contains">user32.dll</ImageLoaded>
   <ImageLoaded condition="contains">wininet.dll</ImageLoaded>
   <ImageLoaded condition="contains">WinSCard.dll</ImageLoaded>
   <ImageLoaded condition="contains">cryptdll.dll</ImageLoaded>
   <ImageLoaded condition="contains">samlib.dll</ImageLoaded>
   <ImageLoaded condition="contains">vaultcli.dll</ImageLoaded>
   ```
   ![Add Common Mimikatz DLLs to Sysmon Event ID 7](images/sysmon-event-id-7.png)
   Finally, update the Sysmon configuration using the appropriate command:
   ```
   Sysmon64.exe -c sysmonconfig-export.xml
   ```
3. **Configure module logging for PowerShell**  
   - Open the **Group Policy Editor** by pressing `Windows+R`, typing `gpedit.msc` and pressing Enter  
   - Select **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows PowerShell**  
   - Double-click **Turn on Module Logging** and set it to **Enabled**
   - In the **Options** section, click the **Show** button
   - Enter `*` or `*=*` as the module name to record all PowerShell modules  
   - Click **OK** on all open windows to apply the changes  
   ![Turn On Module Logging](images/turn-on-module-logging.png)  
4. **Configure script block logging for PowerShell**  
   - Still within the **Windows PowerShell** GPO settings, double-click **Turn on PowerShell Script Block Logging** and set it to **Enabled**  
   ![Turn On Script Block Logging](images/turn-on-script-block-logging.png)  
   - Additionally, to enable **Event ID 4688** (Process Creation), configure **Audit Process Creation**:
     Go to  **Computer Configuration** > **Windows Settings** > **Security Settings** > **Advanced Audit Policy Configuration** > **System Audit Policies** > **Detailed Tracking**  
     Double-click **Audit Process Creation**,  set it to **Enabled**, and check **Configure the following audit events** > **Success**  
    ![Turn on Audit Process Creation](images/turn-on-audit-process-creation.png)
5. **Configure transcription logging**
   - In **Windows PowerShell** GPO settings, enable **Turn on PowerShell Transcription**
   - Set the **Transcript output directory**  to your preferred directory path
   - Enable **Include invocation headers** for more detailed context  
   ![Turn on Transcription Logging](images/turn-on-transcription-logging.png)
6. **Enable monitoring to the Transcription Logging**  
   Navigate to the following path:  
   ```
   C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
   ```
   > If the file doesn't exist yet, create it manually in that directory.
     
   In the `inputs.conf` file of the Splunk Forwarder, add the following configuration:  
   ```
   [monitor://<your transcription folder path>]
   disabled = 0
   index = powershell
   sourcetype = PowerShellTranscript
   recursive = true
   ```
   ![Monitor Transcript](images/monitor-transcript.png)  
     
### References
- [Detect Mimikatz using PowerShell Script Block Logging](https://research.splunk.com/endpoint/8148c29c-c952-11eb-9255-acde48001122/)
- [Detect Mimikatz using Loaded Images](https://research.splunk.com/deprecated/29e307ba-40af-4ab2-91b2-3c6b392bbba0/)
- [Mimikatz Detection using Windows Events](https://github.com/Th1ru-M/Windows-Threat-Hunting/blob/master/Mimikatz%20Detection)
- [Detect Pass the Hash Attack](https://blog.netwrix.com/2021/11/30/how-to-detect-pass-the-hash-attacks/)
- [Windows Security Event ID 4769](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4769)
- [MITRE ATT&CK Pass the Ticket](https://attack.mitre.org/techniques/T1550/003/)
- [Configure PowerShell logging](https://help.splunk.com/en/security-offerings/splunk-user-behavior-analytics/get-data-in/5.4.1/add-other-data-to-splunk-uba/configure-powershell-logging-to-see-powershell-anomalies-in-splunk-uba)  
- [DLL List for Mimikatz](https://www.researchgate.net/figure/DLL-list-for-Mimikatz-Compared-to-Matsuda-et-al-10_tbl3_361991727)
