![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Lustre**

## Examine the file system

### Version 2018.11

fsx.l.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Examine the file system

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Mount the file system**](../4-mount-file-system).

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 5.1: Log on to the Linux EC2 instance

- From the Amazon EC2 Console, select the **Lustre client - FSx Workshop** instance
- Click **Connect**
- Use the connection information to SSH to the instance using your laptop's terminal application

### Step 5.2: Examine the file system's metadata loaded from the linked Amazon S3 bucket

> Complete the following steps SSH'd in to the **Lustre client - FSx Workshop** instance

- These commands assume you linked the file system to the entire NASA NEX bucket (s3://nasanex). If you selected a specific prefix from this bucket or used a different bucket, you'll need to adjust your examination criteria based on your dataset.

- How long did it take to load file metadata from the linked S3 bucket during file system creation?
**Hint** - Go to the CloudWatch dashboard created earlier and look at the **Operations per second (ops)** widget. Look at the start and end times of the heavy metadata operations period.

- What was the max operations per second (ops) during metadata load?
**Hint** - Go to the CloudWatch dashboard created earlier and look at the **Operations per second (ops)** widget. Move your cursor to find the max ops achieved.


- How long does it take to list the entire file system?

```sh
time lfs find /mnt/fsx
```

- What file types did you see?


- How many files?
```sh
time lfs find /mnt/fsx --type f | wc -l
```

- How many directories?
```sh
time lfs find /mnt/fsx --type d | wc -l
```

- How many small files (< 512 KiB)?
```sh
time lfs find /mnt/fsx --type f --size -512k | wc -l
```

- How many large files (> 100 MiB)?
```sh
time lfs find /mnt/fsx --type f --size +100M | wc -l
```

- How many .nc, .hdf, .tif, .gz files?
```sh
time lfs find /mnt/fsx --type f --name *.nc | wc -l
time lfs find /mnt/fsx --type f --name *.hdf | wc -l
time lfs find /mnt/fsx --type f --name *.tif | wc -l
time lfs find /mnt/fsx --type f --name *.gz | wc -l

```

- How much metadata (MDT) has been loaded into the file system?
- How much data (all the OSTs) has been loaded into the file system?
- How much data storage capacity is available?
```sh
time lfs df -h

```


### Step 5.3: Examination results

> The results of your queries above should match the following:


| Query | Results |
| :--- | :--- |
| How long did it take to load file metadata from the linked S3 bucket during file system creation? | ~14m |
| What was the max operations per second (ops) during metadata load? | 4,890 ops |
| How long does it take to list the entire file system? | ~1m18.229s |
| What file types did you see? | .hdf  .nc  .gz  .tif  .json  .md5  .txt  .pdf |
| How many files? | 373572 |
| How many directories? | 42242 |
| How many small files (< 512 KiB)? | 23692 |
| How many large files (> 100 MiB)? | 169617 |
| How many .nc, .hdf, .tif, .gz files? | .hdf = 207552 <br> .tif = 11095 <br> .nc = 87002 <br> .gz = 42009|
| How much metadata (MDT) has been loaded into the file system? | 4.4G |
| How much data (all the OSTs) has been loaded into the file system? | 13.5M | 
| How much data storage capacity is available? | 3.3T |


---
## Next section
### Click on the link below to go to the next section

| [**Lazy load**](../6-lazy-load) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.

