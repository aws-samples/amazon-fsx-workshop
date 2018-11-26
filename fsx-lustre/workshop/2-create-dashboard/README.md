![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Create dashboard

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---
### Prerequisites

* An AWS account with administrative level access
* An Amazon EC2 key pair
* An Amazon FSx for Lustre file system

If a key pair has not been previously created in your account, please refer to [Creating a Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) from the AWS EC2 User's Guide.  

Verify that the key pair is created in the same AWS region you will use for the tutorial.

WARNING!! This tutorial environment will exceed your free-usage tier. You will incur charges as a result of building this environment and executing the scripts included in this tutorial. Delete all files on the EFS file system that were created during this tutorial and delete the  stack so you don’t continue to incur additional compute and storage charges.

---
### 2.1 Create Amazon CloudWatch dashboard for your FSx for Lustre file system

The link below will help launch a CloudFormation stack that will crate a CloudWatch dashboard for your FSx for Luster file system.

#### Parameters

- FSx for Lustre file system id

- You can find the file system id using FSx Console or by running the CLI command below, substituting the appropriate region parameter based on your environment.

```sh
aws fsx describe-file-systems --output json --region <region>
```

---

This CloudFormation template will launch a CloudFormation stack that will create a CloudWatch dashboard to visually display the following file system metrics:

| Widget | Metric Name | Metric Math Express |
| :--- | :--- | :--- |
| Available Storage Capacity (TiB)| Available Storage (TiB) | freeDataStorageCapacity/1073741824 |
| Throughput (MiB/s) | Total Data Throughput (MiB/s) | SUM(METRICS())/1048576/PERIOD(readBytes) |
| Throughput (MiB/s) | Data Write Throughput (MiB/s) | writeBytes/1048576/PERIOD(writeBytes) |
| Throughput (MiB/s) | Data Read Throughput (MiB/s) | readBytes/1048576/PERIOD(readBytes) |
| Percent Throughput (%) | Percent Write Throughput (%) | writeBytes*100/SUM(METRICS()) |
| Percent Throughput (%) | Percent Read Throughput (%)  | readBytes*100/SUM(METRICS()) |
| Operations per Second (iops) | Total Operations per second (iops) | SUM(METRICS())/PERIOD(metadataOperations) |
| Operations per Second (iops) | Data Write Operations per second (iops) | dataWriteOperations/PERIOD(dataWriteOperations) |
| Operations per Second (iops) | Data Read Operations per second (iops) | dataReadOperations/PERIOD(dataReadOperations) |
| Operations per Second (iops) | Metadata Operations per second (iops) | metadataOperations/PERIOD(metadataOperations) |
| Percent Operations per Second (%) | Percent Write Operations per second (%) | dataWriteOperations*100/SUM(METRICS()) |
| Percent Operations per Second (%) | Percent Read Operations per second (%) | dataReadOperations*100/SUM(METRICS()) |
| Percent Operations per Second (%) | Percent Metadata Operations per second (%) | metadataOperations*100/SUM(METRICS()) |

- Click on the link below in the same AWS region where you created your FSx for Lustre file system. 

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=fsx-lustre-workshop-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/fsx-workshop/lustre/current/templates/FSx_CW_Dashboard.yaml) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=fsx-lustre-workshop-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/fsx-workshop/lustre/current/templates/FSx_CW_Dashboard.yaml) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=fsx-lustre-workshop-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/fsx-workshop/lustre/current/templates/FSx_CW_Dashboard.yaml) |
| eu-west-1 | [EU West (Ireland)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=fsx-lustre-workshop-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/fsx-workshop/lustre/current/templates/FSx_CW_Dashboard.yaml) |

---
## Next section
### Click on the link below to go to the next section

| [**Launch a client**](../3-launch-client) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.
