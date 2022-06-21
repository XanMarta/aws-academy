# Module 4 - Guided Lab: Introducing Amazon Elastic File System (Amazon EFS)
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: SPL-151"

## Lab overview and objectives
This lab introduces you to Amazon Elastic File System (Amazon EFS) by using the AWS Management Console.

After completing this lab, you should be able to:

- Log in to the AWS Management Console

- Create an Amazon EFS file system

- Log in to an Amazon Elastic Compute Cloud (Amazon EC2) instance that runs Amazon Linux

- Mount your file system to your EC2 instance

- Examine and monitor the performance of your file system

<br/>
##Duration

This lab requires approximately **20 minutes** to complete.

<br/>
##AWS service restrictions

In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.

<br/>

## Accessing the AWS Management Console

1. At the top of these instructions, choose <span id="ssb_voc_grey">Start Lab</span> to launch your lab.

   A **Start Lab** panel opens, and it displays the lab status.

   <i class="fas fa-info-circle"></i> **Tip**: If you need more time to complete the lab, restart the timer for the environment by choosing the <span id="ssb_voc_grey">Start Lab</span> button again.

2. Wait until the **Start Lab** panel displays the message *Lab status: ready*, then close the panel by choosing the **X**.

3. At the top of these instructions, choose <span id="ssb_voc_grey">AWS</span>.

   This action opens the AWS Management Console in a new browser tab. The system automatically logs you in.

   <i class="fas fa-exclamation-triangle"></i> **Tip**: If a new browser tab does not open, a banner or icon is usually at the top of your browser with the message that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and then choose **Allow pop-ups**.

4. Arrange the **AWS Management Console** tab so that it displays alongside these instructions. Ideally, you will have both browser tabs open at the same time so that you can follow the lab steps more easily.

   <i class="fas fa-exclamation-triangle"></i> **Do not change the Region unless specifically instructed to do so**.

<br/>

## Task 1: Creating a security group to access your EFS file system

The security group that you associate with a mount target *must allow inbound access for TCP on port 2049 for Network File System (NFS)*. This is the security group that you will now create, configure, and attach to your EFS mount targets.


5. In the **AWS Management Console**, on the <span id="ssb_services">Services</span> menu, choose **EC2**.

6. In the navigation pane on the left, choose **Security Groups**.

7. Copy the **Security group ID** of the *EFSClient* security group to your text editor.

   The Group ID should look similar to *sg-03727965651b6659b*.

8. Choose <span id="ssb_orange">Create security group</span> then configure:

   <a id='securitygroup'></a>

    * **Security group name:** `EFS Mount Target`
    * **Description:** `Inbound NFS access from EFS clients`
    * **VPC:** *Lab VPC*

9. Under the **Inbound rules** section, choose <span id="ssb_white">Add rule</span> then configure:

    * **Type:** *NFS*
    * **Source:**
      * *Custom*
      * In the *Custom* box, paste the security group's **Security group ID** that you copied to your text editor
    * Choose <span id="ssb_orange">Create security group</span>.

<br/>

## Task 2: Creating an EFS file system

EFS file systems can be mounted to multiple EC2 instances that run in different Availability Zones in the same Region. These instances use *mount targets* that are created in each *Availability Zone* to mount the file system by using standard NFSv4.1 semantics. You can mount the file system on instances in only one virtual private cloud (VPC) at a time. Both the file system and the VPC must be in the same Region.


10. On the <span id="ssb_services">Services</span> menu, choose **EFS**.

11. Choose <span id="ssb_orange">Create file system</span>

12. In the **Create file system** window, choose <span id="ssb_white">Customize</span>

13. On **Step 1**:

    - Uncheck <i class="far fa-square"></i> Enable automatic backups.
    - **Lifecycle management:** Select *None*
    - In the **Tags** section, configure:
      - **Key:** `Name`
      - **Value:** `My First EFS File System`

14. Choose <span id="ssb_orange">Next</span>

15. For **VPC**, select *Lab VPC*.

16. Detach the default security group from each *Availability Zone* mount target by choosing the <i class="fas fa-times"></i> check box on each default security group.

17. Attach the **EFS Mount Target** security group to each *Availability Zone* mount target by:

   * Selecting each **Security groups** check box.
   * Choosing **EFS Mount Target**.

     A mount target is created for each subnet.

     Your mount targets should look like the following example. The diagram shows two mount targets in the **Lab VPC** that use the **EFS Mount Target** security group. In this lab, you should be using the Lab VPC.

     <img src="images/mount-targets-security-groups.png" alt="Target Security Groups" width="600" >

18. Choose <span id="ssb_orange">Next</span>

19. On **Step 3**, choose <span id="ssb_orange">Next</span>

20. On **Step 4:**

  * Review your configuration.
  * Choose <span id="ssb_orange">Create</span>

<i class="far fa-thumbs-up"></i> Congratulations! You have created a new EFS file system in your Lab VPC and mount targets in each Lab VPC subnet. In a few seconds, the **File system state** of the file system will change to *Available*, followed by the mount targets 2–3 minutes later.

Proceed to the next step after the **Mount target state** for each mount target changes to *Available*. Choose the screen refresh button after 2–3 minutes to check its progress.

<br/>

## Task 3: Connecting to your EC2 instance via SSH

In this task, you will connect to your EC2 instance by using Secure Shell (SSH).

###<i class="fab fa-windows"></i> Microsoft Windows users

<i class="fas fa-comment"></i> These instructions are specifically for Microsoft Windows users. If you are using macOS or Linux, <a href="#ssh-MACLinux">skip to the next section</a>.
​

21. Above these instructions that you are currently reading, choose the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>

   A **Credentials** window opens.

22. Choose the **Download PPK** button and save the **labsuser.ppk** file.

   **Note:** Typically, your browser saves the file to the **Downloads** directory.

23. Note the **EC2PublicIP** address, if it is displayed.

24. Exit the **Details** panel by choosing the **X**.

25. To use SSH to access the EC2 instance, you must use ***\*PuTTY\****. If you do not have PuTTY installed on your computer, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">download PuTTY</a>.

26. Open **putty.exe**.

27. To keep the PuTTY session open for a longer period of time, configure the PuTTY timeout:

   * Choose **Connection**
   * **Seconds between keepalives**: `30`

28. Configure your PuTTY session by using the following settings.

   * Choose **Session**
   * **Host Name (or IP address):** Paste the **EC2PublicIP** for the instance you noted earlier
     * Alternatively, return to the Amazon EC2 console and choose **Instances**
     * Select the instance you want to connect to
     * In the *Description* tab, copy the **IPv4 Public IP** value
   * Back in PuTTY, in the **Connection** list, expand <i class="far fa-plus-square"></i> **SSH**
   * Choose **Auth** (but don't expand it)
   * Choose **Browse**
   * Browse to the *labsuser.ppk* file that you downloaded, select it, and choose **Open**
   * Choose **Open** again


29. To trust and connect to the host, choose **Accept**.

30. When you are prompted with **login as**, enter: `ec2-user`

    This action connects you to the EC2 instance.

31. Microsoft Windows users: <a href="#ssh-after">Choose this link to skip ahead to the next task.</a>



<a id='ssh-MACLinux'></a>

### macOS <span style="font-size: 30px; color: #808080;"><i class="fab fa-apple"></i></span> and Linux <span style="font-size: 30px; "><i class="fab fa-linux"></i></span> users

These instructions are specifically for macOS or Linux users. If you are a Windows user, <a href="#ssh-after">skip ahead to the next task.</a>

32. Above these instructions that you are currently reading, choose the <span id="ssb_voc_grey">Details</span> dropdown menu, and then select <span id="ssb_voc_grey">Show</span>

    A **Credentials** window opens.

33. Choose the **Download PEM** button and save the **labsuser.pem** file.

34. Note the **EC2PublicIP** address, if it is displayed.

35. Exit the **Details** panel by choosing the **X**.

36. Open a terminal window, and change directory to the directory where the *labsuser.pem* file was downloaded by using the `cd` command.

    For example, if the *labsuser.pem* file was saved to your **Downloads** directory, run this command:

    ```bash
    cd ~/Downloads
    ```

37. Change the permissions on the key to be read-only, by running this command:

    ```bash
    chmod 400 labsuser.pem
    ```

38. Run the following command (replace **<public-ip\>** with the **EC2PublicIP** address that you copied earlier).

    * Alternatively, to find the IP address of the on-premises instance, return to the Amazon EC2 console and select **Instances**
    * Select the instance that you want to connect to
    * In the **Description** tab, copy the **IPv4 Public IP** value

     ```bash
     ssh -i labsuser.pem ec2-user@<public-ip>
     ```

39. When you are prompted to allow the first connection to this remote SSH server, enter `yes`.

    Because you are using a key pair for authentication, you are not prompted for a password.

<a id='ssh-after'></a>

<br/>
## Task 4: Creating a new directory and mounting the EFS file system

<i class="fas fa-info-circle" aria-hidden="true"></i> Amazon EFS supports the NFSv4.1 and NFSv4.0 protocols when it mounts your file systems on EC2 instances. Though NFSv4.0 is supported, we recommend that you use NFSv4.1. When you mount your EFS file system on your EC2 instance, you must also use an NFS client that supports your chosen NFSv4 protocol. The EC2 instance that was launched as a part of this lab includes an NFSv4.1 client, which is already installed on it.


40. Back in the **AWS Management Console**, on the <span id="ssb_services">Services</span> menu, choose **EFS**.

41. Choose **My First EFS File System**.

42. In the **Amazon EFS Console**, on the top right corner of the page, choose <span id="ssb_orange">Attach</span> to open the Amazon EC2 mount instructions.

43. In your SSH session, follow the instructions in the **To set up your EC2 instance** section. Copy and paste (or manually type) the command to install the required utilities.

    The command should look similar to this example:

    ```
    sudo yum install -y amazon-efs-utils
    ```

    *Tip:* to paste into the terminal in the browser, place your cursor just to the right of the command prompt and right-click to see the paste option.

44. Scroll down to the **Mounting your file system** section.

45. In your SSH session, make a new directory by entering `sudo mkdir efs`

46. Copy the entire command in the **Using the NFS client** section.

    The mount command should look similar to this example:

    `sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-bce57914.efs.us-west-2.amazonaws.com:/ efs`

    <i class="fas fa-comment" aria-hidden="true"></i> The provided `sudo mount...` command uses the default Linux mount options.

47. In your Linux SSH session, mount your Amazon EFS file system by:

    * Pasting the command
    * Pressing ENTER


48. Get a full summary of the available and used disk space usage by entering:

    `sudo df -hT`

    This following screenshot is an example of the output from the following *disk filesystem* command: 

    `df -hT`
    
    Notice the *Type* and *Size* of your mounted EFS file system.

<img src="images/disk-space.png" alt="disk space" width="600">


<br/>
## Task 5: Examining the performance behavior of your new EFS file system


### Examining the performance by using Flexible IO

<i class="fas fa-info-circle"></i> Flexible IO (fio) is a synthetic I/O benchmarking utility for Linux. It is used to benchmark and test Linux I/O subsystems. During boot, *fio* was automatically installed on your EC2 instance.

49. Examine the write performance characteristics of your file system by entering:

    ```
    sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio
    ```

    <i class="fas fa-comment"></i> The `fio` command will take 5–10 minutes to complete. The output should look like the example in the following screenshot. Make sure that you examine the output of your `fio` command, specifically the summary status information for this WRITE test.

    <img src="images/fio.png" alt="fio" width="600" >

<br/>
### Monitoring performance by using Amazon CloudWatch

50. In the **AWS Management Console**, on the <span id="ssb_services">Services</span> menu, choose **CloudWatch**.

51. In the navigation pane on the left, choose **Metrics**.

52. In the **All metrics** tab, choose **EFS**.

53. Choose **File System Metrics**.

54. Select the row that has the **PermittedThroughput** Metric Name.

    <i class="fas fa-comment"></i> You might need to wait 2–3 minutes and refresh the screen several times before all available metrics, including **PermittedThroughput**, calculate and populate.

55. On the graph, choose and drag around the data line. If you do not see the line graph, adjust the time range of the graph to display the period during which you ran the `fio` command.

    <img src="images/graph.png" alt="choose drag" width="600" >


56. Pause your pointer on the data line in the graph. The value should be *105M*.

    <img src="images/throughput.png" alt="Throughput" width="600" >
    
    
    The throughput of Amazon EFS scales as the file system grows. File-based workloads are typically spiky. They drive high levels of throughput for short periods of time, and low levels of throughput the rest of the time. Because of this behavior, Amazon EFS is designed to burst to high throughput levels for periods of time. All file systems, regardless of size, can burst to 100 MiB/s of throughput. For more information about performance characteristics of your EFS file system, see the official <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">Amazon Elastic File System documentation</a>.
    
57. In the **All metrics** tab, *uncheck* the box for **PermittedThroughput**.

58. Select the check box for **DataWriteIOBytes**.

    <i class="fas fa-comment"></i> If you do not see *DataWriteIOBytes* in the list of metrics, use the **File System Metrics** search to find it.

59. Choose the **Graphed metrics** tab.

60. On the **Statistics** column, select **Sum**.

61. On the **Period** column, select **1 Minute**.

62. Pause your pointer on the peak of the line graph. Take this number (in bytes) and divide it by the duration in seconds (60 seconds). The result gives you the write throughput (B/s) of your file system during your test.

    <img src="images/Sum-1-minute.png" alt="Sum 1 Minute" width="600" >

    The throughput that is available to a file system scales as a file system grows. All file systems deliver a consistent baseline performance of 50 MiB/s per TiB of storage. Also, all file systems (regardless of size) can burst to 100 MiB/s. File systems that are larger than 1T B can burst to 100 MiB/s per TiB of storage. As you add data to your file system, the maximum throughput that is available to the file system scales linearly and automatically with your storage.

    File system throughput is shared across all EC2 instances that are connected to a file system. For more information about performance characteristics of your EFS file system, see the official <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">Amazon Elastic File System documentation</a>.

    <i class="far fa-thumbs-up" style="color:blue"></i> Congratulations! You created an EFS file system, mounted it to an EC2 instance, and ran an I/O benchmark test to examine its performance characteristics.
    

<br/>

## Submitting your work

63. At the top of these instructions, choose <span id="ssb_blue">Submit</span> to record your progress and when prompted, choose <span id="ssb_blue">Yes</span>.

64. If the results don't display after a couple of minutes, return to the top of these instructions and choose <span id="ssb_voc_grey">Grades</span>

    **Tip**: You can submit your work multiple times. After you change your work, choose **Submit** again. Your last submission is what will be recorded for this lab.

65. To find detailed feedback on your work, choose <span id="ssb_voc_grey">Details</span> followed by <i class="fas fa-caret-right"></i> **View Submission Report**.


<br/>
## Lab complete <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Congratulations! You have completed the lab.


66. Choose <span id="ssb_voc_grey">End Lab</span> at the top of this page, and then select <span id="ssb_blue">Yes</span> to confirm that you want to end the lab.

    A panel should appear with this message: *DELETE has been initiated... You may close this message box now.*​

67. Select the **X** in the top right corner to close the panel.





*©2020 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*