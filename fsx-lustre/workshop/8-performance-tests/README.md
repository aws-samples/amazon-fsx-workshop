![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Performance tests

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---
### Prerequisites

* An AWS account with administrative level access
* An Amazon FSx for Lustre file system
* An Amazon EC2 instance
* An Amazon EC2 key pair

If a key pair has not been previously created in your account, please refer to [Creating a Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) from the AWS EC2 User's Guide.  

Verify that the key pair is created in the same AWS region you will use for the tutorial.

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and executing the scripts included in this workshop. Delete all AWS resources created during this workshop so you don’t continue to incur additional compute and storage charges.

---

### Performance tests

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Mount the file system**](../4-mount-file-system)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 9.1: Log on to the Linux EC2 instance

- From the Amazon EC2 Console, select the **Lustre client - FSx Workshop** instance
- Click **Connect**
- Use the connection information to SSH to the instance using your laptop's terminal application

### Step 9.2: Performance tests

> Complete the following steps in your SSH session connected to the **Amazon Linux - FSx Workshop** instance

- Use smallfile to write, read, stat, append, rename, and delete a large number of smallfiles. Run these commands in order and review the results.

- Write (create) 32,000 4KiB files

```sh
job_name=$(echo $(uuidgen)| grep -o ".\{6\}$")
prefix=$(echo $(uuidgen)| grep -o ".\{6\}$")
path=/mnt/fsx/${job_name}
sudo mkdir -p ${path}

threads=64
file_size=4
file_count=500
operation=create
same_dir=N

sudo python ~/smallfile/smallfile_cli.py \
--operation ${operation} \
--threads ${threads} \
--file-size ${file_size} \
--files ${file_count} \
--same-dir ${same_dir} \
--hash-into-dirs Y \
--prefix ${prefix} \
--dirs-per-dir ${file_count} \
--files-per-dir ${file_count} \
--top ${path}

```

- Read 32,000 4KiB files

```sh
operation=read

sudo python ~/smallfile/smallfile_cli.py \
--operation ${operation} \
--threads ${threads} \
--file-size ${file_size} \
--files ${file_count} \
--same-dir ${same_dir} \
--hash-into-dirs Y \
--prefix ${prefix} \
--dirs-per-dir ${file_count} \
--files-per-dir ${file_count} \
--top ${path}

```

- Stat 32,000 4KiB files

```sh
operation=stat

sudo python ~/smallfile/smallfile_cli.py \
--operation ${operation} \
--threads ${threads} \
--file-size ${file_size} \
--files ${file_count} \
--same-dir ${same_dir} \
--hash-into-dirs Y \
--prefix ${prefix} \
--dirs-per-dir ${file_count} \
--files-per-dir ${file_count} \
--top ${path}

```

- Append 4KiB to 32,000 4KiB files

```sh
operation=append
file_size=4

sudo python ~/smallfile/smallfile_cli.py \
--operation ${operation} \
--threads ${threads} \
--file-size ${file_size} \
--files ${file_count} \
--same-dir ${same_dir} \
--hash-into-dirs Y \
--prefix ${prefix} \
--dirs-per-dir ${file_count} \
--files-per-dir ${file_count} \
--top ${path}

```

- Rename 32,000 4KiB files

```sh
operation=rename

sudo python ~/smallfile/smallfile_cli.py \
--operation ${operation} \
--threads ${threads} \
--file-size ${file_size} \
--files ${file_count} \
--same-dir ${same_dir} \
--hash-into-dirs Y \
--prefix ${prefix} \
--dirs-per-dir ${file_count} \
--files-per-dir ${file_count} \
--top ${path}

```

- Delete 32,000 4KiB files

```sh
operation=delete-renamed

sudo python ~/smallfile/smallfile_cli.py \
--operation ${operation} \
--threads ${threads} \
--file-size ${file_size} \
--files ${file_count} \
--same-dir ${same_dir} \
--hash-into-dirs Y \
--prefix ${prefix} \
--dirs-per-dir ${file_count} \
--files-per-dir ${file_count} \
--top ${path}

```


- Use dd to generate 128 GiB of data using 16 threads and a 1MiB block size

```sh
job_name=$(echo $(uuidgen)| grep -o ".\{6\}$")
bs=1024K
count=8192
sync=oflag=sync
threads=16

sudo mkdir -p /mnt/fsx/${job_name}/{1..16}

time seq 1 ${threads} | parallel --will-cite -j ${threads} sudo dd if=/dev/zero of=/mnt/fsx/${job_name}/{}/dd-$(date +%Y%m%d%H%M%S.%3N) bs=${bs} count=${count} ${sync} &

```

- While the above dd operation is running, monitor network throughput for ~30 seconds using **nload**. Use Ctrl+Z to exit nload.


```sh
nload -u M

```

- Monitor throughput and operations per second using the CloudWatch dashboard you created earlier.
- How long did it take to generate 128 GiB of data?
- Calculate the average throughput for this test?


- Use IOR to generate read & write load against the file system. Review the results at the end of the test. Remember to sum up the results of both threads to get the aggregate throughput to the file system.

```sh
cd /mnt/fsx
time seq 1 2 | parallel --will-cite -j2 'sudo ior -b 32g -t 8m -w -r -F -B -o /mnt/fsx/test{}.txt' &

```

- Monitor throughput using nload and the CloudWatch dashboard.


## Tear-down
### Delele all the resources you created in this workshop

- Terminate the EC2 instance
- Delete the FSx for Lustre file system from the FSx Console
- Delete the CloudFormation stack


---
For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.


