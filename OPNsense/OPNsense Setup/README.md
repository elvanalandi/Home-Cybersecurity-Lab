## Project Overview
This project provides a step-by-step guide on installing and deploying OPNsense, an open-source firewall that manages incoming and outgoing traffic to protect the network from cyber threats.

## Documentation
To download the OPNsense firewall, first access the OPNsense official website. <br />
You can click on this url to get into the website [https://opnsense.org/](https://opnsense.org/)
Then, follow these steps to install and deploy OPNsense:
1. Navigate to **Download** section on the navigation bar (You can look at the system requirements page on **Users -> Get started** to make sure your computer is compatible)<br />
  ![Network Diagram](images/download-page.png)
2. While waiting for the download to complete, you can visit the [bzip2 website](https://gnuwin32.sourceforge.net/packages/bzip2.htm) to download bzip2, which is needed to uncompress the OPNsense file. To download bzip2, click the **Setup** link next to the **Complete  package** description, highlighted in purple.<br />
  Alternatively, you can use WinRAR to uncompress the file without needing bzip2.<br />
  If you are using WinRAR application, you can skip to step 8.<br />
  Please note that this bzip2 can only be used in the Windows platform.<br />
  ![Bzip2](images/bzip2.png)
3. It’s good practice to compare the file hash with the hash provided on the official website to ensure the file hasn’t been corrupted or tampered with. On Windows, you can use the `Get-FileHash` command to generate the file hash. After obtaining the hash value, compare it manually with the hash listed on the website.<br />
   ![Get File Hash](images/hashes.png)
4. Open the bzip2 setup wizard to begin the installation. Select **I accept the agreement**, then click the **Next** button..
   ![Bzip2 License Agreement](images/bzip2-license.png)
5. Choose your preferred installation folder, or use the default folder as I did.
   ![Bzip2 Installation Folder](images/bzip2-folder.png)
6. Select **Full installation** and click **Next**. Then, choose the Start Menu folder where you want the program’s shortcuts to be placed.
   ![Bzip2 Components](images/bzip2-components.png)
   ![Bzip2 Start Menu Folder](images/bzip2-startmenu.png)
7. Next, follow the prompts until the installation is complete. Then, open **Windows Powershell** and navigate to the **bin** folder of the bzip2 installation. Run the application with the **-d** flag followed by the OPNsense downloaded file `.\bunzip2.exe -d <file path>`.
   ![Bzip2 Command](images/bzip2-command.png)
8. The uncompressed file will appear as follows:<br />
   ![ISO file](images/opnsense-iso.png)
9. Next, create the OPNsense virtual machine using the ISO image. I am using Oracle VirtualBox for this installation, but you can use any virtual machine application of your choice.<br />
   After clicking the **Create** button, specify the name, folder, and path to the ISO image. Set the VM type to **BSD** and select **FreeBSD (64-bit)** as the version, as OPNsense is based on FreeBSD.
   ![VM Settings](images/vm-settings.png)
10. Allocate a minimum of 2048 MB of memory and 2 CPUs to ensure optimal performance.
   ![Hardware Specification](images/hardware-specs.png)
11. Create a Virtual Hard Disk with a minimum size of 8 GB.
   ![Virtual Hard Disk](images/vharddisk.png)
12. Review the settings and click the **Finish** button.
   ![VM Summary](images/vm-summary.png)
13. Before starting the OPNsense virtual machine, configure the network settings. I am using two types of networks: **NAT** and **Host-only Adapter**. This setup allows the firewall to capture traffic from both the internet and the internal network, including the host.<br /><br />
    In VirtualBox, access the network settings by going to **Settings** and then the **Network** tab. Ensure **Enable Network Adapter** is checked. For the first adapter, select **NAT**, and for the second adapter, choose **Host-only Adapter**.
   ![NAT Network Settings](images/NAT.png)
   ![Host Network Settings](images/host.png)
14. Start the OPNsense virtual machine and wait for it to fully boot up. Once it’s ready, provide the login credentials:<br />
   **login: installer**<br />
   **Password: opnsense**<br />
   ![VM Login](images/vm-login.png)
15. Select **Continue with default keymap**, then choose **Install (UFS)** and select **ada0** as the hard disk for the file system. <br />
    Finally, reset the contents of the disk. <br />
   ![Keymap Selection](images/keymap.png)
   ![UFS](images/ufs.png)
   ![Hard Disk](images/harddisk.png)
   ![Reset Disk](images/reset-disk.png)
16. Wait for the installation to complete. You can then either change the root password or finish the installation. For enhanced security, it is recommended to change the default root password.
   ![Installing Process](images/installing.png)
   ![Complete Installation Prompt](images/complete-installation.png)
17. While the virtual machine is rebooting, remove the ISO disk from the virtual drive by going to **Devices** -> **Optical Drives** -> **Remove disk from virtual drive**.<br />
   In the image, this option is highlighted in grey because the disk was removed before taking the screenshot.
   ![Rempve Disk from Virtual Drive](images/remove-disk.png)
18. Then, log in with the root account. You will see various options for configuring the firewall.<br /><br />
   First, select option 1 to assign the interfaces. Choose **No/n** for LAGGs (Link Aggregation Groups, similar to EtherChannel in Cisco for aggregating network links) and VLANs configuration.<br />
   ![Network Interfaces Settings 1](images/net-interfaces-1.png) <br />
   For the WAN and LAN interfaces, assign **em0** to the WAN interface and **em1** to the LAN interface. Press Enter for the Optional interface to continue without providing additional input.<br />
   ![Network Interfaces Settings 2](images/net-interfaces-2.png)
   ![Network Interfaces Settings 3](images/net-interfaces-3.png) <br />
   Then, proceed to set up the interfaces.<br />
   ![Network Interfaces Settings 4](images/net-interfaces-4.png)
19. After assigning the interfaces, assign IP addresses to the LAN interface by choosing option 2.
   ![Set Interface IP Address Mode](images/set-ip.png) <br />
    I am using a static LAN IP address to ensure consistent access to OPNsense without changes over time.<br />
   The IP address used is within my subnet, but you should select an IP address that fits your own network.<br />
   ![Set IPv4 Address](images/ipv4-addr.png)<br />
   We are not configuring the WAN interface because the firewall will be used within a home network, and IPv6 is not in use.<br />
   The Web GUI protocol does not affect firewall access, even if you use HTTPS, it will default to HTTP due to the certificate not being issued by a certificate authority.<br />
   ![Set IPv6 Address and Protocol](images/ipv6-proto.png)<br />
20. If everything has been set, we can access the OPNsense GUI in the web browser and type the static IP address that already set.<br />
   Then, we can use the root account to login to the website.
   ![OPNsense Login](images/login-page.png)<br />
   The first step is to check for updates by navigating to **Firmware -> Updates**. Next, go to the **Plugins** section, located next to **Updates**, to download the VirtualBox plugins, which include the Guest Additions extension to ensure OPNsense runs smoothly.
   ![OPNsense Update](images/update-page.png)<br />
   ![OPNsense Download Plugins](images/download-plugins.png)<br />
21. Finally, the firewall will be up and running when you return to the **Lobby -> Dashboard**. You can add widgets to suit your preferences.
   ![OPNsense Dashboard](images/dashboard.png)
