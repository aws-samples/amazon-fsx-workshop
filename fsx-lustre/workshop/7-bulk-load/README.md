![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Bulk load

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Bulk load

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Mount the file system**](../4-mount-file-system)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 7.1: Log on to the Linux EC2 instance

- From the Amazon EC2 Console, select the **Lustre client - FSx Workshop** instance
- Click **Connect**
- Use the connection information to SSH to the instance using your laptop's terminal application

### Step 7.2: Bulk load files

> Complete the following steps SSH'd in to the **Lustre client - FSx Workshop** instance

- These commands assume you linked the file system to the entire NASA NEX bucket (s3://nasanex). If you selected a specific prefix from this bucket or used a different bucket, you'll need to adjust your examination criteria based on your dataset.

- Based on your examination of the file system in an earlier section, you know that object data is not loaded into the file system that's liked to an S3 bucket when the file system is created. Only metadata is loaded when the file system is created. There is no need to per-warm a file system with data. You access the file system like any other POSIX compliant file system. When a file is first accessed, the file's data is lazy loaded into the file system from the S3 bucket. While is this very convenient and requires not change to existing applications, you may decide to reduce latencies by loading file data prior to running your workload. FSx for Lustre allows you to bulk load data from a linked S3 bucket.

- Decide what data you want to bulk load. This could be by file type, size, subdirectory, or all of the above. Or you could have other criteria.

- Construct a find command to return the data you want to bulk load.

- Run the command below to load 2048 .tif files. This command uses GNU parallel to parallelize load (restore) commands to the file system. Data isn't loaded through Lustre client issuing the command. The parallel design of Lustre allows data to be loaded in parallel directly from S3 into the file system's object storage targets (OSTs). This results is fast, high throughput data loads. 

For more information about GNU Parallel, pleae refer to GNU Parallel - https://www.gnu.org/software/parallel/ - used to parallelize single-threaded commands; O. Tange (2018): GNU Parallel 2018, March 2018, https://doi.org/10.5281/zenodo.1146014

```sh
threads=128
time lfs find /mnt/fsx --type f --name *.tif | head -2048 | parallel --will-cite -j ${threads} sudo lfs hsm_restore {} &

```


- While this is running, verify data isn't being loaded through the client by running nload to monitor network throughput in real time on the client.  Monitor the throughput for a few seconds then exit nload by using Control+Z.

```sh
nload -u M
```

- Access the CloudWatch dashboard you created earlier and monitor file system performance during the data load.

- How much throughput are you achieving?  Read throughput? Write throughput? Total throughput?
- How many operations per second are you seeing? Reads? Writes? Metadata?

- Monitor storage capacity by running **lfs df** to view **Used** and **Available** storage for all the object storage servers (OSTs). Run this a few times to monitor its progress.

```sh
lfs df -h
```
- How much data was loaded?
- How much available storage is there?

- After a few minutes cancel the load by issues the following command

```sh
threads=128
time lfs find /mnt/fsx --type f --name *.tif | head -2048 | parallel --will-cite -j ${threads} sudo lfs hsm_cancel {} &

```

- After a few minutes the bulk load (hsm_restore) should stop.

- Verify the state of the .tif files by running the command below.
- Are they how many are loaded vs. how many are "released"?


```sh
threads=128
time lfs find /mnt/fsx --type f --name *.tif | head -2048 | parallel --will-cite -j ${threads} sudo lfs hsm_state {} &

```

- Run a final **df** to see how much data was loaded before you cancelled it.

```sh
lfs df -h
```


---
## Next section
### Click on the link below to go to the next section

| [**Performance tests**](../8-performance-tests) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.


