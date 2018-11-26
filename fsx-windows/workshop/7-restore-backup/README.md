![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Windows File Server**

## Restore backup

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Restore backup

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Map a file share**](../4-create-new-shares)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 7.1: Restore a backup

- Click on the link below to log in to the Amazon FSx Management Console in the same AWS region as your file system. 

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/fsx/home?region=us-east-1#file-systems) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/fsx/home?region=us-east-2#file-systems) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/fsx/home?region=us-west-2#file-systems) |
| eu-west-1 | [EU East (Ireland)](https://console.aws.amazon.com/fsx/home?region=eu-west-1#file-systems) |

- Click the **File system name** link of the FSx for Windows file system you created earlier in the workshop
- Click the **Backups** tab
- Click the checkbox next to the backup you created earlier. It should have the type **User-Initiated**
- Click **Restore backup**
- Complete the **Create file system from backup...** wizard with the following settings

| File system details | Value |
| :--- | :--- 
| File system name | type in a name of your file system - this will **not** be used to access the file share or file system |
| Storage capacity | Immutable when restoring a backup |
| Throughput capacity | 64 MB/s |


| Network & security | Value |
| :--- | :--- 
| Virtual private cloud (VPC) | Accept the default |
| Availability zone | Select a different Availability Zone from the default |
| Subnet | Accept the default |
| VPC Security Groups | Accept the default |


| Windows authentication | Value |
| :--- | :--- 
| Microsoft Active Directory ID | Accept the default |


| Encryption | Value |
| :--- | :--- 
| Encryption key | Accept the default |


| Maintenance preferences | Value |
| :--- | :--- 
| Daily automatic backup window | No preference |
| Automatic backup retention period | 0 days (this disables automatic backups) |
| Weekly maintenance window | No preference |


- Select **Review summary**

- Review the file system attributes & estimated monthly costs

- Select **Create file system**

---
## Next section
### Click on the link below to go to the next section

| [**Mount file system**](../6-mount-file-system) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.



