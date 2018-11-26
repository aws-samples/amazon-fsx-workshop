![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Create file system

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Create file system

You must first complete [**Prerequisites**](../0-prerequisites) before continuing with this workshop.

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 1.1: Install the latest AWS CLI

- Install the latest AWS CLI on your laptop. For more details on how to install the latest AWS CLI, please refer to the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/installing.html). 


### Step 1.2: Create an FSx for Lustre file system

- Copy the script below into your favorite text editor

```sh
subnet=<subnet>
bucket=nasanex
region=<region>


aws fsx create-file-system \
--file-system-type LUSTRE \
--storage-capacity 3600 \
--subnet-ids ${subnet} \
--lustre-configuration ImportPath=s3://${bucket} \
--region ${region} \
--output json
```

- Replace the variable values <subnet_id> and <region> with the appropriate values for your environment. If you completed the prerequisites section, look at the output of the CloudFormation stack to find the subnet_id value. Use the same region where you created your VPC. We highly recommend you use the "nasanex" bucket. NASA NEX is a part of the **Registry of Open Data on AWS** project and is a collection of Earth science datasets maintained by NASA, including climate change projections and satellite images of the Earth's surface. We will be using some objects in this bucket for some of the other workshop sections. If you have another data repository you would like to use during the workshop, please replace the "nasanex" variable value with the bucket name and prefix (e.g. mybucket/prefix).

- Run the script, with the updated variable values, from the command line.

---
## Next section
### Click on the link below to go to the next section

| [**Create dashboard**](../2-create-dashboard) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.
