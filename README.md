
<p align="center">
<img src="https://i.imgur.com/xlYwPUf.png" alt="Permissions Photo"/>
</p>

<h1>Understanding File Permissions</h1>
This lab focuses on file permissions and shares within the context of an Active Directory domain. Proper file permissions and sharing are essential in a business environment to ensure users have the right level of access to the files and resources they need. This lab builds on a previous exercise where I joined a client machine to the ernestotest.com domain. For this lab, I am logged in as Jane Doe, an admin account, on the domain controller VM, and as a regular user on the client VM. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>File Permissions Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/bUjobDC.png" height="80%" width="80%" alt="Permissions Steps"/>
<img src="https://i.imgur.com/lz1DMos.png" height="80%" width="80%" alt="Permissions Steps"/>
</p>
<p>
To set file and folder permissions, we first need to create the folders that we want to share. While logged in as an admin on the domain controller VM, I created four folders on the C:\ drive, each with a descriptive name for their intended use.

To share a folder and assign permissions, right-click the folder, go to Properties, and then click on the Sharing tab. Under the Sharing tab, select Share to specify the users or groups on the network that you want to share the folder with, and assign the appropriate permissions (read, write, etc.).

For this lab, I set permissions for each of the folders. The Accounting folder's permissions will be updated later.

For the folder permissions, here’s how I set them up:

Domain Users: They have Read permissions on the read-access folder, meaning they can view and open the files in this folder but cannot make changes. They have Read/Write permissions on the write-access folder, allowing them to both view and modify files within it.

Domain Admins: They have Read/Write permissions on the no-access folder, giving them full access to read and modify files in the folder. This might seem counterintuitive to the folder name, but in this case, "no-access" refers to the folder's restricted access for standard users, not admins.

These settings ensure that users have the appropriate level of access according to their roles within the domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/kWrpLFE.png" height="80%" width="80%" alt="Permissions Steps"/>
</p>
<p>
On the client VM, you can access the shared folders by navigating to the following path in File Explorer: \\dc-1. Once there, you will notice the following:

Some folders, like the read-access folder, allow you to view the files but not add or modify them. This is because Domain Users have only Read permissions for that folder.

Other folders, such as the write-access folder, allow both viewing and modifying files since Domain Users have Read/Write permissions for that folder.

One folder, likely the no-access folder, won’t allow access at all. This is due to the Domain Admins permissions, where only admins have access, while Domain Users do not have any permissions for that folder.

This demonstrates how folder permissions are tied to the Security Group (Domain Users in this case) and the specific permissions set for each folder, restricting or granting access based on these configurations.
</p>
<br />

<p>
<img src="https://i.imgur.com/NgM0CcI.png" height="80%" width="80%" alt="Permissions Steps"/>
<img src="https://i.imgur.com/I4k9T2J.png" height="80%" width="80%" alt="Permissions Steps"/>
</p>
<p>
To create a new Security Group and assign permissions to the accounting folder, follow these steps:

1.) Create the Security Group:
- Open Active Directory Users and Computers on the domain controller.
- In the left pane, right-click on the domain (e.g., ernestotest.com) and select New → Group.
- Name the group ACCOUNTANTS and set the group scope to Global and the group type to Security.
- Click OK to create the group.

2.) Assign Permissions to the Accounting Folder:
- Go to the accounting folder on the domain controller (on the *C:* drive).
- Right-click the folder and select Properties.
- In the Properties window, go to the Sharing tab and click Advanced Sharing.
- In the Advanced Sharing window, click Permissions.
- Under the Permissions window, click Add, and search for the ACCOUNTANTS group.
- Once the group is selected, assign the Read/Write permissions (allowing full access to the folder) by checking the appropriate boxes.
- Click Apply and then OK to save the changes.

Now the ACCOUNTANTS group will have Read/Write access to the accounting folder, meaning members of the group can view and modify the files within that folder.
</p>
<br />

<p>
<img src="https://i.imgur.com/xev1Svv.png" height="80%" width="80%" alt="Permissions Steps"/>
<img src="https://i.imgur.com/SHotVB2.png" height="80%" width="80%" alt="Permissions Steps"/>
</p>
<p>
To resolve the issue where the user cannot access the accounting folder, follow these steps:

1.) Log Off the Client VM:
- Log off the client machine to ensure that the permissions are properly applied when logging back in.

2.) Add the User to the ACCOUNTANTS Group:
- On the domain controller, open Active Directory Users and Computers.
- Locate the ACCOUNTANTS security group by navigating to the domain (e.g., ernestotest.com) and then selecting the ACCOUNTANTS group.
- Right-click on the ACCOUNTANTS group and select Properties.
- Go to the Members tab and click Add.
- In the Select Users, Contacts, or Computers window, type the username of the user you want to add (e.g., bon.rovej) and click Check Names.
- Once the username is verified, click OK to add the user to the ACCOUNTANTS group.

3.) Log Back into the Client VM:
- Log back into the client machine (as bon.rovej) to ensure that the group membership is updated.
- Once logged in, bon.rovej should now have access to the accounting folder because they are now a member of the ACCOUNTANTS security group, which has the required permissions to read and write in the folder.

Now, bon.rovej will be able to access, modify, and manage files in the accounting folder, as they are part of the group that has been granted Read/Write access.
<br />

<h2>Lessons Learned </h2>

This lab helped me gain a deeper understanding of how file permissions work within Windows and Active Directory environments. In a real-world scenario, managing file access is crucial for ensuring security and efficiency. By assigning specific permissions to folders and files, I can ensure that users only have access to what they need to complete their tasks, without granting unnecessary permissions. This principle, known as the least privilege principle, is essential in any secure IT environment to minimize the risk of unauthorized access or accidental data loss.
