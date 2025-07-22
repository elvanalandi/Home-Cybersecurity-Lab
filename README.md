## Home Cybersecurity Lab
  
This is a collection of projects I’ve built to learn how to protect my home network. I’ve tried out different tools like firewalls, security monitoring systems, and threat detection platforms. Each project includes easy-to-follow steps showing what I did and what I learned along the way.  
  
<h3>🔥 OPNsense Firewall</h3>  
<p>Set up and configured OPNsense, a powerful open-source firewall, to protect my home network from unauthorized access and malicious traffic.</p>
<ul>
  <li>
    <a href="/OPNsense/OPNsense%20Setup"><strong>OPNsense Firewall Installation</strong></a>: Step-by-step configuration to establish a secure perimeter.
  </li>
</ul>
  
<h3>🛡️ Wazuh SIEM</h3>
<p>A range of hands-on projects using Wazuh to monitor and protect systems in a home lab environment.</p>
<ul>
  <li>
    <a href="/Wazuh/Wazuh%20SSH%20Brute%20Force%20Attack%20Protection"><strong>SSH Brute Force Attack Protection</strong></a>: Set up Wazuh to detect and automatically block brute force attempts.
  </li>
  <li>
    <a href="/Wazuh/Wazuh%20Integration%20with%20TheHive"><strong>Integration with TheHive</strong></a>: Connected Wazuh with TheHive to enhance incident response.
  </li>
  <li>
    <a href="/Wazuh/Wazuh%20Integration%20with%20Suricata"><strong>Integration with Suricata</strong></a>: Linked Wazuh with Suricata IDS for better threat detection and correlation.
  </li>
</ul>

<h3>🧠 Incident Response Platform (TheHive, Cortex, MISP)</h3>
<p>Built a complete open-source incident response stack to analyze and manage threats.</p>
<ul>
  <li>
    <a href="/TheHive-Cortex-MISP/TheHive-Cortex-MISP%20Setup%20and%20Integration"><strong>Setup and Integration</strong></a>: Installed and configured TheHive, Cortex, and MISP to streamline threat intel and automate response workflows.
  </li>
</ul>

<h3>📊 Splunk</h3>
<p>Integrated <strong>Windows Logs</strong> into Splunk for threat detection</p>
<ul>
  <li>
    <a href="/Splunk/Windows%20Logs%20Ingestion"><strong>Windows Logs Ingestion</strong></a>: Collected and ingested Windows Event Logs into Splunk Enterprise using Universal Forwarder for future threat hunting and analysis.
  </li>
</ul>
<p>Used <strong>Splunk Enterprise</strong> for threat analysis and visualization.</p>
<ul>
  <li>
    <a href="/Splunk/DNS%20Spoofing%20Analysis"><strong>DNS Spoofing Analysis</strong></a>: Investigated a simulated DNS spoofing attack using Splunk dashboards and queries.
  </li>
  <li>
    <a href="/Splunk/Mimikatz%20Detection"><strong>Mimikatz Detection</strong></a>: Mimikatz detection using Sysmon and Windows PowerShell events in Splunk, including dashboard creation and queries.
  </li>
</ul>
<p>Installed and integrated <strong>Splunk</strong> with <strong>Splunk SOAR</strong> to enable automated cybersecurity incident detection and response.</p>
<ul>
  <li>
    <a href="/Splunk/Splunk%20SOAR/Splunk%20SOAR%20Installation"><strong>Splunk SOAR Installation</strong></a>: Deployed Splunk SOAR in a virtual machine environment.
  </li>
</ul>
<ul>
  <li>
    <a href="/Splunk/Splunk%20SOAR/Splunk%20SOAR%20Integration"><strong>Splunk & Splunk SOAR Integration</strong></a>: Connected Splunk to Splunk SOAR to forward security alerts and trigger automated playbooks.
  </li>
</ul>
<ul>
  <li>
    <a href="/Splunk/Splunk%20SOAR/Threat%20Intelligence%20Integration"><strong>Threat Intelligence Integration</strong></a>: Integrated <strong>VirusTotal</strong> and <strong>AbuseIPDB</strong> APIs within Splunk SOAR to enrich IOCs with real-time reputation data.
  </li>
</ul>
<ul>
  <li>
    <a href="/Splunk/Splunk%20SOAR/Email%20Ingestion"><strong>Email Ingestion</strong></a>: Configured <strong>IMAP</strong> to retrieve emails from monitored inboxes and <strong>SMTP</strong> to enable email-based alerts and analyst notifications.
  </li>
</ul>
<ul>
  <li>
    <a href="/Splunk/Splunk%20SOAR/Phishing%20Playbook"><strong>Phishing Automated Playbook</strong></a>: Developed a playbook to automate phishing response, including IOC extraction, enrichment, reputation checks, and escalation.
  </li>
</ul>

<h3>🌐 Suricata IDS/IPS</h3>
<p>Deployed Suricata to detect suspicious activity in network traffic.</p>
<ul>
  <li>
    <a href="/Suricata/Suricata%20Installation%20and%20Configuration"><strong>Installation and Configuration</strong></a>: Set up Suricata with rule tuning and log forwarding for better visibility and control.
  </li>
</ul>  

<h3>🛡️ Active Directory Monitoring with Grafana</h3>
<p>Visualize and monitor Active Directory performance and system health by integrating Windows Exporter, Prometheus, and Grafana.</p>
<ul>
  <li>
    <a href="/Grafana/Tools%20Installation%20and%20Configuration"><strong>Installation of Windows Exporter, Prometheus, and Grafana</strong></a>: Installed Windows Exporter (formerly WMI Exporter) on domain controllers to expose key AD metrics such as CPU usage and system uptime. Configured Prometheus to collect these metrics at regular intervals and set up Grafana dashboards for real-time visibility and alerting.
  </li>
  <li>
    <a href="/Grafana/Setting%20Up%20Prometheus%20Data%20Source"><strong>Setting Up Prometheus Data Source in Grafana</strong></a>: Connected Prometheus to Grafana by configuring it as a data source.
  </li>
  <li>
    <a href="/Grafana/Creating%20a%20Dashboard%20for%20AD%20Metrics"><strong>Creating a Dashboard for AD Metrics in Grafana</strong></a>: Built a custom Grafana dashboard using Prometheus data to visualize Active Directory performance metrics.
  </li>
  <li>
    <a href="/Grafana/Setting%20Up%20Alerts%20for%20AD%20Issues"><strong>Setting Up Alerts for AD Issues in Grafana</strong></a>: Configured alert rules in Grafana to monitor Active Directory performance and availability using Prometheus data, enabling prompt notification of issues.
  </li>
</ul>
