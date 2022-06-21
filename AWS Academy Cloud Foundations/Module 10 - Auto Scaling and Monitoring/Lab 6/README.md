<header>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.5.0/css/all.css" integrity="sha384-B4dIYHKNBt8Bc12p+WXckhzcICo0wtJAoU8YZTY5qE0Id1GSseTk6S+L3BlXeVIU" crossorigin="anonymous">
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <!-- Optional theme -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
    <!-- Latest compiled and minified JavaScript -->
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
 </header>
 <!--include:Logo-->
 <style type="text/css">
    body {
    font-family:  "Roboto", "Helvetica", sans-serif;
    font-size: 12pt;
    font-color: Gray;
    line-height: 1.6;
    margin: 50px;
    }
    p {
    list-style-position: inside;
    }
    #ssb_blue {
    background-color: #257ACF;
    font-weight: bold;
    font-size: 90%;
    color: white;
    border-radius: 5px;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
    }
    #ssb_s3_blue {
    background-color: #329ad6;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_voc_grey {
    background-color: #F2F3F4;
    font-weight: normal;
    font-size: 90%;
    color: black;
    border-radius: 3px;
    border: 1px solid gray;
    padding-top: 5px;
    padding-bottom: 5px;
    padding-left: 6px;
    padding-right: 6px;
    white-space: nowrap;
    }
    #ssb_grey {
    background-color: #DEDEDE;
    font-weight: bold;
    font-size: 90%;
    color: #444;
    position: relative;
    top:-1px;
    border-radius: 5px;
    border-width: 1px;
    border-style: solid;
    border-color: #444;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
    }
    #ssb_white {
    background-color: white;
    font-weight: bold;
    font-size: 90%;
    color: #545b64;
    position: relative;
    top: -1px;
    border-color: #545b64;
    border-radius: 2px;
    border-width: 1px;
    border-style: solid;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
    }
    #ssb_services {
    background-color: #232f3e;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_orange {
    background-color:#ec7211;
    font-weight:bold;
    font-size:90%;
    color:white;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    white-space:
    nowrap;
    }
    #ssbox_cloudformation_blue {
    font-weight:bold;
    background-color:#f1faff;
    font-size:90%;
    border-color:#00A1C9;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
    #ssb_ssm_white {
    background-color:white;
    font-weight:bold;
    font-size:90%;
    color:#545b64;
    border-color:#545b64;
    border-radius:2px;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
 </style>


# Lab 6: Scale and Load Balance Your Architecture

<!-- Note to translators: This is based on Technical Essentials Lab 3. Copy the translation from there. Do not re-translate the whole document. -->

&nbsp;&nbsp;

**Version 4.6.6 (TESS3) + custom change**

This lab walks you through using the Elastic Load Balancing (ELB) and Auto Scaling services to load balance and automatically scale your infrastructure.

**Elastic Load Balancing** automatically distributes incoming application traffic across multiple Amazon EC2 instances. It enables you to achieve fault tolerance in your applications by seamlessly providing the required amount of load balancing capacity needed to route application traffic.

**Auto Scaling** helps you maintain application availability and allows you to scale your Amazon EC2 capacity out or in automatically according to conditions you define. You can use Auto Scaling to help ensure that you are running your desired number of Amazon EC2 instances. Auto Scaling can also automatically increase the number of Amazon EC2 instances during demand spikes to maintain performance and decrease capacity during lulls to reduce costs. Auto Scaling is well suited to applications that have stable demand patterns or that experience hourly, daily, or weekly variability in usage.  

&nbsp;

**Objectives**

After completing this lab, you can:

- Create an Amazon Machine Image (AMI) from a running instance.
- Create a load balancer.
- Create a launch configuration and an Auto Scaling group.
- Automatically scale new instances within a private subnet
- Create Amazon CloudWatch alarms and monitor performance of your infrastructure.

&nbsp;

**Duration**

This lab takes approximately **30 minutes**.

&nbsp;

**Scenario**

You start with the following infrastructure:

<img src="../../images/starting-architecture.png" alt="Starting architecture" width="800">

&nbsp;

The final state of the infrastructure is:

&nbsp;

<img src="../../images/final-architecture.png" alt="Final architecture" width="800">

&nbsp;

&nbsp;
___
## Accessing the AWS Management Console

1. At the top of these instructions, click <span id="ssb_voc_grey">Start Lab</span> to launch your lab.

    A Start Lab panel opens displaying the lab status.

2. Wait until you see the message "**Lab status: in creation**", then click the **X** to close the Start Lab panel.

    **Note**: It may take approximately 10 minutes or longer for the lab status to change to ready.

3. At the top of these instructions, click <span id="ssb_voc_grey">AWS</span>

    This will open the AWS Management Console in a new browser tab. The system will automatically log you in.

    **Tip**: If a new browser tab does not open, there will typically be a banner or icon at the top of your browser indicating that your browser is preventing the site from opening pop-up windows. Click on the banner or icon and choose "Allow pop ups."

4. Arrange the AWS Management Console tab so that it displays along side these instructions. Ideally, you will be able to see both browser tabs at the same time, to make it easier to follow the lab steps.

&nbsp;
___
## Task 1: Create an AMI for Auto Scaling

In this task, you will create an AMI from the existing _Web Server 1_. This will save the contents of the boot disk so that new instances can be launched with identical content.

5. In the **AWS Management Console**, on the <span id="ssb_services"><i class="fas fa-th"></i> Services </span> menu, click **EC2**.

6. In the left navigation pane, click **Instances**.

    First, you will confirm that the instance is running.

7. Wait until the **Status Checks** for **Web Server 1** displays *2/2 checks passed*. Click refresh <i class="fas fa-sync"></i> to update.

    You will now create an AMI based upon this instance.

8. Select <i class="far fa-check-square"></i> **Web Server 1**.

9. In the <span id="ssb_white">Actions <i class="fas fa-angle-down"></i></span> menu, click **Image and templates** &gt; **Create image**, then configure:

   - **Image name:** `WebServerAMI`
   - **Image description:** `Lab AMI for Web Server`

10. Click <span id="ssb_orange">Create image</span>

   A confirmation banner displays the **AMI ID** for your new AMI.

   You will use this AMI when launching the Auto Scaling group later in the lab.


&nbsp;
___
## Task 2: Create a Load Balancer

In this task, you will create a load balancer that can balance traffic across multiple EC2 instances and Availability Zones.

11. In the left navigation pane, choose **Target Groups**.

    **Analysis**: _Target Groups_ define where to *send* traffic that comes into the Load Balancer. The Application Load Balancer can send traffic to multiple Target Groups based upon the URL of the incoming request, such as having requests from mobile apps going to a different set of servers. Your web application will use only one Target Group.

    - Choose a target type: **Instances**
    - **Target group name**, enter: `LabGroup`
    - Select **Lab VPC** from the **VPC** drop-down menu.

12. Choose <span id="ssb_orange">Next</span>. The **Register targets** screen appears.

    Note: _Targets_ are the individual instances that will respond to requests from the Load Balancer.

    You do not have any web application instances yet, so you can skip this step.

11. Review the settings and choose <span id="ssb_orange">Create target group</span>

11. In the left navigation pane, click **Load Balancers**.

12. At the top of the screen, choose <span id="ssb_blue">Create Load Balancer</span>.

    Several different types of load balancer are displayed. You will be using an _Application Load Balancer_ that operates at the request level (layer 7), routing traffic to targets — EC2 instances, containers, IP addresses and Lambda functions — based on the content of the request. For more information, see: <a href="https://aws.amazon.com/elasticloadbalancing/features/#compare" target="_blank">Comparison of Load Balancers</a>

13. Under **Application Load Balancer**, choose <span id="ssb_white">Create</span>

13. Under **Load balancer name**, enter: `LabELB`

14. Scroll down to the **Network mapping** section, then:

    - For **VPC**, select: **Lab VPC**

      You will now specify which _subnets_ the Load Balancer should use. The load balancer will be internet facing, so you will select both Public Subnets.

    - Choose the **first** displayed Availability Zone, then select **Public Subnet 1** from the Subnet drop down menu that displays beneath it.

    - Choose the **second** displayed Availability Zone, then select **Public Subnet 2** from the Subnet drop down menu that displays beneath it.

      You should now have two subnets selected: **Public Subnet 1** and **Public Subnet 2**. (If not, go back and try the configuration again.)

15. In the **Security groups** section:

    - Choose the Security groups drop down menu and select <i class="far fa-check-square"></i> **Web Security Group**

    - Below the drop down menu, choose the **X** next to the default security group to remove it.

      The **Web Security Group** security group should now be the only one that appears.

23. For the Listener HTTP:80 row, set the Default action to forward to **LabGroup**.

21. Scroll to the bottom and choose <span id="ssb_orange">Create load balancer</span>

    ​	The load balancer is successfully created.

    - Choose <span id="ssb_orange">View load balancer</span>

    The load balancer will show a state of _provisioning_. There is no need to wait until it is ready. Please continue with the next task.

&nbsp;
___
## Task 3: Create a Launch Configuration and an Auto Scaling Group

In this task, you will create a _launch configuration_ for your Auto Scaling group. A launch configuration is a template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances such as the AMI, the instance type, a key pair, security group and disks.

22. In the left navigation pane, click **Launch Configurations**.

23. Click <span id="ssb_orange">Create launch configuration</span>

24. Configure these settings:

    - **Launch configuration name:** `LabConfig`

    - **Amazon Machine Image (AMI)** Choose *Web Server AMI*

    - **Instance type:** 

      - Choose <span id="ssb_white">Choose instance type</span>
      - Select  *t3.micro*
      - Choose <span id="ssb_orange">Choose</span>

      **Note:** If you have launched the lab in the us-east-1 Region, select the **t2.micro** instance type. To find the Region, look in the upper right-hand corner of the Amazon EC2 console.

      **Note:** If you receive the error message "Something went wrong. Please refresh and try again.", you may ignore it and continue with the exercise. 

    - **Additional configuration**

      - **Monitoring:**  </i> Select <i class="far fa-check-square"></i> *Enable EC2 instance detailed monitoring within CloudWatch*
      
      This allows Auto Scaling to react quickly to changing utilization.

25. Under **Security groups**, you will configure the launch configuration to use the _Web Security Group_ that has already been created for you.

    - Choose **Select an existing security group**
    - Select <i class="far fa-check-square"></i> **Web Security Group**

26. Under **Key pair** configure:

    - **Key pair options:** *Choose an existing key pair*
    - **Existing key pair:** vockey
    - Select <i class="far fa-check-square"></i> **I acknowledge...**
    - Click <span id="ssb_orange">Create launch configuration</span>

    You will now create an Auto Scaling group that uses this Launch Configuration.

27. Select the checkbox for the *LabConfig* Launch Configuration.

28. From the <span id="ssb_white">Actions<i class="fas fa-caret-down"></i></span> menu, choose *Create Auto Scaling group*

29. Enter Auto Scaling group name:

    - **Name:** `Lab Auto Scaling Group`
    
30. Choose <span id="ssb_orange">Next</span>

31. On the **Network** page configure

    - **Network:** _Lab VPC_

      <i class="fas fa-comment"></i> You can ignore the message regarding "No public IP address"

    - **Subnet:** Select _Private Subnet 1 (10.0.1.0/24)_ **and** _Private Subnet 2 (10.0.3.0/24)_

    This will launch EC2 instances in private subnets across both Availability Zones.

32. Choose <span id="ssb_orange">Next</span>

33. In the **Load balancing - *optional*** pane, choose **Attach to an existing load balancer**

34. In the **Attach to an existing load balancer** pane, use the dropdown list to select *LabGroup*.

35. In the **Additional settings - *optional*** pane, select <i class="far fa-check-square"></i> **Enable group metrics collection within CloudWatch**

    This will capture metrics at 1-minute intervals, which allows Auto Scaling to react quickly to changing usage patterns.

36. Choose <span id="ssb_orange">Next</span>

37. Under **Group size**, configure: 

    - **Desired capacity:** 2
    - **Minimum capacity:** 2
    - **Maximum capacity:** 6

    This will allow Auto Scaling to automatically add/remove instances, always keeping between 2 and 6 instances running.

38. Under **Scaling policies**, choose *Target tracking scaling policy* and configure:

    - **Lab policy name:** `LabScalingPolicy`
    - **Metric type:** _Average CPU Utilization_
    - **Target value:** `60`

    This tells Auto Scaling to maintain an _average_ CPU utilization _across all instances_ at 60%. Auto Scaling will automatically add or remove capacity as required to keep the metric at, or close to, the specified target value. It adjusts to fluctuations in the metric due to a fluctuating load pattern.

39. Choose <span id="ssb_orange">Next</span>

    Auto Scaling can send a notification when a scaling event takes place. You will use the default settings.

40. Choose <span id="ssb_orange">Next</span>

    Tags applied to the Auto Scaling group will be automatically propagated to the instances that are launched.

41. Choose <span id="ssb_orange">Add tag</span> and Configure the following:

    - **Key:** `Name`
    - **Value:** `Lab Instance`

42. Click <span id="ssb_orange">Next</span>

43. Review the details of your Auto Scaling group, then click <span id="ssb_orange">Create Auto Scaling group</span>. If you encounter an error **Failed to create Auto Scaling group**, then click <span id="ssb_blue">Retry Failed Tasks</span>.

    Your Auto Scaling group will initially show an instance count of zero, but new instances will be launched to reach the **Desired** count of 2 instances.

&nbsp;
___
## Task 4: Verify that Load Balancing is Working

In this task, you will verify that Load Balancing is working correctly.

44. In the left navigation pane, click **Instances**.

    You should see two new instances named **Lab Instance**. These were launched by Auto Scaling.

    <i class="fas fa-comment"></i> If the instances or names are not displayed, wait 30 seconds and click refresh <i class="fas fa-sync"></i> in the top-right.

    First, you will confirm that the new instances have passed their Health Check.

45. In the left navigation pane, click **Target Groups** (in the _Load Balancing_ section).

46. Choose *LabGroup*

47. Click the **Targets** tab.

    Two **Lab Instance** targets should be listed for this target group.

48. Wait until the **Status** of both instances transitions to *healthy*. Click Refresh <i class="fas fa-sync"></i> in the upper-right to check for updates.

    _Healthy_ indicates that an instance has passed the Load Balancer's health check. This means that the Load Balancer will send traffic to the instance.

    You can now access the Auto Scaling group via the Load Balancer.

49. In the left navigation pane, click **Load Balancers**.

50. In the lower pane, copy the **DNS name** of the load balancer, making sure to omit "(A Record)".

    It should look similar to: _LabELB-1998580470.us-west-2.elb.amazonaws.com_

51. Open a new web browser tab, paste the DNS Name you just copied, and press Enter.

    The application should appear in your browser. This indicates that the Load Balancer received the request, sent it to one of the EC2 instances, then passed back the result.

&nbsp;
___
## Task 5: Test Auto Scaling

You created an Auto Scaling group with a minimum of two instances and a maximum of six instances. Currently two instances are running because the minimum size is two and the group is currently not under any load. You will now increase the load to cause Auto Scaling to add additional instances.

52. Return to the AWS management console, but do not close the application tab — you will return to it soon.

53. On the <span id="ssb_services"><i class="fas fa-th"></i> Services </span> menu, click **CloudWatch**.

54. In the left navigation pane, choose **All alarms**.

    Two alarms will be displayed. These were created automatically by the Auto Scaling group. They will automatically keep the average CPU load close to 60% while also staying within the limitation of having two to six instances.
    
    ​    <i class="fas fa-exclamation-triangle" style="color:red"></i> **Note**: Please follow these steps only if you do not see the alarms in 60 seconds.

    - On the <span id="ssb_services"><i class="fas fa-th"></i> Services </span> menu, click **EC2**.
    - In the left navigation pane, choose **Auto Scaling Groups**. 
    - Select <i class="far fa-check-square"></i> **Lab Auto Scaling Group**.
    - In the bottom half of the page, choose the **Automatic Scaling** tab.
    -  Select <i class="far fa-check-square"></i> **LabScalingPolicy**.
    - Click <span id="ssb_white">Actions <i class="fas fa-angle-down"></i></span> and **Edit**.
    - Change the **Target Value** to `50`.
    - Click <span id="ssb_orange">Update</span>
    - On the <span id="ssb_services"><i class="fas fa-th"></i> Services </span> menu, click **CloudWatch**.
    - In the left navigation pane, click **All alarms** and verify you see two alarms.



55. Click the **OK** alarm, which has _AlarmHigh_ in its name.

    <i class="fas fa-comment"></i> If no alarm is showing **OK**, wait a minute then click refresh <i class="fas fa-sync"></i> in the top-right until the alarm status changes.

    The **OK** indicates that the alarm has _not_ been triggered. It is the alarm for **CPU Utilization > 60**, which will add instances when average CPU is high. The chart should show very low levels of CPU at the moment.

    You will now tell the application to perform calculations that should raise the CPU level.

56. Return to the browser tab with the web application.

57. Click **Load Test** beside the AWS logo.

    This will cause the application to generate high loads. The browser page will automatically refresh so that all instances in the Auto Scaling group will generate load. Do not close this tab.

58. Return to browser tab with the **CloudWatch** console.

    In less than 5 minutes, the **AlarmLow** alarm should change to **OK** and the **AlarmHigh** alarm status should change to *In alarm*.

    <i class="fas fa-comment"></i> You can click Refresh <i class="fas fa-sync"></i> in the top-right every 60 seconds to update the display.

    You should see the **AlarmHigh** chart indicating an increasing CPU percentage. Once it crosses the 60% line for more than 3 minutes, it will trigger Auto Scaling to add additional instances.

59. Wait until the **AlarmHigh** alarm enters the _In alarm_ state.

    You can now view the additional instance(s) that were launched.

60. On the <span id="ssb_services"><i class="fas fa-th"></i> Services </span> menu, click **EC2**.

61. In the left navigation pane, click **Instances**.

    More than two instances labeled **Lab Instance** should now be running. The new instance(s) were created by Auto Scaling in response to the Alarm.

&nbsp;
___
## Task 6: Terminate Web Server 1

In this task, you will terminate _Web Server 1_. This instance was used to create the AMI used by your Auto Scaling group, but it is no longer needed.

62. Select <i class="far fa-check-square"></i> **Web Server 1** (and ensure it is the only instance selected).

63. In the <span id="ssb_white">Instance state <i class="fas fa-angle-down"></i></span> menu, click **Instance State** > **Terminate Instance**.

64. Choose <span id="ssb_orange">Terminate</span>

&nbsp;
___
## Lab Complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

65. Click <span id="ssb_voc_grey">End Lab</span> at the top of this page and then click <span id="ssb_blue">Yes</span> to confirm that you want to end the lab.  

    A panel will appear, indicating that "DELETE has been initiated... You may close this message box now."

66. Click the **X** in the top right corner to close the panel.

    For feedback, suggestions, or corrections, please email us at: *aws-course-feedback@amazon.com*
