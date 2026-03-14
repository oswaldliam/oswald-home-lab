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
 
  ### Project 1 Challenges and Resolutions
  - **Challenge**: Required IP address information in order to assign static addresses
  - **Resolution**: Use of the `ipconfig` command to identify my Default Gateway; researched how to generate a static IPv4 address based off of the Default Gateway address
## Project 2: Initializing Active Directory
- **Status**: In Progress
- **Steps Taken**:
  - [x] Used the Add Roles and Features Wizard to configure and install Active Directory Domain Services to the destination server domainControl
  - [x] Used the Active Directory Domain Services Configuration Wizard to assign the forest root domain name: homelabad.home.arpa
  - [x] Set the Forest functional level and Domain functional level to Windows Server 2025
  - [x] Established Directory Service Restore Mode password
  - [ ] Used default NetBIOS domain name and default Database/Log file/SYSVOL folders
  - [ ] 

### Project 2 Challenges and Resolutions
  - **Challenge**: Required a fully qualified forest root domain name, but I didn't have a publicly registered domain name
  - **Resolution**: Research lead me to [RFC 8375](https://datatracker.ietf.org/doc/html/rfc8375) where in the introduction it explains that .home.arpa is the correct domain for local name service in redidential homenets.
