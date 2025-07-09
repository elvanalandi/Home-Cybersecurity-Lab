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
   Custom functions are used in the playbook to support email analysis and IOC extraction.
   Go to the **Custom Functions** menu and then click on the **+ Custom Function** button.
   ![Create Custom Functions](images/create-custom-functions.png)  
   
   
