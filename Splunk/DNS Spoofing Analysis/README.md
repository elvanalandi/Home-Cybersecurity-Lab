## DNS Spoofing Analysis Using Splunk Enterprise
### Overview
This project demonstrate how to analyse DNS spoofing attack data using Splunk. After simulating a DNS spoofing attack and capturing network traffic with Wireshark, the data is converted to CSV and imported into Splunk for analysis. While it's not an in-depth analysis, it helps identify key patterns and behaviors typical of a DNS spoofing attack.  

### Requirements
- Splunk installed and running locally.
- Wireshark to capture network traffic.
- Pcap file converted to CSV format for analysis.

### Analysis Steps
1. Use the **Add Data** option in Splunk and select the CSV file containing the captured network data.  
   ![Import CSV](images/import.png)
2. Ensure that the data from the CSV file is correctly recognised by Splunk. Then, click **Save As** to set the Name, Description, Category, and App. For this project, we will be using the **Search & Reporting** App.  
   ![Source Type](images/source.png)
3. Provide a name and description for the data, and select the appropriate Category and App.
   ![Source Type Details](images/source-detail.png)
4. You can modify the host field if necessary, but it's optional. Once everything is set, proceed to the **Review** step and then click **Done**.  
   ![Input Settings](images/input.png)
5. Once the setup is complete, click on the **Start Searching** button to begin your analysis in Splunk.
   ![Start Analysing](images/start-analyse.png)
6. Now, we can see many events being logged, and we can filter them using the fields on the left sidebar. To make our analysis more focused, we can add more fields to filter the events more specifically. For example, in the picture, we’re only seeing events related to ARP protocol, but by adding more fields, we can include DNS traffic, IP addresses, or other important details. This helps us identify patterns linked to the DNS spoofing attack more easily.  
   ![Search Page](images/search-page.png)
7. Select one event to use as a sample.  
   ![Raw Events](images/raw-events.png)  
   ![Select Sample Event](images/sample-event.png)  
8. Choose "Regular Expression" as the selection method.  
   ![Selection Method](images/method.png)  
9. Assign tags to the chosen fields. In the example below, I used four tags: **src_ip** (source IP), **dst_ip** (destination IP), **protocol**, and **fqdn** (fully qualified domain name as the domain).  
   ![New Fields](images/new-fields.png)  
10. Save the extracted fields to filter the data more effectively.  
   ![Save Extracted Fields](images/extracted-fields.png)  
