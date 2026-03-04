
# INTRODUCION
This repository was created for log my training in Active Directory(AD) Penetration Testing.  Here you will see installation process, Domain setup process, connecting User's PC's to domain and Creating Domain Users. 
# What we Actually Testing
Active Directory is Microsoft product, created to manage Domains networks, Auth devices and store information about domain objects. Knowengle of AD is important because whole world using this to work with domains.
AD can be splitted to Physical components and Logical components. 
Physical components are real devices, such as Domain Controllers(DC), and other PC's.
- Domain controller - stores all information about Domain, Users, PC's, Paswords, Policies.
- Data store - Contains ntds.dat, which contains all information about users.
AD Logical components:
- AD DS Schema - Blueprint of Domain, contains **Class Object** and **Attribute object**
- - Class Object is representation of objects in domain (User or Computer)
- - Attribute Object is representation of infomation attached to object (Display Name)
- Domain - Group of objects
- Trees - Group of Domains, like EU.exploitable.lab and US.exploitable.lab
- Forest - Collection of trees.
- Organization Units(OU) -  AD containers which can contain users, computers and other UO's
-  Trusts - Gain access to resources
-  - Directional trust flows from trusted to trusting domain
-  - transitive trust extended between directional trust, includes other trusted domains
- 

# Scheme
Domain **exploitable.lab** will use 1 PC as DC and 2 PC's as users.

- DC: Windows Server 2025
- Users: Windows 10 Pro

|Computer | OS | User |
|---------|----|------|
| Domain-Exploit | Windows Server 2025 | Administrator 
| DESKTOP-REIMU | Windows 10 Pro | Marisa 
| DESKTOP-SUIKA | Windows 10 Pro | Suika 
 
# Installation
After installation Windows Server, in server manager we adding some roles and features:
- Active Directory Domain Service
- Active Directory Certificate Service

After setting up all computers we have this lab scheme:

| Computer | IP |
|--|--|
| Domain Controller | 192.168.31.150 |
| user SUIKA | 192.168.31.152 |
| user REIMU | 192.168.31.151 |



 
> Written with [StackEdit](https://stackedit.io/).
