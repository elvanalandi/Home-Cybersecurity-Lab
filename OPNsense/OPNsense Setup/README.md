## Project Overview
This project provides a step-by-step guide on installing and deploying OPNsense, an open-source firewall that manages incoming and outgoing traffic to protect the network from cyber threats.

## Documentation
To download the OPNsense firewall, first access the OPNsense official website. <br />
You can click on this url to get into the website [https://opnsense.org/](https://opnsense.org/)
Then, follow these steps to install and deploy OPNsense:
1. Navigate to **Download** section on the navigation bar (You can look at the system requirements page on **Users->Get started** to make sure your computer is compatible)<br />
  ![Network Diagram](images/download-page.png)
2. While waiting for the download to complete, you can visit the [bzip2 website](https://gnuwin32.sourceforge.net/packages/bzip2.htm) to download bzip2, which is needed to uncompress the OPNsense file. Alternatively, you can use WinRAR to uncompress the file without needing bzip2. Please note that this bzip2 can only be used in the Windows platform.<br />
  ![Bzip2](images/bzip2.png)
3. It is a good practice to compare the file hash with the hash from the official website to avoid file corruption and file tampering. In Windows, we can use **Get-FileHash** command. After getting the hash value, you can compare it manually with the hash on the website.<br />
  ![Get File Hash](images/hashes.png)

   
   


