![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Lazy load

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Lazy load

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Mount the file system**](../4-mount-file-system)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 6.1: Log on to the Linux EC2 instance

- From the Amazon EC2 Console, select the **Lustre client - FSx Workshop** instance
- Click **Connect**
- Use the connection information to SSH to the instance using your laptop's terminal application

### Step 6.2: Lazy load some files

> Complete the following steps SSH'd in to the **Lustre client - FSx Workshop** instance

- These commands assume you linked the file system to the entire NASA NEX bucket (s3://nasanex). If you selected a specific prefix from this bucket or used a different bucket, you'll need to adjust your examination criteria based on your dataset.

- Based on your examination of the file system in the previous section, you know that no object data has been loaded into the file system. Only metadata has been loaded at this time. There is no need to per-warm a file system with data. You access the file system like any other POSIX compliant file system. When a file's is first accessed, the file's data will be lazy loaded into the file system which leads to slightly higher access times. But once the file has been loaded into the file system, you have super fast access to it. In this section of the workshop you will lazy load a few files multiple times to understand the different performance characteristics between first and subsequent access and how to load and release individual file data from the file system. Releasing the file data from the file system isn't deleting the file. The metadata resides in the file system but the data has been removed. If you need to access the file again, upon first access it will be loaded from the linked S3 bucket but subsequent access will be served from the file system until it is released.


- Generate a list of some .tif files

```sh
time lfs find /mnt/fsx --type f --name *.tif | head 100
```

- Copy the full path of one of the .tif files and paste it into the commands below, replacing the <file> placeholder.

- First, verify the data of this file isn't loaded into the file system. Run the command below to get the state of the file.

```sh
time lfs hsm_state <file> # time lfs hsm_state /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif
```

- My results are below. The file's state is **"released"** or not loaded.

```sh
[ec2-user@ip-10-0-0-210 ~]$ time lfs hsm_state /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif
/mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif: (0x0000000d) released exists archived, archive_id:1
```

- How large is the file? Run an ls -la against the file to find out the size.

```sh
time ls -lah <file> # time ls -lah /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif
```

- My results are below. The file I'm using is 183MB.
```sh
-rwxr-xr-x. 1 root root 183M Aug 15  2014 /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif
```

- Read the file and measure the time it takes to load it from the linked S3 bucket. This command will load the file by reading it from S3 and writing it to tempfs.

```sh
time cat <file> >/dev/shm/fsx # time cat /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif >/dev/shm/fsx
```

- How long did it take? My results are below.

```sh
real	0m4.702s
user	0m0.000s
sys	    0m0.152s
```

- Now run the same **"time cat..."** command again and see how long it takes? My results are below.

```sh
real	0m0.110s
user	0m0.000s
sys	    0m0.108s
```

- If I think the file system returned the same results that fast, would I be **right** or **wrong**?
- If you guessed **wrong** you are **right**. The data was cached on my EC2 instance. Lets flush the cache and run the command again, this time getting the data from the FSx for Lustre file system.
- Run this command to flush cache

```sh
sudo su
sync && echo 3 > /proc/sys/vm/drop_caches
exit

```

- Now run the same **"time cat..."** command again and see how long it takes? My results are below.

```sh
real	0m0.389s
user	0m0.000s
sys	    0m0.170s
```

- Run the **lfs hsm_state** command again to verify the file's data has been loaded into the file system.

```sh
time lfs hsm_state <file> # e.g. time lfs hsm_state /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif
```

- My results are below. See the difference? This file is no longer **released** so the data has been loaded.

```sh
[ec2-user@ip-10-0-0-210 ~]$ time lfs hsm_state /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif
/mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif: (0x00000009) exists archived, archive_id:1
```

- Run the **lfs df -h** command again to see now how much data is being used?

```sh
time lfs df -h

```

- How much data has been loaded into the file system now?
- Where is your file?

- My results are below.

```sh
UUID                       bytes        Used   Available Use% Mounted on
fsx-MDT0000_UUID          102.8G        4.4G       98.5G   4% /mnt/fsx[MDT:0]
fsx-OST0000_UUID            1.1T        4.5M        1.1T   0% /mnt/fsx[OST:0]
fsx-OST0001_UUID            1.1T        4.5M        1.1T   0% /mnt/fsx[OST:1]
fsx-OST0002_UUID            1.1T      187.0M        1.1T   0% /mnt/fsx[OST:2]

filesystem_summary:         3.3T      196.0M        3.3T   0% /mnt/fsx
```

- I can release the file (NOT remove or delete it), just release the data storage it's consuming and keep the file's metadata in the file system.
- Run the **lfs hsm_release** command below to release the file's data and free up storage.

```sh
time lfs hsm_release <file> # e.g. time lfs hsm_release /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif

```

- Run the **lfs df -h** command again to see now how much data is being used? It may take a few seconds for the data to be released.
 
```sh
time lfs df -h

```
- My results are below.

```sh
UUID                       bytes        Used   Available Use% Mounted on
fsx-MDT0000_UUID          102.8G        4.4G       98.5G   4% /mnt/fsx[MDT:0]
fsx-OST0000_UUID            1.1T        4.5M        1.1T   0% /mnt/fsx[OST:0]
fsx-OST0001_UUID            1.1T        4.5M        1.1T   0% /mnt/fsx[OST:1]
fsx-OST0002_UUID            1.1T        4.5M        1.1T   0% /mnt/fsx[OST:2]

filesystem_summary:         3.3T       13.5M        3.3T   0% /mnt/fsx
```

- Access the same file again and see how long it takes to access it.

```sh
time cat <file> >/dev/shm/fsx # time cat /mnt/fsx/NAIP/ca_1m_2012/36117/m_3611718_nw_11_1_20120623.tif >/dev/shm/fsx
```

- My results are below.

```sh
real	0m3.854s
user	0m0.000s
sys	    0m0.167s
```

- And subsequent reads show my file is loaded into the file system and cached on my client.

- Experiment with different file sizes and file types, walking through all the steps above.


---
## Next section
### Click on the link below to go to the next section

| [**Bulk load**](../7-bulk-load) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.


