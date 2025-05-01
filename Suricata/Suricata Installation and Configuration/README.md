## Project Overview
This is a step-by-step guide on installing and deploying Suricata, an open-source network threat detection engine that functions as IDS/IPS (Intrusion Detection System/Intrusion Prevention System), enabling it to detect and block potential threats from entering your systems.

## Documentation
To download Suricata, first access the Suricata official website and head to the installation documentation <br />
You can click on this url to get into the installation page [https://docs.suricata.io/en/latest/install.html](https://docs.suricata.io/en/latest/install.html)
Then, follow these steps to install and deploy Suricata:
1. Follow the installation steps based on your operating system. Since I’m using Ubuntu, I’ll demonstrate the installation process specifically for Ubuntu.  
   ![Installation Page](images/installation-page.png)  
   Begin by installing the required libraries and repositories, such as `software-properties-common` and `ppa:oisf/suricata-stable`. Once added, update your packages and install Suricata.  
   ![Install software-properties-common](images/install-software-properties-common.png)  
   ![Add Repo](images/add-repo.png)  
   ![Install Suricata](images/install-suricata.png)  
2. Enable the Suricata service and check if it’s running properly. You can skip stopping the service at this stage.  
   ![Enable Suricata](images/enable-suricata.png)
3. Go to the Suricata configuration file located at **/etc/suricata/suricata.yaml** and modify the **HOME_NET** variable to match the network subnets you want Suricata to monitor.  
   ![Change Network Subnets](images/net-subnets.png)
4. While editing the configuration file, ensure the correct network interface is specified under the **af-packet** and **pcap** sections so Suricata can capture network traffic. You can check your network interface by typing **ip address** or **ifconfig** command.  
   ![AF Packet Network Interface](images/af-net-interface.png)  
   ![PCAP Network Interface](images/pcap-net-interface.png)  
5. Set the **community-id** value to **true** to correlate events across different security tools.  
   ![Community Flow ID](images/community-id.png)
6. 
   

   
