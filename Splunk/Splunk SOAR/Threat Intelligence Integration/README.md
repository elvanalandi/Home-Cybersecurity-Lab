## Threat Intelligence Integration 
### Table of Contents
- [Overview](#overview)  
- [Requirements](#requirements)  
- [VirusTotal Integration Steps](#virustotal-integration-steps)
- [AbuseIPDB Integration Steps](#abuseipdb-integration-steps) 

### Overview
This project demonstrates how to integrate Threat Intelligence platform like VirusTotal and AbuseIPDB with Splunk SOAR  enrich Indicators of Compromise (IOCs) and enhance automated threat analysis and response capabilities.  

### Requirements
- A running and accessible instance of **Splunk SOAR**

### VirusTotal Integration Steps
1. **Navigate to the [VirusTotal App on Splunkbase](https://splunkbase.splunk.com/app/5865)**  
   Go to the Splunkbase website and search for *VirusTotal App*.  
   ![VirusTotal App from Splunk Website](images/virustotal-app.png)  
2. **Download the App and Verify Its Integrity**  
   After downloading the file, copy the verification command provided on the Splunkbase page to validate the file's integrity.  
   - If you're using **Linux**, use the SHA256 command provided.  
   ![VirusTotal SHA256](images/virustotal-sha256.png)  
   - If you're using **Windows**, run the `Get-FileHash` command in PowerShell and compare the resulting hash with the one shown on the Splunkbase.  
   ![Get File Hash](images/get-filehash.png)
3. **Access the Apps Dashboard**  
   In the Splunk SOAR interface, navigate to the top-left menu and select **Automation** > **Apps**. Then, click **Install App** from the top-right corner.  
   ![Apps Dashboard](images/apps.png)  
4. **Upload the Downloaded App File**  
   Drag and drop the downloaded file into the upload area. Then, proceed with the installation.  
   ![Install App](images/install-app.png)  
5. **Retrieve Your VirusTotal API Key**  
   Visit the [VirusTotal website](https://www.virustotal.com/), and log in to your account. If you don't have one yet, sign up first for free.  
   Once logged in, go to your **Account** menu, select **API Key**. Copy the key and store it in a temporary location (you'll need it later).  
   ![VirusTotal Website](images/virustotal-web.png)  
   ![VirusTotal API Key](images/virustotal-api-key.png)  
6. **Return to the Apps Dashboard in Splunk SOAR**
   You should now see that the VirusTotal app has been successfully installed.  
   To establish a connection between **Splunk SOAR** and **VirusTotal**, click **Configure New Asset**.  
   ![VirusTotal App on Splunk SOAR](images/virustotal-app-soar.png)  
7. **VirusTotal Configuration**  
   
### AbuseIPDB Integration Steps
