# Liam Oswald's Home Lab Documentation 

## Goal:
Establish a professional server environment for personal learning. 

## Project 1: Initializing Hypervisor and Operating System
- **Hypervisor**: VMware Workstation Pro 25H2u1 (Type-2)
- **Status**: Complete
- **Steps Taken**:
  - [x] Established Windows Server 2025 on VMware Workstation Pro 
   <img width="346" height="265" alt="image" src="https://github.com/user-attachments/assets/8fd4ade5-ef24-449f-9be7-b551dc0c60eb" />
   
  - [x] Manually assigned static IPv4 address and custom computer name (domainControl)
  - [x] OS hardened with Windows updates and assigning administrator's password
 
  ### Project 1 Challenges and Resolutions
  - **Challenge**: Required IP address information in order to assign static addresses
  - **Resolution**: Use of the `ipconfig` command to identify my Default Gateway; researched how to generate a static IPv4 address based off of the Default Gateway address
## Project 2: Initializing Active Directory
- **Status**: Complete
- **Steps Taken**:
  - [x] Established the forest root domain (homelabad.lab.local) 
  - [x] Established fundamental domain services; Forest functional level and Domain functional level to Windows Server 2025 and set the Directory Service Restore Mode password
  - [x] Defaulted on NetBIOS domain name and default Database/Log file/SYSVOL folders
  - [x] Verified with Active Directory Users and Computers
  <img width="751" height="293" alt="Screenshot 2026-03-13 214021" src="https://github.com/user-attachments/assets/e0fdb57f-56e0-4987-a602-fb62dd1e52a3" />
  
  - [x] Generated OUs for a tiered OU structure in Active Directory Users and Computers to establish different administration levels and organize GPOs
  <img width="618" height="309" alt="image" src="https://github.com/user-attachments/assets/075feb67-4a08-463e-bb80-717b7e02910e" />

### Project 2 Challenges and Resolutions
  - **Challenge**: Required a fully qualified forest root domain name, but I didn't have a publicly registered domain name
  - **Resolution**: Research lead to using .lab.local; attempted using .home.arpa but this lead to dynamic registrations being generated in the reverse lookup zone rather than the forward.

## Project 3: DNS Initial Configuration 
- **Status**: In Progress
- **Steps Taken**:
  - [x] Configured DNS Forwarder for more reliable external name resolution (8.8.8.8 dns.google)
  - [x] Created a IPv4 Reverse Lookup Zone for future utility in diagnostics and security 
  - [x] Verified using the DC's IPv4 address and domain name in `nslookup` command to confirm forward and reverse resolution 
  - [x] Enabled DNS scavenging of stale records (7 days) to keep the home lab lightweight and reduce possible conflicts
  - [x] Hardened the DNS server by restricting the listener to the DC's static IPv4 address

 ### Project 3 Challenges and Resolutions
  - **Challenge**: Initial creation of the IPv4 Reverse Lookup Zone did not return with `nslookup`
  - **Resolution**: Identified the reverse lookup zone did not have a PTR so one was manually added; verified resolution in both directions with `nslookup`
        
