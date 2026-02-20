## ClickFix-Style Clipboard Execution Detection Lab
### Executive Summary  
This project simulates a ClickFix social engineering attack chain where clipboard content is staged and executed through the Windows Run dialog.

The objective was to design a behavior-based detection capable of identifying:  
- Clipboard manipulation
- Run dialog usage (RunMRU modification)
- Suspicious process execution
- Multi-stage attack correlation

All detections were built using Sysmon and validated in Splunk.  

### Detection Goal
Identify clipboard-based staging followed by user-triggered execution via Run dialog within a short time.  

### Lab Environment
- Windows 10 VM (Victim Machine)
- Sysmon configured with ClipboardChange (Event ID 24)
- Atomic Red Team
- Kali Linux (Attacker Machine)
- Splunk
- AutoHotkey for realistic interaction simulation
- `mshta.exe` for payload execution
- Sliver for C2 Server

### Simulated Attack Chain
1. Malicious command written to clipboard
2. Opens Run dialog (`Win + R`)
3. Clipboard content pasted
4. `mshta.exe executes the malicious payload
5. Gets control of the victim machine

