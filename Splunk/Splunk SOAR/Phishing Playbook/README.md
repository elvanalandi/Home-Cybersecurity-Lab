## Phishing Playbook
### Table of Contents
- [Overview](#overview)  
- [Requirements](#requirements)  
- [Phishing Playbook Implementation Walkthrough](#phishing-playbook-implementation-walkthrough)
### Overview
This project demonstrates how to develop a phishing playbook in Splunk SOAR to automate phishing response and escalation.  

### Requirements
- A running and accessible instance of **Splunk SOAR**
- A Gmail account for the analyst
- A Gmail account to be monitored

### Phishing Playbook Implementation Walkthrough
1. **Create Playbook**  
   Navigate to the SOAR **Playbooks** dashboard and click the **+ Playbook** button on the right-hand side.   
   ![Playbooks Dashboard](images/playbook-dashboard.png)  
   Select **Automation** as the **Playbook Type**  
   ![Playbook Type](images/playbook-type.png)  
2. **IMAP Block**  
   Begin by adding an **Action** block and selecting the **get email** function from **IMAP**.  
   ![IMAP Block](images/imap-block.png)  
   Rename the block to **List Emails** and edit the code using the Python editor on the right-hand side.  
   Specify the following parameters:
   ```
   "folder": "inbox",
   "container_id": container.get("id"),
   "search_filter": "UNSEEN",
   "ingest_email": True
   ```
   ![List Emails Parameters](images/list-emails.png)  
3. **Create Custom Functions**  
   Custom functions are used in the playbook to support email analysis and IOC (Indicators of Compromise) extraction.  
   Go to the **Custom Functions** menu and click on the **+ Custom Function** button.  
   ![Create Custom Functions](images/create-custom-functions.png)
   In this project, I will create two custom functions — one for extracting IOCs from the email, and another for performing a full analysis of the email.  
   The **extract_email_iocs** function combines the email header and body, and uses regex to extract patterns such as URLs, IP addresses, and email addresses.  
   [Extract Email IOCs Custom Function](extract_email_iocs.txt)  
   The **vt_abuse_ipdb_email_analysis** function analyses the email using data from the headers, email body, VirusTotal results, and AbuseIPDB scores.
   - It extracts the sender's email address from the `From` header.
   - It checks for suspicious keywords in the email body.
   - It reviews VirusTotal URL analysis results and generates a summary message if any are flagged as malicious.
   - It analyses any attached files using VirusTotal file reputation and summarizes the results.
   - It evaluates AbuseIPDB results, flagging IPs with an abuseConfidenceScore >= 50.
   - Finally, it builds a verdict(`malicious` or `benign`) and a reason based on all the findings.  
     
   [Email Analysis Custom Function](vt_abuseipdb_email_analysis.txt)  
4. **Extract Email IOCs Utility Block**  
   Next, add a Utility Block using the **extract_email_iocs** function.  Map the headers to the **List Email** data and the body to the **CEF artifact** `bodyPart1` field.  
   ![Extract Email IOCs Function](images/extract_email_iocs_function.png)  
5. **IP Decision Block**   
   This Decision Block checks whether the email contains any IP addresses to analyse. If the result from the extraction is **not empty**, the playbook proceeds with IP analysis using AbuseIPDB.  
   ![IP Decision Block](images/ip-decision.png)  
6. **Check IPs in AbuseIPDB Action Block**  
   Add an Action Block using the **lookup ip** action from the **AbuseIPDB** app. I added some custom code to support handling both single IPs and lists of IPs.  
   ![Check IP in AbuseIPDB Action Block](images/abuseipdb-block.png)  
7. **URL Decision Block**
   This block checks if at least one URL was detected in the email. If a URL is found, it proceeds to URL analysis. I unchecked the **Join settings** on the `check_ips_in_abuseipdb` block so that the playbook continues even if no IPs are detected.  
   ![URL Decision Block](images/url-decision.png)  
