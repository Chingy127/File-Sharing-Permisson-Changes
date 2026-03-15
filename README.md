<h2>Shared Folders, Share Permissions, and NTFS Permissions</h2>

<p>
<img width="800" alt="File Share Permissions Overview"
src="images/FilePermsGraphic.png" />
</p>

<p>
This lab focuses on how shared folders work in a Windows domain environment and how different types of permissions affect what users can do when they access files over the network.
</p>

<p>
There are two main permission types being demonstrated here:
</p>

<ul>
<li><b>Share permissions</b> – these apply when a folder is accessed over the network as a shared folder.</li>
<li><b>NTFS permissions</b> – these apply to the actual files and folders on the disk.</li>
</ul>

<p>
A very important idea in this lab is that <b>share permissions apply to the shared folder itself</b>, while <b>NTFS permissions control access inside the folder structure</b>. 
In real environments, administrators often combine both permission types to control exactly who can read, write, or be denied access.
</p>

<p>
In this project, several folders are being used to simulate different levels of access:
</p>

<ul>
<li><b>read-access</b> – a folder intended to allow reading only</li>
<li><b>write-access</b> – a folder intended to allow writing or modifying content</li>
<li><b>no-access</b> – a folder intended to block access</li>
<li><b>accounting</b> – another folder being prepared as part of the file-sharing setup</li>
</ul>

<p>
This walkthrough shows how those folders are prepared, shared, and made available across the network so that domain users can test different levels of access.
</p>

<br><br>


<h3>Creating the Folder Structure on the File System</h3>

<p>
<img width="900" alt="Folder Structure on C Drive"
src="images/01-AD-File-Share.png" />
</p>

<p>
The first step is creating the folders directly on the Windows system drive. 
In this screenshot, File Explorer is open to <b>This PC &gt; Windows (C:)</b>, where several folders have been created:
</p>

<ul>
<li><b>read-access</b></li>
<li><b>write-access</b></li>
<li><b>no-access</b></li>
<li><b>accounting</b></li>
</ul>

<p>
These folders will later be shared over the network and used to demonstrate how different permission settings affect user access.
</p>

<p>
Creating the folders first is important because Windows needs an actual local folder before it can be shared with other users on the network.
</p>

<p>
At this point, the folders exist locally on the machine, but they are not yet available to other users until sharing is configured.
</p>

<br><br>


<h3>Configuring a Shared Folder With Read Access</h3>

<p>
<img width="700" alt="Network Access Read Permission"
src="images/02-AD-File-Share.png" />
</p>

<p>
This screenshot shows the <b>Network access</b> sharing window for one of the folders. 
Here, Windows is asking which users or groups on the network should be allowed to access the shared folder.
</p>

<p>
The listed entries include:
</p>

<ul>
<li><b>Administrators</b> – set to <b>Owner</b></li>
<li><b>Domain Users</b> – set to <b>Read</b></li>
<li><b>Shai</b> – set to <b>Read/Write</b></li>
</ul>

<p>
This setup means that:
</p>

<ul>
<li>Administrators have full control over the share</li>
<li>All domain users can only open and read the contents</li>
<li>The user <b>Shai</b> has additional write capability</li>
</ul>

<p>
This is a common setup when a folder needs to be broadly visible to users, but administrators still want to prevent most people from editing the contents.
</p>

<p>
Notice that this is specifically the <b>share permission</b> stage. 
This controls what happens when users connect to the folder over the network, not necessarily what they can do inside the folder once NTFS permissions are also considered.
</p>

<br><br>


<h3>Changing the Share Permission to Read/Write</h3>

<p>
<img width="700" alt="Network Access Read Write Permission"
src="images/03-AD-File-Share.png" />
</p>

<p>
In this step, the share permission for <b>Domain Users</b> has been changed from <b>Read</b> to <b>Read/Write</b>.
</p>

<p>
This means all domain users who connect to this shared folder over the network will now be allowed to:
</p>

<ul>
<li>Open files</li>
<li>Create files</li>
<li>Modify files</li>
<li>Save changes back to the shared folder</li>
</ul>

<p>
Administrators often use this type of permission when a folder is meant to be collaborative, such as a team folder where multiple users need to add or edit files.
</p>

<p>
Even though the share permission now allows writing, the final access a user receives still depends on the folder’s NTFS permissions as well. 
Windows combines both permission systems, and the most restrictive effective access usually wins.
</p>

<br><br>


<h3>Sharing a Folder With Domain Admins and a Specific User</h3>

<p>
<img width="700" alt="Network Access Domain Admins"
src="images/04-AD-File-Share.png" />
</p>

<p>
This screenshot shows another variation of the sharing configuration. 
Instead of assigning access broadly to all domain users, the share now includes:
</p>

<ul>
<li><b>Administrators</b> – <b>Owner</b></li>
<li><b>Domain Admins</b> – <b>Read/Write</b></li>
<li><b>Shai</b> – <b>Read/Write</b></li>
</ul>

<p>
This setup is more restrictive because it does not automatically grant access to every user in the domain. 
Only highly privileged administrators and the specifically listed user are given write access.
</p>

<p>
This kind of permission structure is useful for sensitive folders, where access should be limited to management, IT staff, or a small group of authorized users.
</p>

<p>
In enterprise environments, this helps reduce unnecessary exposure and follows the principle of least privilege, meaning users should only receive the minimum access they actually need.
</p>

<br><br>


<h3>Viewing the Shared Folders From the Network</h3>

<p>
<img width="900" alt="Browse Shared Folders Over Network"
src="images/05-AD-File-Share.png" />
</p>

<p>
After the folders have been shared, they can be accessed over the network from another system.
</p>

<p>
In this screenshot, File Explorer is opened to:
</p>

<pre>Network &gt; CLIENT</pre>

<p>
The shared folders visible on the machine are:
</p>

<ul>
<li><b>no-access</b></li>
<li><b>read-access</b></li>
<li><b>write-access</b></li>
</ul>

<p>
This confirms that the shares have been successfully published and are now visible to other computers on the network.
</p>

<p>
At this stage, users can attempt to open these folders and test what level of access they actually have. 
Whether they can only view the contents, create files, edit files, or are denied completely depends on how the <b>share permissions</b> and <b>NTFS permissions</b> were configured.
</p>

<p>
This is the key goal of the lab: to demonstrate that just because a folder is visible on the network does not automatically mean every user has the same level of access to it.
</p>

<p>
Visibility and permission are two different things. 
A user may see a shared folder, but once they try to open it or modify something inside it, Windows checks their permissions and either allows or blocks the action.
</p>

<h3>Testing Read-Only Access on a Shared Folder</h3>

<p>
<img width="900" alt="Read Access Permission Denied"
src="images/06-AD-File-Share.png" />
</p>

<p>
In this step, the shared folder <b>read-access</b> is opened from another machine through the network path:
</p>

<pre>Network &gt; CLIENT &gt; read-access</pre>

<p>
The user attempts to perform an action that requires modification permissions inside the folder. 
However, because the share and NTFS permissions were configured to allow <b>read-only access</b>, Windows blocks the action.
</p>

<p>
The message displayed is:
</p>

<p>
<b>"Destination Folder Access Denied – You need permission to perform this action."</b>
</p>

<p>
This confirms that the permissions applied to this folder are working correctly. 
Users can open the folder and view its contents, but they cannot create, modify, or delete files.
</p>

<p>
This type of configuration is commonly used for folders that contain reference documents or files that should not be modified by standard users.
</p>

<br><br>


<h3>Testing Write Access on a Shared Folder</h3>

<p>
<img width="900" alt="Write Access Create Folder"
src="images/07-AD-File-Share.png" />
</p>

<p>
Next, the <b>write-access</b> shared folder is opened through the network path:
</p>

<pre>Network &gt; CLIENT &gt; write-access</pre>

<p>
Unlike the read-only folder, this shared folder has been configured with permissions that allow users to create and modify files.
</p>

<p>
In this screenshot, a new folder named <b>New folder</b> is successfully created inside the shared directory.
</p>

<p>
This demonstrates that both the share permissions and NTFS permissions allow write operations for the current user.
</p>

<p>
Folders configured this way are often used for collaboration spaces where multiple users need to upload files, edit documents, or organize shared content.
</p>

<br><br>


<h3>Testing a Folder With No Access</h3>

<p>
<img width="900" alt="No Access Network Error"
src="images/08-AD-File-Share.png" />
</p>

<p>
The <b>no-access</b> folder is designed to completely block users from opening the share.
</p>

<p>
When the user attempts to open the folder from the network path:
</p>

<pre>\\CLIENT\no-access</pre>

<p>
Windows immediately returns a network error stating:
</p>

<p>
<b>"Windows cannot access \\CLIENT\no-access. You do not have permission to access this location."</b>
</p>

<p>
This indicates that the user account does not have the necessary share permissions or NTFS permissions to access the folder.
</p>

<p>
Blocking access like this is useful when administrators need to restrict sensitive directories from unauthorized users.
</p>

<br><br>


<h3>Opening Active Directory Users and Computers</h3>

<p>
<img width="900" alt="Active Directory Users and Computers"
src="images/09-AD-File-Share.png" />
</p>

<p>
To manage users and groups within the domain, administrators open <b>Active Directory Users and Computers (ADUC)</b>.
</p>

<p>
In this view, the domain <b>mydomain.com</b> is expanded to display its Organizational Units (OUs), which include:
</p>

<ul>
<li><b>_ADMINS</b></li>
<li><b>_CLIENTS</b></li>
<li><b>_EMPLOYEES</b></li>
<li><b>_GROUPS</b></li>
</ul>

<p>
Organizational Units are used to logically organize users, computers, and groups within Active Directory.
</p>

<p>
They help administrators apply policies, manage permissions, and delegate administrative tasks more easily across the domain.
</p>

<br><br>


<h3>Viewing a Security Group in Active Directory</h3>

<p>
<img width="900" alt="Accounting Security Group"
src="images/10-AD-File-Share.png" />
</p>

<p>
Inside the <b>_GROUPS</b> Organizational Unit, a security group named <b>ACCOUNTANTS</b> is shown.
</p>

<p>
Security groups are used in Active Directory to manage permissions more efficiently.
</p>

<p>
Instead of assigning permissions directly to individual users, administrators assign permissions to groups and then add users to those groups.
</p>

<p>
This approach simplifies access management because administrators only need to modify group membership rather than updating permissions across multiple folders and resources.
</p>

<br><br>


<h3>Preparing to Assign Permissions Using Security Groups</h3>

<p>
<img width="900" alt="Preparing Group-Based Permissions"
src="images/11-AD-File-Share.png" />
</p>

<p>
Using groups like <b>ACCOUNTANTS</b>, administrators can assign permissions to shared folders such as the <b>accounting</b> directory.
</p>

<p>
For example, the accounting department could be granted read/write access to the accounting folder while other users in the organization are restricted from accessing it.
</p>

<p>
This group-based permission model follows best practices in enterprise environments because it:
</p>

<ul>
<li>Reduces administrative workload</li>
<li>Makes permission management easier</li>
<li>Improves security by centralizing access control</li>
</ul>

<br><br>


<h3>Assigning the Accounting Group to a Shared Folder</h3>

<p>
<img width="900" alt="Assigning Group Permissions"
src="images/12-AD-File-Share.png" />
</p>

<p>
In this stage, the <b>ACCOUNTANTS</b> group can be added to the shared folder permissions for the <b>accounting</b> directory.
</p>

<p>
Once the group is assigned the correct permission level, any user added to the <b>ACCOUNTANTS</b> group will automatically receive the same level of access.
</p>

<p>
This means administrators do not need to manually configure folder permissions for each employee individually.
</p>

<br><br>


<h3>Testing Access Through Group Membership</h3>

<p>
<img width="900" alt="Testing Group Access"
src="images/13-AD-File-Share.png" />
</p>

<p>
After the security group has been assigned to the folder permissions, users who belong to the <b>ACCOUNTANTS</b> group will be able to access the shared folder.
</p>

<p>
If a user is removed from the group, their access will automatically be revoked the next time permissions are evaluated.
</p>

<p>
This demonstrates how Active Directory groups can be used to efficiently manage access to network resources such as shared folders, printers, and applications.
</p>

<p>
Using groups rather than assigning permissions directly to users is considered a best practice in enterprise environments because it keeps access control organized, scalable, and easier to maintain.
</p>


<h3>Assigning the ACCOUNTANTS Group to the Accounting Shared Folder</h3>

<p>
<img width="900" alt="Assign ACCOUNTANTS group to shared folder"
src="images/11-AD-File-Share.png" />
</p>

<p>
In this step, the <b>accounting</b> folder is being shared and access permissions are assigned to a specific Active Directory security group.
</p>

<p>
The group <b>ACCOUNTANTS</b> has been added to the list of users allowed to access the share. The permission level assigned is <b>Read/Write</b>, which allows members of the group to:
</p>

<ul>
<li>Open files</li>
<li>Create new files</li>
<li>Edit existing files</li>
<li>Save changes inside the folder</li>
</ul>

<p>
Administrators are automatically listed as <b>Owner</b>, which means they have full control over the shared folder. This includes the ability to modify permissions, delete files, and manage access.
</p>

<p>
Using security groups like <b>ACCOUNTANTS</b> instead of assigning permissions to individual users makes access management significantly easier in enterprise environments. When a new employee joins the accounting department, administrators simply add the user to the group and the permissions are automatically applied.
</p>

<br><br>

<h3>Attempting to Access the Accounting Share Without Proper Group Membership</h3>

<p>
<img width="900" alt="Access denied to accounting share"
src="images/12-AD-File-Share.png" />
</p>

<p>
Next, the user attempts to open the shared folder from another machine through the network path:
</p>

<pre>\\DC01\accounting</pre>

<p>
Windows displays a network error message stating that access to the folder is denied.
</p>

<p>
This occurs because the current user account has not yet been added to the <b>ACCOUNTANTS</b> security group. Since the folder permissions only allow access to that group, users outside of the group are blocked.
</p>

<p>
This demonstrates how group-based permissions restrict access to network resources and enforce proper access control policies.
</p>

<br><br>

<h3>Adding a User to the ACCOUNTANTS Security Group</h3>

<p>
<img width="900" alt="User membership in ACCOUNTANTS group"
src="images/13-AD-File-Share.png" />
</p>

<p>
To grant access, the administrator opens <b>Active Directory Users and Computers</b> and locates the user account <b>bis.guj</b>.
</p>

<p>
Inside the user’s properties window, the <b>Member Of</b> tab shows the groups the user currently belongs to.
</p>

<p>
The user has now been added to the <b>ACCOUNTANTS</b> group, which resides in the <b>_GROUPS</b> organizational unit.
</p>

<p>
Once this change is applied, the user automatically inherits the permissions assigned to the group.
</p>

<p>
This is a fundamental concept in Active Directory permission management: users gain access to resources through group membership rather than being granted permissions directly.
</p>

<br><br>

<h3>Verifying Membership Inside the ACCOUNTANTS Group</h3>

<p>
<img width="900" alt="ACCOUNTANTS group members"
src="images/14-AD-File-Share.png" />
</p>

<p>
The properties of the <b>ACCOUNTANTS</b> group are opened to verify its members.
</p>

<p>
Under the <b>Members</b> tab, the user <b>bis.guj</b> is now listed as a member of the group.
</p>

<p>
This confirms that the user should inherit the permissions associated with the accounting shared folder.
</p>

<p>
Group membership changes may require the user to log off and log back in for the new permissions to fully apply because Windows caches authentication tokens at login time.
</p>

<br><br>

<h3>Confirming the Logged-In User Account</h3>

<p>
<img width="900" alt="whoami command"
src="images/15-AD-File-Share.png" />
</p>

<p>
To confirm the identity of the currently logged-in user, the <b>whoami</b> command is executed in PowerShell.
</p>

<pre>whoami</pre>

<p>
The output shows:
</p>

<pre>mydomain\bis.guj</pre>

<p>
This verifies that the active session belongs to the user account that was just added to the <b>ACCOUNTANTS</b> security group.
</p>

<p>
This step helps ensure that the correct user is being used to test the folder permissions.
</p>

<br><br>

<h3>Successfully Accessing the Accounting Shared Folder</h3>

<p>
<img width="900" alt="Accessing accounting share"
src="images/16-AD-File-Share.png" />
</p>

<p>
After the user has been added to the correct group, the shared folder is opened again using the network path:
</p>

<pre>Network &gt; DC01 &gt; accounting</pre>

<p>
This time the folder opens successfully without any permission errors.
</p>

<p>
The directory appears empty because no files have been created yet. However, the important result is that the user now has the ability to access the folder.
</p>

<br><br>

<h3>Creating and Editing a File in the Shared Folder</h3>

<p>
<img width="900" alt="Creating file in accounting share"
src="images/17-AD-File-Share.png" />
</p>

<p>
To confirm that write permissions are functioning correctly, a new text file named <b>yo.txt</b> is created inside the accounting shared folder.
</p>

<p>
The file is opened and text is written inside it, demonstrating that the user can:
</p>

<ul>
<li>Create files</li>
<li>Modify file contents</li>
<li>Save changes inside the shared directory</li>
</ul>

<p>
This confirms that the permissions assigned to the <b>ACCOUNTANTS</b> group are working as expected.
</p>

<p>
Because the user <b>bis.guj</b> is a member of the group, they inherit the <b>Read/Write</b> permissions that were assigned to the shared folder.
</p>

<p>
This final step demonstrates how Active Directory security groups simplify access management for shared resources in a domain environment.
</p>
