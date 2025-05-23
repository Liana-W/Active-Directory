# Integrated Active Directory Environment in VMware

<h2>Introduction</h2>

This project demonstrates the creation of an integrated Active Directory (AD) environment using VMware and VirtualBox. A Windows Server 2019 Virtual Machine (VM) was set up as the Domain Controller (DC) with Active Directory service, and a custom PowerShell script was executed to populate AD with approximately 1000 fictional users. Three additional VMs were created: Windows 10 Pro, Ubuntu 22.04.4, and Ubuntu 23.10.1, and integrated into the AD domain to create a centralised management system for user accounts, computers, and other network resources. Initially, difficulties were encountered when attempting to connect an Ubuntu 22.04.4 VM to Active Directory, but a solution was found and implemented. A review of the lab revealed that Ubuntu 23.10.1 resolved the AD domain joining issue, while Ubuntu 22.04.4 still required the workaround solution.

<h2>Project Architecture</h2>

![image](https://github.com/user-attachments/assets/0f5a6478-cc05-4559-93df-7e7ce60699ca)

<h2>Technologies and Components Utilized:</h2>

  - Active Directory
  - AD Domain Service
  - DNS
  - NAT
  - Networking
  - Oracle VirtualBox
  - PowerShell
  - Ubuntu 22.04.4
  - Ubuntu 23.10.1
  - VMware
  - Windows 10 Pro
  - Windows Server 2019

<h2>Windows Server 2019 Setup</h2>

  - Two network adapters were used to separate traffic between external and internal.
![image](https://github.com/user-attachments/assets/eae9ff68-fac5-4cc7-bdf6-f5028d4645f8)
  - The following roles and services were configured.
![image](https://github.com/user-attachments/assets/d7cd4157-445f-4b33-b617-785ddfd9f84b)
  - A PowerShell script was used to generate approximately 1000 fictional users.
![image](https://github.com/user-attachments/assets/d3447ac6-d07a-41f6-8231-284d6dd33901)
![image](https://github.com/user-attachments/assets/0ab1aed6-8edd-45ac-9906-fee094e2653a)
  - After the configurations were completed, I then took time to explore AD by performing tasks such as: creating groups, organisational units, modifying user permissions, disabling users, deleting users, and so on.
![image](https://github.com/user-attachments/assets/cfeb8d7a-7fc1-480e-8c3a-be1df3e9f468)

<h2>Connecting Windows 10 Pro to AD</h2>
  
  - The creation of the Windows VM was straightforward, and I encountered no issues.
  - During the installation, I placed the VM on the internal network and created a local account.
  - I then updated the system's name and joined it to my domain.
    
  ![image](https://github.com/user-attachments/assets/9d522ee6-41d2-45b9-9311-babd6f9128b5)
  
  - From there, I verified the VM was connected to AD by logging into several of the random users I had created.
   ![image](https://github.com/user-attachments/assets/1610053d-d4b1-4444-9cd8-d0940dd50c8b)

<h2>Connecting Ubuntu 22.04.4 to AD</h2>

To expand the lab's scope, I attempted to connect Ubuntu to Active Directory (AD). However, this proved to be the most challenging part of the project. Despite the apparent simplicity of selecting AD during Ubuntu installation, I consistently encountered failure messages. After conducting extensive research online, including YouTube videos, official documentation, and various websites, I discovered that this issue is common and developed a workaround to successfully connect my Ubuntu VM to AD.
![image](https://github.com/user-attachments/assets/3fd3e95c-4d7a-432e-9f90-5eb2733cab11)

<h4>Steps</h4>

  Computer Name: LINUX
  
  Domain Name: mydomain.com
  
  Host Name: LINUX.mydomain.com
  
  Administrator Account: username

  1. Create your Ubuntu VM, update everything, and take a snapshot so you can easily go back if something goes wrong. Network settings will be the same as your Windows 10 Pro VM.
  2. Open up the command line interface.
  3. Verify you can ping the DC and that the DC can ping back.
  4. Set the host name for the machine:
sudo hostnamectl set-hostname LINUX.mydomain.com
  5. Verify the host name:

     hostnamectl
     
  6. Install the following:
     
     sudo apt install sssd-ad sssd-tools realmd adcli
     
  7. Discover the DC:
   
     sudo realm -v discover mydomain.com

  8. Install the following:
   
     sudo apt-get install -y krb5.conf

  9. Edit the krb5.conf file. (Capilization matters, verify default_realm is your DC and add rdns = false)

     sudo nano /etc/krb5.conf
     
   ![image](https://github.com/user-attachments/assets/f93da765-d841-4a8d-97eb-33490bd26eb4)
   
  10. You might have to install this package:

      sudo apt install krb5-user
      
  11. Obtain a Kerberos ticket with an account that has admin privileges:
      
      kinit username
      
  12. Connect the system to the DC:
   
      realm join -v -U username mydomain.com
      
  13. Verify you can see random users in your AD:
      
      id user@mydomain.com
      
![image](https://github.com/user-attachments/assets/e2f23dd4-5b49-4877-8ccc-c88291a1752a)

  14. Now log off your primary account and pick a random user in AD.

      user@mydomain.com
      
![image](https://github.com/user-attachments/assets/0ccea50d-0e94-4820-8efc-1d68b287a7ed)

  15. Finally, once Ubuntu does the initial setup, open the command line interface and verify:
      who
      
![image](https://github.com/user-attachments/assets/215bfbc4-f450-4a46-a0be-b85f6d30f35d)

<h2>Update</h2>

During a review of this lab, I discovered a new release of Ubuntu 23.10.1 and investigated whether the issue of joining an AD domain still existed. The installation was straightforward, and joining an AD domain from the setup was successful. I then tested a newer version of Ubuntu 22.04.4 and found that the AD issue persists, although my workaround still enables connection to the AD domain. Additionally, I utilised VMware to recreate the lab and verify that the issues encountered were not related to VirtualBox.

<h2>Conclusion</h2>

This project created an integrated AD environment using VirtualBox. The first VM, Windows Server 2019, served as the DC, DHCP, and Active Directory, populated with approximately 1000 users. After completing the server's networking configurations, I created three more VMs and connected them to an internal network using Active Directory. While the initial Windows VM setup was straightforward, I encountered difficulties connecting an Ubuntu 22.04.4 VM to Active Directory. After researching and testing, I found a solution that successfully fixed the problem, demonstrating how AD provides centralized management of user accounts, computers, and network resources. Upon review, I also discovered that Ubuntu 23.10.1 resolved the AD domain joining issue, whereas the previous version still requires a workaround.

![image](https://github.com/user-attachments/assets/6d37577e-1a9f-4ff0-a5fd-fab9fd30e487)
![image](https://github.com/user-attachments/assets/2a24e1be-2a8e-4629-980f-f04b70bd46ae)

