# Liam Oswald's Home Lab Documentation 

## Goal:
Establish a professional server environment for personal learning. 

## Project 1: Initializing Hypervisor and Operating System
- **Hypervisor**: VMware Workstation Pro 25H2u1 (Type-2)
- **Status**: Complete
- **Steps Taken**:
  - [x] Downloaded and installed VMware Workstation Pro 25H2u1
  - [x] Downloaded Windows Server 2025 ISO
  - [x] Initialized the Windows Server 2025 on VMWare Workstation
   <img width="346" height="265" alt="image" src="https://github.com/user-attachments/assets/8fd4ade5-ef24-449f-9be7-b551dc0c60eb" />
   
  - [x] VMware tools downloaded on the VM
  - [x] Assigned a static IPv4 address and identified the Default Gateway address to start the Domain Controller setup. (Assigned a public DNS server address for now.) Used the  `ipconfig` command in Windows PowerShell to identify addresses and generate a static address. 
  - [x] Used Settings to check for and update Windows
  - [x] Assigned a custom computer name: domainControl
  - [x] Used Computer Managment > Local Users and Groups and set the Administrator password  
 
  ### Project 1 Challenges and Resolutions
  - **Challenge**: Required IP address information in order to assign static addresses
  - **Resolution**: Use of the `ipconfig` command to identify my Default Gateway; researched how to generate a static IPv4 address based off of the Default Gateway address
## Project 2: Initializing Active Directory
- **Status**: Complete
- **Steps Taken**:
  - [x] Used the Add Roles and Features Wizard to configure and install Active Directory Domain Services to the destination server domainControl
  - [x] Used the Active Directory Domain Services Configuration Wizard to assign the forest root domain name: homelabad.lab.local
  - [x] Set the Forest functional level and Domain functional level to Windows Server 2025
  - [x] Established Directory Service Restore Mode password
  - [x] Used default NetBIOS domain name and default Database/Log file/SYSVOL folders
  - [x] Passed prerequisite checks and proceeded with automatic restart
  - [x] Navigated to Active Directory Users and Computers to confirm
  <img width="751" height="293" alt="Screenshot 2026-03-13 214021" src="https://github.com/user-attachments/assets/e0fdb57f-56e0-4987-a602-fb62dd1e52a3" />
  
  - [x] Generated OUs for a tiered OU structure in Active Directory Users and Computers
  <img width="618" height="309" alt="image" src="https://github.com/user-attachments/assets/075feb67-4a08-463e-bb80-717b7e02910e" />

### Project 2 Challenges and Resolutions
  - **Challenge**: Required a fully qualified forest root domain name, but I didn't have a publicly registered domain name
  - **Resolution**: Research lead to using .lab.local; attempted using .home.arpa but this lead to dynamic registrations being generated in the reverse lookup zone rather than the forward.

## Project 3: DNS Initial Configuration 
- **Status**: In Progress
- **Steps Taken**:
  - [x] Used the DNS Manager to add a public server as a forwarder (8.8.8.8 dns.google)
  - [x] Created a IPv4 Reverse Lookup Zone and manually added a PTR 
  - [x] Used the DC's IPv4 address in `nslookup` command to confirm
  - [x] Used the DNS Manager to enable scavenging of stale records (7 days)

 ### Project 3 Challenges and Resolutions
  - **Challenge**: Initial creation of the IPv4 Reverse Lookup Zone did not return with `nslookup`
  - **Resolution**: Manually added a PTR 
        
