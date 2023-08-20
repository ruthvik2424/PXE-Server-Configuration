# PXE-Server-Configuration
## Configuring a PXE Server in VMware Workstation On Ubuntu VM for Network Booting and OS Installation
## Overview
**This project guides you through the process of setting up a PXE (Preboot Execution Environment) server within an Ubuntu virtual machine hosted on VMware. A PXE server is a powerful tool that simplifies the deployment and installation of operating systems across a network. With this setup, you can perform network-based OS installations on multiple virtual machines within your VMware environment, eliminating the need for physical installation media such as DVDs or USB drives. This project specifically focuses on configuring a PXE server for booting and installing CentOS 8, a popular Linux distribution, on your VMware-based virtual machines.**

**By the end of this project, you'll have a fully functional PXE server ready to deploy CentOS 8 across vm network. You can Try to Deploy other OS Using this Setup. This setup can serve as a foundation for various IT tasks, including system maintenance, software distribution, and automated provisioning of new systems. One's Successful you can try it on actual server in real world environments**

Clone the Repository First 


## Step-by-Step Configuration
Step 1: Firewall Setup
Disable and stop the firewall to ensure it doesn't interfere with network booting.
```bash
sudo systemctl stop ufw
sudo systemctl disable ufw
```
Step 2: Install Required Software
Install the necessary software packages (dnsmasq, vsftpd, httpd, syslinux).
```bash
sudo apt install -y dnsmasq vsftpd apache2 syslinux
```
Step 3: Backup dnsmasq Configuration
Create a backup of the dnsmasq configuration file.
```bash
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.bck
```

Step 4: Check The Interface and the IP Address and Edit dnsmasq Configuration
```bash
# Open the dnsmasq configuration file for editing. Customize it to your network settings.
ip a
sudo nano /etc/dnsmasq.conf
# Sample file is provided in the repository
# Edit the variable According to Your Network Configuration  
```
Step 5: Create Folders
Create essential directories for the PXE server.
```bash
sudo mkdir -p /netboot/tftp/pxelinux.cfg
sudo mkdir /netboot/www
```
Step 6: Copy Syslinux Files
Copy Syslinux bootloader files to the tftp directory.
```bash
sudo cp /usr/share/syslinux/{menu.c32,ldlinux.c32,libutil.c32} /netboot/tftp
# pxelinux.cfg  file is provided in Repository Copy that file also. 
```
Step 7: Configure Apache Web Server
Create a symbolic link to your Apache web server's document root.
```bash
sudo ln -s  /netboot/www /var/www/html/
```
Step 8: Start and Enable Apache2
Start and enable the Apache2 web server.
```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```
Step 9: Create OS Directory
Create directories for the OS you want to install (e.g., CentOS 8).
```bash
sudo mkdir /netboot/{tftp,www}/centos8
```
Step 10: Install rsync
If not already installed, install rsync.
```bash
sudo apt install -y rsync
```
Step 11: Copy OS Files
Use rsync to copy the OS iso files to the web server directory.
If you download the iso on your main OS (Windows) you can copy the content by using the SCP.   
```bash
# first mount the iso and then copy its content to another folder eg:(Centos8)
# then running the command to copy it inside your pxe-server ubuntu vm
# scp C:\Users\Admin\Desktop\Centos8 user@ipaddress:/home/user/centos8
# ones you got the location of the files you can modify the command accordingly
sudo rsync -avz /home/user/centos8 /netboot/www/centos8
```
Step 12: Copy Kernel and Initrd Files
Copy the kernel and initrd files to the tftp directory.
```bash
sudo cp /netboot/www/centos8/images/pxeboot/{initrd.img,vmlinuz} /netboot/tftp/centos8/
```
Step 13: Edit PXE Configuration (pxelinux.cfg/default)
Customize the PXE boot menu configuration for your OS.
```bash
# This sample file is also provided in the repository
sudo nano /netboot/tftp/pxelinux.cfg/default
```
Step 14: Restart Services
Restart the HTTP and dnsmasq services to apply changes.
```bash
sudo systemctl restart apache2
sudo systemctl restart dnsmasq
```
Step 15: Test The Configuration 
Create Another Vm Without Providing any ISO File




Congratulations! You've successfully configured a PXE server for network booting and OS installations. This setup is not only a valuable tool for IT professionals but also a fundamental concept to understand in the world of network-based system provisioning.

By mastering the PXE server configuration, you've opened the door to a wide range of possibilities, including:

- Rapid and automated OS installations on multiple machines.
- Streamlined system maintenance and updates.
- Scalable and efficient management of IT infrastructure.
- Reduced reliance on physical installation media.

The knowledge gained from this project serves as a strong foundation for various IT tasks, making it an essential skill in today's dynamic technology landscape. As you continue to explore and experiment with PXE, you'll unlock even more capabilities for network-based system administration.

Feel free to explore further, customize your PXE server setup to suit specific needs, and continue to expand your knowledge in this fascinating field.
