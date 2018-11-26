![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Windows File Server**

## Create new shares

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Create new shares

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Map a file share**](../3-map-file-share)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 4.1: Log on to the Windows EC2 instance

- From the Amazon EC2 Console, select the **Public DNS (IPv4)** name of the **Windows Server 2016 - FSx Workshop** instance
- Launch your remote desktop application to log on to the Windows EC2 instance you created in the previous workshop
- Log on to the **Windows Server 2016 - FSx Workshop** instance using following AD credentials

| Username | Password |
| :--- | :--- 
| admin@<directory> (e.g. admin@example.com) | The Microsoft Active Directory (MAD) password you entered as a parameter when you launched the prerequisites CloudFormation stack|

### Step 4.2: Create file shares

> Complete the following steps when logged on to the **Windows Server 2016 - FSx Workshop** instance

- Click **Start**
- Type **fsmgmt.msc**
- From the **Shared Folders** window, select **Action > Connect to another computer...**
- Click **Browse...**
- Click **Advanced...**
- Click **Find Now**
- Select the AWS Managed instance hosting the FSx for Windows file system (e.g. AMZNFSX...) you created in the **Create file system** workshop.
- Click **OK** until you're back to the **Shared Folders** window
- Double-click the **Shares** folder
- With the **Shares** folder selected, click **Action > New Share...** from the menu.
- Complete the **Create A Shared Folder Wizard** creating new shares based on the following information.

| Share name | Folder path | Create new path | Shared folder permissions
| :--- | :--- |
| data | D:\data | Yes | Customize permissions > Everyone Full Control |
| finance | D:\finance | Yes | Administrators have full access; other users have no access |
| sales | D:\sales | Yes | All users have read-only access |
| marketing | D:\marketing | Yes | Customize permissions > Everyone Full Control |

- Experiment and create other file shares. All shares must be created on the D:\ drive.

### Step 4.3: Access the new file shares

> Complete the following steps when logged on to the **Windows Server 2016 - FSx Workshop** instance

- Open **File Explorer**
- Type the UNC path to one of the new shares using the file system's DNS name (e.g. **\\\\fs-0123456789abcdef.example.com\data**)
- Access all the new shares created above (e.g. \data, \finance, \sales, marketing)
- Can we read and write to all shares?

---
## Next section
### Click on the link below to go to the next section

| [**Backup file system**](../5-backup-file-system) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.

