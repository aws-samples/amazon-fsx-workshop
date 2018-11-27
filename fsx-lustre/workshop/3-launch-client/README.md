![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Launch a client

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---
### Prerequisites

* An AWS account with administrative level access
* An Amazon EC2 key pair

If a key pair has not been previously created in your account, please refer to [Creating a Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) from the AWS EC2 User's Guide.  

Verify that the key pair is created in the same AWS region you will use for the tutorial.

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and executing the scripts included in this workshop. Delete all AWS resources created during this workshop so you don’t continue to incur additional compute and storage charges.

---
### Launch a client

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Create a file system**](../1-create-file-system).

### Step 3.1: Launch a Linux EC2 instance

- Click on the link below to log in to the Amazon EC2 Console in the same AWS region where you launched your CloudFormation stack.

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:) |
| eu-west-1 | [EU West (Ireland)](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:) |

- Launch one EC2 instance with the following configuration details. If a value isn't specified below, accept the default value. A variety of Linux distributions are supported. Select one of the listed AMIs from Community AMIs or the AWS Marketplace. Search in the Community AMIs or AWS Marketplace for one of the AMI values below.

| Configuration detail | Value |
| :--- | :--- 
| Amazon Machine Image (AMI) | Red Hat Enterprise Linux 7.6 (HVM), SSD Volume Type </br>us-east-1 = ami-011b3ccf1bd6db744 |
| |
| Instance Type | c5.2xlarge |
| |
| Number of instances | 1 |
| Network | Select the VPC Id created in the prerequisites section (to verify the the VPC Id, view the output of the AWS CloudFormation stack) |
| |
| Add tags | Key=Name; Value=Lustre client - FSx Workshop  |
| |
| Security group | Select the existing default VPC security group  |
| |
| EC2 key pair | Select an existing EC2 key pair you have access to  |

- Launch the instance


---
## Next section
### Click on the link below to go to the next section

| [**Mount file system**](../4-mount-file-system) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.
