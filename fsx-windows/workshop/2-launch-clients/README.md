![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Windows File Server**

## Launch clients

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Launch clients

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Create a file system**](../1-create-file-system)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 2.1: Launch a Windows EC2 instance

- Click on the link below to log in to the Amazon EC2 Console in the same AWS region where you launched your CloudFormation stack.

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:) |
| eu-west-1 | [EU West (Ireland)](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:) |

- Launch an EC2 instance with the following configuration details. If a value isn't specified below, accept the default value. Create one EC2 instance per table below.

| Configuration detail | Value |
| :--- | :--- 
| Amazon Machine Image (AMI) | Microsoft Windows Server 2016 Base |
| |
| Instance Type | c5.2xlarge |
| |
| Number of instances | 1 |
| Network | Select the VPC Id created in the prerequisites section (to verify the the VPC Id, view the output of the AWS CloudFormation stack) |
| Domain join directory | Select the Directory Id created in the prerequisites section (to verify the the Directory Id, view the output of the AWS CloudFormation stack) |
| IAM role | Select fsx-workshop-prerequisites... |
| |
| Add tags | Key=Name; Value=Windows Server 2016 - FSx Workshop  |
| |
| Security group | Select the default VPC security group  |
| |
| EC2 key pair | Select an existing EC2 key pair you have access to  |

- Launch the instance

### Step 2.2: Launch a Linux EC2 instance

- Click on the link below to log in to the Amazon EC2 Console in the same AWS region where you launched your CloudFormation stack.

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:) |
| eu-west-1 | [EU West (Ireland)](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:) |

- Launch an EC2 instance with the following configuration details. If a value isn't specified below, accept the default value. Create one EC2 instance per table below.

| Configuration detail | Value |
| :--- | :--- 
| Amazon Machine Image (AMI) | Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type |
| |
| Instance Type | c5.2xlarge |
| |
| Number of instances | 1 |
| Network | Select the VPC Id created in the prerequisites section (to verify the the VPC Id, view the output of the AWS CloudFormation stack) |
| |
| Add tags | Key=Name; Value=Amazon Linux - FSx Workshop  |
| |
| Security group | Select the default VPC security group  |
| |
| EC2 key pair | Select an existing EC2 key pair you have access to  |

- Launch the instance

### Step 2.3: Launch two Amazon WorkSpaces

- Click on the link below to log in to the Amazon WorkSpaces Console in the same AWS region where you launched your CloudFormation stack.

| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/workspaces/home?region=us-east-1#addworkspaces:addworkspaces) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/workspaces/home?region=us-east-2#addworkspaces:addworkspaces) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/workspaces/home?region=us-west-2#addworkspaces:addworkspaces) |
| eu-west-1 | [EU West (Ireland)](https://console.aws.amazon.com/workspaces/home?region=eu-west-1#addworkspaces:addworkspaces) |

- Launch the WorkSpaces with the following configuration details. If a value isn't specified below, accept the default value. Create one EC2 instance per table below.

| Directory | Value |
| :--- | :--- 
| Directory | Select the Directory Id created in the prerequisites section (to verify the the Directory Id, view the output of the AWS CloudFormation stack) |
| Enable Self Service Permissions | Yes |

- Create a user with the following details.

| Detail | Value |
| :--- | :--- |
| Username | shawn |
| First Name | Shawn |
| Last Name | Spencer |
| Email | Enter an email address you have access to |

- Click **+Create Additional Users**
- Create a second user with the following details.

| Detail | Value |
| :--- | :--- |
| Username | burton |
| First Name | Burton |
| Last Name | Guster |
| Email | Enter an email address you have access to (this can be the same email you used above) |

- Click **Create Users**
- Click **Next Step**

- Assign WorkSpaces bundle with the following details.

| Username | Bundle |
| :--- | :--- |
| <directory>\burton | Standard with Windows 10 |
| <directory>\shawn | Standard with Amazon Linux 2 |

- Click **Next Step**
- Change the **Running Mode** to **Always On**
- Click **Next Step**
- Click **Launch WorkSpaces**


---
## Next section
### Click on the link below to go to the next section

| [**Map file share**](../3-map-file-share) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.
