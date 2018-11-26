![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon FSx for Windows File Server**

## WorkSpace access

### Version 2018.11

fsx.w.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### WorkSpace access

You must first complete [**Prerequisites**](../0-prerequisites) and the previous step [**Create a file system**](../2-launch-clients)

WARNING!! This workshop environment will exceed your free-usage tier. You will incur charges as a result of building this environment and completing the steps below.

### Step 8.1: Log on to the Windows WorkSpace

- You should have received an email from @...amazonworkspaces.com with login information for the two WorkSpaces you created earlier in the Workshop.
- Read the email related to the Windows WorkSpace (for the username **burton**)
- Complete the steps to login to the Windows WorkSpace

### Step 8.2: Copy the DNS name of the FSx for Windows file system

- From the Amazon FSx Console select the file system you created in the **Create a file system** section.
- Click the **Network & Security** tab.
- Copy the **DNS name** of the file system

### Step 8.3: Map one of the file system's file shares

> Complete the following steps logged on to the **Windows WorkSpace**

- Open **File Explorer**
- Context-click **This PC** and click **Map network drive...**
- Map the file share using the following information, 

| Configuraiton detail | Value 
| :--- | :--- 
| Drive | Z:
| Folder | UNC path of one of the file system's file shares you created earlier - using the DNS name you copied above - **\\\\<file system's DNS name>\share** - (e.g. **\\\\fs-0123456789abcdef.example.com\share**)
| Reconnect at sign-in | Leave **checked**
| Connect using different credentials | Leave **unchecked**

### Step 8.4: Access a file share

> Complete the following steps logged on to the **Windows WorkSpace**

- In the **File Explorer** window of the **Z:**
- Create new empty files on the **Z:** drive
- Context-click >> **New** >> **Text Document**
- Create a few different types of files

### Step 8.5: Test the performance of the new file share

> Complete the following steps logged on to the **Windows WorkSpace**

- Open a **PowerShell** window as an **Administrator**
- Install DiskSpeed using the script below. Copy >> Paste >> Execute the script in the PowerShell window

```sh
$path = "C:\Tools\DiskSpd-2.0.21a"
$url = "https://gallery.technet.microsoft.com/DiskSpd-A-Robust-Storage-6ef84e62/file/199535/2/DiskSpd-2.0.21a.zip"
$destination = "C:\Tools\DiskSpd-2.0.21a.zip"
$download = New-Object -Typename System.Net.WebClient
New-Item -Type Directory -Path $path
$download.DownloadFile($url,$destination)

$extract = New-Object -ComObject Shell.Application
$files = $extract.Namespace($destination).Items()
$extract.NameSpace($path).CopyHere($files)

```

- Open a new **PowerShell** window **NOT** as **Administrator**
- Run the DiskSpeed script below to test write performance of the mapped **Z:** drive

```sh
C:\Tools\DiskSpd-2.0.21a\amd64\DiskSpd.exe -b16K -c4G -o4 -t8 -w25 -r -L -d30 -Z1G Z:\${env:computername}.dat
```

- While the script is running, open **Task Explorer** and monitor network performance (e.g. Task Explorer >> Performance (tab) >> Ethernet)

- What was the peak write throughput you achieved?
- What was the peak read throughput you achieved?
- What was the P99 (99th %-tile) of your test?
- How is this different from the Windows instance test you ran eariler?

- Experiment with different DiskSpd parameter settings. Use the table below was a guide. Test with different block sizes (-b), file sizes (-c), number of outstanding I/O requests (-o), number of threads per file (-t), and read/write ratio (-w).

| Parameter | Description 
| :--- | :--- 
| `-b<size>[K\|M\|G]` | Block size in bytes or KiB, MiB, or GiB (default = 64K). |
| `-c<size>[K\|M\|G\|b]` | Create files of the specified size. Size can be stated in bytes or KiBs, MiBs, GiBs, or blocks. |
| `-o<count>` | Number of outstanding I/O requests per-target per-thread. (1 = synchronous I/O, unless more than one thread is specified with by using `-F`.) (default = 2) |
| `-t<count>` | Number of threads per target. Conflicts with `-F`, which specifies the total number of threads. |
| `-w<percentage>` | Percentage of write requests to issue (default = 0, 100% read). The following are equivalent and result in a 100% read-only workload: omitting `-w`, specifying `-w` with no percentage and `-w0`. **CAUTION**: A write test will destroy existing data without issuing a warning. |

- What different parameters did you test?
- How did the different parameter options alter the results?


---
## Next section
### Click on the link below to go to the next section

| [**Setup DFS**](../9-setup-dfs) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).

## License

This library is licensed under the Amazon Software License.

