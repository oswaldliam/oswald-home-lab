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
  - [x] Manually assigned the preferred DNS address to match the static IPv4 address to ensure smooth Active Directory promotion 
  - [x] OS hardened with Windows updates and assigning the administrator's password
 
  ### Project 1 Challenges and Resolutions
  - **Challenge**: Required IP address information in order to assign static addresses
  - **Resolution**: Use of the `ipconfig` command to identify my Default Gateway; researched how to generate a static IPv4 address based off of the Default Gateway address
    
## Project 2: Initializing Active Directory
- **Status**: Complete
- **Steps Taken**:
  - [x] Used Add Roles and Features Wizard to install Active Directory Domain Services 
  - [x] Established the forest root domain (homelabad.lab.local) 
  - [x] Established fundamental domain services; Forest functional level and Domain functional level to Windows Server 2025 and set the Directory Service Restore Mode password
  - [x] Defaulted on NetBIOS domain name and default Database/Log file/SYSVOL folders
  - [x] Verified with Active Directory Users and Computers
  <img width="751" height="293" alt="Screenshot 2026-03-13 214021" src="https://github.com/user-attachments/assets/e0fdb57f-56e0-4987-a602-fb62dd1e52a3" />
  
  - [x] Generated OUs for a tiered OU structure in Active Directory Users and Computers to establish different administration levels and organize GPOs
  <img width="618" height="309" alt="image" src="https://github.com/user-attachments/assets/075feb67-4a08-463e-bb80-717b7e02910e" />

  ### Project 2 Challenges and Resolutions
  - **Challenge**: Required a fully qualified forest root domain name, but I didn't have a publicly registered domain name
  - **Resolution**: Research led to using .lab.local; attempted using .home.arpa but this lead to dynamic registrations being generated in the reverse lookup zone rather than the forward lookup zone. 

## Project 3: DNS Initial Configuration 
- **Status**: Complete
- **Steps Taken**:
  - [x] Configured DNS Forwarder for more reliable external name resolution (8.8.8.8 dns.google)
  - [x] Created a IPv4 Reverse Lookup Zone for future utility in diagnostics and security 
  - [x] Verified using the DC's IPv4 address and domain name in `nslookup` command to confirm forward and reverse resolution 
  - [x] Enabled DNS scavenging of stale records (7 days) to keep the home lab lightweight and reduce possible conflicts
  - [x] Hardened the DNS server by restricting the listener to the DC's static IPv4 address

 ### Project 3 Challenges and Resolutions
  - **Challenge**: Initial `nslookup` to the Reverse Lookup Zone did not return with the host name
  - **Resolution**: Identified the reverse lookup zone did not have a PTR so one was manually added; verified resolution in both directions with `nslookup`
        
## Project 4: Active Directory Replication and Availability 
- **Status**: Complete
- **Steps Taken**:
  - [x] Established another Windows Server 2025 on VMware Workstation Pro
  - [x] Manually assigned static IPv4 address in sequence with the original DC (domainControl) and assigned a custom computer name (replicaControl)
  - [x] Manually assigned the preferred DNS address to match the static IPv4 address of domainControl and the alternate DNS address to 127.0.0.1
  - [x] Hardened the OS with Windows updates and the assignment of an administrator's password
  - [x] Joined replicaControl to the homelabad.lab.local domain to confirm connection and DNS resolution
   <img width="334" height="66" alt="image" src="https://github.com/user-attachments/assets/82770cd6-7ac8-4b14-8cc2-1c74436cb804" />
   
   <img width="483" height="42" alt="image" src="https://github.com/user-attachments/assets/89dc4554-7c84-42ce-a5c7-506705b7b6a6" />

  - [x] Used Add Roles and Features Wizard to install Active Directory Domain Services on replicaControl
  - [x] Promoted replicaControl to a replica domain controller
  - [x] Confirmed DNS zones and AD objects synchronized
  <img width="509" height="153" alt="image" src="https://github.com/user-attachments/assets/7c93d445-7943-4c28-9572-e7910e793fd6" />

  <img width="649" height="114" alt="image" src="https://github.com/user-attachments/assets/8a214436-a99b-44ab-b0c9-8316bd35d1b1" />

 ### Project 4 Challenges and Resolutions
  - **Challenge 1**: Attempted to `ping` domainControl's IPv4 address on replicaControl's command line but it could not be reached 
  - **Resolution 1**: Research led to enabling an inbound rule (File and Printer Sharing (Echo Request - ICMPv4-In)) in domainControl's Windows Defender Firewall allowing for inbound pings
  - **Challenge 2**: Router traffic caused DNS resolution conflicts 
  - **Resolution 2**: Research led to updating the virtual infrastructure to an isolated VMware LAN in order to remove interference 

## Project 5: Client Intergration (Manual Bootstrap)
- **Status**: Complete
- **Steps Taken**:
  - [x] Established another Windows Server 2025 on VMware Workstation Pro to serve as the client machine
  - [x] Hardened the OS with Windows updates and the assignment of an administrator's password
  - [x] Manually assigned static IPv4 address in sequence with the domain controllers
  - [x] Updated client's virtual infrastructure to the same isolated VMware LAN as the domain controllers
  - [x] Manually assigned a computer name (homelabclient) and joined it to the homelabad.lab.local domain
  - [x] Verified domain name resolution with `nslookup` and confirmed the client identified both domain controllers
  <img width="514" height="219" alt="image" src="https://github.com/user-attachments/assets/995878ae-a69e-4538-b233-556ebe44465d" />


 
### Project 5 Challenges and Resolutions
  - **Challenge**: Client hardening with Windows updates required
  - **Resolution**: Initially established VM in a bridged network configuration and proceeded with hardening, then went back to update the VM network configuration to share the same LAN segment as the domain controllers.

## Project 6: DHCP Deployment / DHCP Failover Implementation 
- **Status**: Complete
- **Steps Taken**:
  - [x] DHCP installed on both domain controllers (domainControl and replicaControl)
  - [x] Confirmed DHCP was authorized in Active Directory
  <img width="336" height="99" alt="image" src="https://github.com/user-attachments/assets/e533d39b-e59a-4e8e-9ca7-c9a882188e84" />

  - [x] Used the New Scope Wizard to set a range of IPv4 addresses on the original domain controller
  <img width="487" height="127" alt="image" src="https://github.com/user-attachments/assets/4afc166e-c275-4aff-8bfc-61683a0c882a" />

  - [x] Specified both domain controllers' IPv4 addresses for client DNS name resolution on the original domain controller
  - [x] Confirmed new scope and IPv4 address pool on the original domain controller
  <img width="580" height="76" alt="image" src="https://github.com/user-attachments/assets/fedd8918-1db0-4243-a770-f0a78867ae3f" />

  <img width="544" height="96" alt="image" src="https://github.com/user-attachments/assets/a2b62c23-600e-4fd2-ad27-01743f32becd" />

  - [x] Selected the replica domain controller (replicaControl) as the partner server in the Configure Failover Wizard
  <img width="497" height="55" alt="image" src="https://github.com/user-attachments/assets/a6cea7e0-cabe-429a-8de4-51da16264bc3" />

  - [x] Set the relationship name and mode to Load Balance
  <img width="496" height="163" alt="image" src="https://github.com/user-attachments/assets/9ca3f4f1-6aec-4960-a5b4-acffa14be8fc" />

  - [x] Updated the IPv4 properties to obtain an IP address automatically on the bootstrapped client
  - [x] Used `ipconfig /renew` to initiate and confirm a dynamic allocation from the established scope 
   <img width="621" height="279" alt="image" src="https://github.com/user-attachments/assets/8b59c133-6fcf-4b64-9d5e-d79e62b517a7" />

  - [x] Verified in both domain controllers' DHCP manager that an address was leased
  <img width="496" height="59" alt="image" src="https://github.com/user-attachments/assets/70bb5e80-2f6b-4a46-94f3-2e21c36f8875" />
        
  <img width="493" height="58" alt="image" src="https://github.com/user-attachments/assets/72501de0-6155-4a30-9063-4d03c0fb3727" />


  ## Project 7: Basic GPO and OU Implementation
  - **Status**: In Progress
  - **Steps Taken**:
    - **Goal 1**: Automate Administrative Control
    - **Status**: GPO Established (Pending Member Server)
    - [x] Created a new GPO in the Group Policy Management wizard titled Local-Admins within the Lab Servers OU
    - [x] Edited Local-Admin and added a new group titled Administrators to the Restricted Groups Policy with Administrator as the member

     - **Goal 2**: Password policy hardening
    - [x] Domain's password policy updated by setting minimum password length to 12 characters
    - [x] Insured password must meet complexity requirements was enabled
    - [x] Updated maximum password age to 90 days

     - **Goal 3**: Centralized Policies Store
    - [x] Copied local PolicyDefinitions folder to \\YourDomain.com\SYSVOL\YourDomain.com\Policies to create a shared reserve for policies
    - [x] Added a compressed PolicyDefinitions backup copy and titled it as such  

     - **Goal 4**: An Attack Vector Reduction (Disabling Print Spooler Service)
    - [x] Created a new GPO using the Group Policy Management wizard titled Disable-Print-Spooler within the Domain Controllers OU
    - [x] Navigated to System Services in the Group Policy Management Editor Defined the Print Spooler service policy as disabled
    - [x] To harden further the Allow Print Spooler to accept client connections in the Printers Administrative Template was configured to disabled
    - [x] Forced a policy update with `gpupdate /force` and verified in the Services wizard that Print Spooler wasn't running and it's Startup Type is Disabled
    <img width="766" height="75" alt="image" src="https://github.com/user-attachments/assets/44c42567-624c-4e23-8784-fc3e96acfb12" />

     - **Goal 5**: Interactive Logon Message 
     - [x] Created a new GPO using the Group Policy Management wizard titled Computer-Notice within the Lab Computers OU
     - [x] Navigated to Local Policies' Security Options and defined a Interactive logon title and Interactive logon text
     - [x] Verified by logging on to the client VM (contained in the Lab Computers OU)
    <img width="500" height="225" alt="image" src="https://github.com/user-attachments/assets/1aed76d7-08fb-41c8-8144-3eaaf265396c" />

     - **Goal 6**: 


  
