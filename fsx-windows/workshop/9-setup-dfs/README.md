![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Windows File Server**

## Setup DFS

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Setup DFS

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Restore backup**](../7-restore-backup)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 9.1: Setup a DFS Namespace for consolidation

- From the Amazon EC2 Console, copy the **Public DNS (IPv4)** name of the **NSS1-fsx-...** instance
- Launch your remote desktop application to log on to the Windows EC2 instance you created in the previous workshop
- Log on to the **NSS1-fsx-...** instance using following AD credentials

| Username | Password |
| :--- | :--- 
| admin@<<directory>> (e.g. admin@example.com) | The Microsoft Active Directory (MAD) password you entered as a parameter when you launched the prerequisites CloudFormation stack|

- Make a note of the computer name of the instance (e.g. $NSS1 = "EC2AMAZ-")

- From the Amazon EC2 Console, copy the **Public DNS (IPv4)** name of the **NSS2-fsx-...** instance
- Launch your remote desktop application to log on to the Windows EC2 instance you created in the previous workshop
- Log on to the **NSS2-fsx** instance using following AD credentials

| Username | Password |
| :--- | :--- 
| admin@<<directory>> (e.g. admin@example.com) | The Microsoft Active Directory (MAD) password you entered as a parameter when you launched the prerequisites CloudFormation stack|

- Make a note of the computer name of the instance (e.g. $NSS2 = "EC2AMAZ-")

- From either Namespace Server instance (e.g. NSS1 or NSS2), complete the following steps

> Complete the following steps logged on to one of the namespace instances

- Access the DFS management console. Open the Start menu and type dfsmgmt.msc and print the enter/return key.

- Connect to a new namespace. Select **Action** the choose **New Namespace...**

- Browse and select one of the two namespace servers and choose **Next**.

- Enter the name of the namespace (e.g. corp).

- Set the permissions. Choose **Edit Settings** and set the appropriate permissions based on your requirements. Choose **Next**.

- Leave the default **Domain-based namespace** option selected. 

- Leave the **Enable Windows Server 2008 mode** option selected. 

- Choose **Next**.

- Review the namespace settings and choose **Create**.

- With the newly created namespace selected choose **Action** then **Add Namespace Server...**.

- From the Add Namespace Server window, choose **Browse...** and select the other namespace server.

- Select **Edit Settings...** and set the appropriate permissions based on your requirements. Choose **OK**.

- Context-click the namespace you just created and choose **New Folder...**.

- Enter the name of the folder (e.g. share) and choose **Add...**.

- Enter the DNS name of the file system share in UNC format (e.g. \\fs-0123456789abcdef.example.com\share) and choose OK.

- Use the table below and repeat the steps above for the other shares for the other shares. 

| Share name |
| :--- |
| data |
| finance |
| sales |
| marketing |

### Step 9.2: Access the new file shares

- Open **File Explorer**
- Type the UNC path to one of the new namespace (e.g. **\\\\example.com\corp\\**)
- Access all the new shares created above (e.g. \data, \finance, \sales, marketing)
- Can we read and write to all shares?

### Step 9.3: Setup a DFS Replication

> Complete the following steps logged on to one of the namespace instances

- These steps will setup DFS replication, replicating the default file share (/share) of the first file system you created to the second file system you created from the restore.

- Open a **PowerShell** window as an **Administrator**

- Create a DFS replication group and folder

```sh
$Group = "Group" # e.g. "ReplicationGroup"
$Folder = "Folder" # e.g. "folder"

New-DfsReplicationGroup -GroupName ${Group}
Grant-DfsrDelegation -GroupName ${Group} –AccountName "FSxAdmins" –Force
New-DfsReplicationFolder -GroupName ${Group} -FolderName ${Folder}

```


- Determine the Active Directory computer name associated with the write file system and the first read file system.

```sh
$FirstFSDnsName = "DNS Name of the source file system" # e.g. "fs-abcdef0123456789.example.com"
$RestoredFSDnsName = "DNS Name of the restored file system" # e.g. "fs-0123456789abcdef.example.com"
$FirstFSComputerName = (Resolve-DnsName ${FirstFSDnsName} -Type CNAME).NameHost
$ResotredFSComputerName = (Resolve-DnsName ${RestoredFSDnsName} -Type CNAME).NameHost

```


- Add your two file systems as members of the DFS Replication group.

```sh
Add-DfsrMember -GroupName ${Group} -ComputerName ${FirstFSComputerName}
Add-DfsrMember -GroupName ${Group} -ComputerName ${RestoredFSComputerName}

```


- Add the local path of the default file share (e.g. D:\share) for each file system to the DFS Replication group. In this workshop, the first file system will serve as the primary member, meaning that its contents will initially be synced to the restored file system.



```sh
$FirstFSReplicationPath = "D:\Share"
$RestoredFSReplicationPath = "D:\Share"

Set-DfsrMembership –GroupName ${Group} –FolderName ${Folder} –ContentPath ${FirstFSReplicationPath} –ComputerName ${FirstFSComputerName} –PrimaryMember $True -Force
Set-DfsrMembership –GroupName ${Group} –FolderName ${Folder} –ContentPath ${RestoredFSReplicationPath} –ComputerName ${RestoredFSComputerName} –PrimaryMember $False -Force

```


- Add a connection between the file shares.

```sh
Add-DfsrConnection -GroupName ${Group} -SourceComputerName ${FirstFSComputerName} -DestinationComputerName ${RestoredFSComputerName}
```

- Map a drive to the default file shares of both file systems
- Open each mapped drive in a separate **File Explorer** window
- It may take 10-15 minutes for replication to start.
- Once the replication is complete, test creating files on one file system and see them get replicated to the other.


---
## Tear-down
### Delele all the resources you created in this workshop

- Delete both Amazon WorkSpaces
- Once the WorkSpaces are deleted you must also register WorkSpaces from Active Directory
- Remove the inbound rule referencing the Amazon WorkSpaces Security Group from the VPC's default security group
- Terminate the Windows EC2 instance you launched
- Terminate the Amazon Linux EC2 instance you launched
- Delete the file system backups
- Delete the both file systems
- Delete the CloudFormation stack
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.

