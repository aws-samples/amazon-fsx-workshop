![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)


# **Amazon FSx for Windows File Server**

## Workshop

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

---

## Workshop Overview

### Overview

This will show solutions architects how to take advantage of a fully-managed Windows native file system for various application workloads like home directories, web serving & content management, enterprise applications, analytics, and media & entertainment.

This workshop will cover how to create and initiate an incremental file-system-consistent backup of a file system, as well as how to access the file system from a broad range of clients from Windows and Linux EC2 instances to Amazon WorkSpaces. We will also restore a backup as a new file system and setup Microsoft Distributed File System (DFS) Replication between two file systems across Availability Zones. For those of you who want to consolidate multiple file systems under a single namespace, we'll spend time setting up DFS Namespaces to consolidate muliptle shares under a single namespace.

### Informational

You will be using your own AWS account for all workshop activities.
WARNING!! This workshop will exceed your free-usage tier. You will incur charges as a result of stepping through this workshop. Terminate all resources that were created during this workshop so you don’t continue to incur ongoing charges.

### Prerequisites

Click on the link below to go to the Amazon FSx for Windows File Server prerequisites section. Start this section at the start of the workshop, as soon as you sit down. The referenced Amazon CloudFormation template will create a stack that contains all the prerequisites for the next section of the workshop and it will take 20-30 minutes for the CloudFormation stack to  automatically build out the environment. You must complete this section before moving to **The Workshop**.

| Step | Workshop |
| :--- | :---
| 0 | [Prerequisites](workshop/0-prerequisites) |

### The Workshop

Click on the links below to go to the FSx for Windows workshop. You must first complete **Prerequisites** above before moving on to the steps in**The Workshop** below. These steps must be completed in order.


| Step | Workshop |
| :--- | :---
| 1 | [Create a file system](workshop/1-create-file-system) |
| 2 | [Launch a few clients](workshop/2-launch-clients) |
| 3 | [Map a file share on Windows](workshop/3-map-file-share)
| 4 | [Create new shares](workshop/4-create-new-shares)
| 5 | [Backup the file system](workshop/5-backup-file-system)
| 6 | [Mount a file system on Linux](workshop/6-mount-file-system)
| 7 | [Restore a backup](workshop/7-restore-backup)
| 8 | [WorkSpaces access](workshop/8-workspaces-access)
| 9 | [Setup DFS](workshop/9-setup-dfs)


## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.
