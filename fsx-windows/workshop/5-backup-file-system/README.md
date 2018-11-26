![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Windows File Server**

## Backup file system

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Backup file system

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Map a file share**](../4-create-new-shares)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 5.1: Backup the file system

- Click on the link below to log in to the Amazon FSx Management Console in the same AWS region as your file system. 

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/fsx/home?region=us-east-1#file-systems) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/fsx/home?region=us-east-2#file-systems) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/fsx/home?region=us-west-2#file-systems) |
| eu-west-1 | [EU East (Ireland)](https://console.aws.amazon.com/fsx/home?region=eu-west-1#file-systems) |

- Select the FSx for Windows file system you created earlier in the workshop
- Click **Actions > Create backup**
- The backup is an incremental file-system consistent backup. This is an online operation so you can continue with the next section of the workshop.

---
## Next section
### Click on the link below to go to the next section

| [**Mount file system**](../6-mount-file-system) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.


