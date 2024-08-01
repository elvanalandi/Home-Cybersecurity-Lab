## Project Overview
This project provides a step-by-step guide on installing and deploying OPNsense, an open-source firewall that manages incoming and outgoing traffic to protect the network from cyber threats.

## Documentation
To download the OPNsense firewall, first access the OPNsense official website. <br />
You can click on this url to get into the website [https://opnsense.org/](https://opnsense.org/)
Then, follow these steps to install and deploy OPNsense:
1. Navigate to **Download** section on the navigation bar (You can look at the system requirements page on **Users->Get started** to make sure your computer is compatible)<br />
  ![Network Diagram](images/download-page.png)
2. While waiting for the download to complete, you can visit the [bzip2 website](https://gnuwin32.sourceforge.net/packages/bzip2.htm) to download bzip2, which is needed to uncompress the OPNsense file. To download bzip2, click the **Setup** link next to the **Complete  package** description, highlighted in purple.<br />
  Alternatively, you can use WinRAR to uncompress the file without needing bzip2. If you are using WinRAR application, you can skip to step 8.<br />
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
10. 

   


