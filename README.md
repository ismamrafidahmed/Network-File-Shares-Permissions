<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in Windows Server 2022 Virtual Machine </h1>
This tutorial outlines the implementation of on-premises Active Directory within Windows Server 2022 using VirtualBox<br />

<h2>Environments and Technologies Used</h2>

- VirtualBox
- Active Directory Domain Services
- Group Policy Management 

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro

<h2>Deployment and Configuration Steps Covered in This Section</h2>

- Network File Shares and Permissions

<h2>Deployment and Configuration Steps</h2>

Create some sample file shares with various permissions:
  1. Connect/log into DC-1 as your domain admin account 
  2. Connect/log into Client-1 as a domain user
  3. On DC-1, on the C: drive, create 4 folders: “read-access”, “write-access”, “no-access”, “accounting”

![image](https://github.com/user-attachments/assets/2b1d5cf7-b37c-42a6-b94e-189da7c3d466)

Set the following permissions (share the folder) 
  1. Folder: “read-access”, Group: “Domain Users”, Permission: “Read” 
  2. Go to Properties, sharing, share type “domain users”, give read access

![image](https://github.com/user-attachments/assets/1c481cf8-cfbb-42d7-8347-434913be1a4c)

Folder: “write-access”, Group: “Domain Users”, Permissions: “Read/Write”
  1. Go to Properties, sharing, share type “domain users”, give read/write access
  2. Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write” 
  3. Go to Properties, sharing, share type “domain admins”, give read/write access
  4. Skip accounting for now

Attempt to access file shares as a normal user: 
  1. Log in to Client-1 as a domain user, navigate to the shared folder (start, run, \\dc-1)
     ![image](https://github.com/user-attachments/assets/d38bfbd4-44ba-41e8-a5bf-90d8aa41e646)
  2. Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in?
     ![image](https://github.com/user-attachments/assets/a203b26c-f0e2-4fef-8190-addfe6188b39)
     ![image](https://github.com/user-attachments/assets/c914790d-bdfd-484c-9f1b-2aa2dd9ce070)
     ![image](https://github.com/user-attachments/assets/97d782c0-b188-4e13-b78f-14c0d4f288ca)

Create an “ACCOUNTANTS” Security Group, assign permissions, and test access:
  1. Go back to DC-1, in Active Directory, create a security group called “ACCOUNTANTS”
  2. Go ADUC, create an OU called “_GROUPS”, right click, and select group.
     ![image](https://github.com/user-attachments/assets/e7347cc9-4fd4-49d3-820d-4020c102f40f)
  3. On the “accounting” folder you created earlier, set the following permissions below
  4. Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”
     ![image](https://github.com/user-attachments/assets/779d9c2e-64c5-4812-812b-63fa7b1fcedd)
  5. On Client-1, as a domain user (kyle Lowry), attempt to access the Accountants folder. It should fail because this user is not a part of the  “ACCOUNTANTS” security group.
     ![image](https://github.com/user-attachments/assets/b2e49c73-6a8d-4036-8491-5871021324cb)
  6. Log out of Client-1 as the domain user.
  7. On DC-1, make the same domain user a member of the “ACCOUNTANTS” Security Group
  8. Go to properties on the “ACCOUNTANTS” security group, click members, add the name of domain user you to want to add to this security group, check names and click apply.
     ![image](https://github.com/user-attachments/assets/91cb4d33-4b75-4e7c-a0e5-61c12e8e36e4)
  9. Sign back into Client-1 as the same domain user and try to access the “accounting” share in \\DC-1
  10. It will work now as you will see that you now have access to the accounting share as you were added to the “ACCOUNTANTS” security group. 
      ![image](https://github.com/user-attachments/assets/3d3559a9-f790-4a69-9f9b-90546558e9c1)









