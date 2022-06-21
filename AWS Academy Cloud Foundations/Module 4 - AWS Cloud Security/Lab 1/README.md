
# Lab 1: Introduction to AWS IAM

<!-- Note to translators: This is based on SPL-66. Copy the translation from there. Do not re-translate the whole document. -->

<!-- Copied from Version 3.1.2 (spl66) -->

**AWS Identity and Access Management (IAM)** is a web service that enables Amazon Web Services (AWS) customers to manage users and user permissions in AWS. With IAM, you can centrally manage **users**, **security credentials** such as access keys, and **permissions** that control which AWS resources users can access.



## Topics covered

This lab will demonstrate:

<img src="images/lab-scenario.jpeg" alt="LabOverview" width="600" />

 * Exploring pre-created **IAM Users and Groups**
 * Inspecting **IAM policies** as applied to the pre-created groups
 * Following a **real-world scenario**, adding users to groups with specific capabilities enabled
 * Locating and using the **IAM sign-in URL**
 * **Experimenting** with the effects of policies on service access

&nbsp;
**Other AWS Services**

During this lab, you may receive error messages when performing actions beyond the steps in this lab guide. These messages will not impact your ability to complete the lab.

&nbsp;
**AWS Identity and Access Management**

AWS Identity and Access Management (IAM) can be used to:

  * **Manage IAM Users and their access:** You can create Users and assign them individual security credentials (access keys, passwords, and multi-factor authentication devices). You can manage permissions to control which operations a User can perform.

  * **Manage IAM Roles and their permissions:** An IAM Role is similar to a User, in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a Role is intended to be *assumable* by anyone who needs it.

  * **Manage federated users and their permissions:** You can enable *identity federation* to allow existing users in your enterprise to access the AWS Management Console, to call AWS APIs and to access resources, without the need to create an IAM User for each identity.

**Duration**

This lab takes approximately **40 minutes** to complete.

&nbsp;
&nbsp;
## Accessing the AWS Management Console

1. At the top of these instructions, choose <span id="ssb_voc_grey">Start Lab</span> to launch your lab.

    A Start Lab panel opens displaying the lab status. In the **Start Lab** dialog box that opens, note the AWS Region, as you will need to refer to it later in this lab.

    

2. Wait until you see the message "**Lab status: ready**", then click the **X** to close the Start Lab panel.

    

3. At the top of these instructions, choose <span id="ssb_voc_grey">AWS</span>

    This will open the AWS Management Console in a new browser tab. The system will automatically log you in.

    **Tip**: If a new browser tab does not open, there will typically be a banner or icon at the top of your browser indicating that your browser is preventing the site from opening pop-up windows. Click on the banner or icon and choose "Allow pop ups."

    

4. Arrange the AWS Management Console tab so that it displays along side these instructions. Ideally, you will be able to see both browser tabs at the same time, to make it easier to follow the lab steps.

&nbsp;
&nbsp;
## Task 1: Explore the Users and Groups

In this task, you will explore the Users and Groups that have already been created for you in IAM.

5. In the **AWS Management Console**, on the **Services** menu, select **IAM**.

      

6. In the navigation pane on the left, choose **Users**.

    The following IAM Users have been created for you:

     * user-1

     * user-2

     * user-3

       

7. Choose **user-1**.

    This will bring to a summary page for user-1. The **Permissions** tab will be displayed.

    

8. Notice that user-1 does not have any permissions.

      

9. Choose the **Groups** tab.

    user-1 also is not a member of any groups.

    

10. Choose the **Security credentials** tab.

    user-1 is assigned a **Console password**

    

11. In the navigation pane on the left, choose **User groups**.

      The following groups have already been created for you:

      * EC2-Admin

      * EC2-Support

      * S3-Support

          

12. Choose the **EC2-Support** group.

     This will bring you to the summary page for the **EC2-Support** group.

     

13. Choose the **Permissions** tab.

     This group has a Managed Policy associated with it, called **AmazonEC2ReadOnlyAccess**. Managed Policies are pre-built policies (built either by AWS or by your administrators) that can be attached to IAM Users and Groups. When the policy is updated, the changes to the policy are immediately apply against all Users and Groups that are attached to the policy.

       

14. Choose the plus (**+**) icon next to the AmazonEC2ReadOnlyAccess policy to view the policy details.

     **Note**: A policy defines what actions are allowed or denied for specific AWS resources. 

     This policy is granting permission to List and Describe information about EC2, Elastic Load Balancing, CloudWatch and Auto Scaling. This ability to view resources, but not modify them, is ideal for assigning to a Support role.

     The basic structure of the statements in an IAM Policy is:

      *  **Effect** says whether to *Allow* or *Deny* the permissions.

      *  **Action** specifies the API calls that can be made against an AWS Service (eg *cloudwatch:ListMetrics*).

      * **Resource** defines the scope of entities covered by the policy rule (eg a specific Amazon S3 bucket or Amazon EC2 instance, or * which means *any resource*).

        

15. Choose the minus icon (**-**) to hide the policy details.

       

16. In the navigation pane on the left, choose **User groups**.

       

17. Choose the **S3-Support** group and then choose the **Permissions** tab.

     The S3-Support group has the **AmazonS3ReadOnlyAccess** policy attached.

       

18. Choose the plus (**+**) icon to view the policy details.

     This policy grants permissions to Get and List resources in Amazon S3.

       

19. Choose the minus icon (**-**) to hide the policy details.

       

20. In the navigation pane on the left, choose **User groups**.

       

21. Choose the **EC2-Admin** group and then choose the **Permissions** tab. 

     This Group is slightly different from the other two. Instead of a *Managed Policy*, it has an **Inline Policy**, which is a policy assigned to just one User or Group. Inline Policies are typically used to apply permissions for one-off situations.

       

22. Choose the plus (**+**) icon to view the policy details.

     This policy grants permission to view (Describe) information about Amazon EC2 and also the ability to Start and Stop instances.

       

23. Choose the minus icon (**-**) to hide the policy details.

&nbsp;
&nbsp;

## Business Scenario

For the remainder of this lab, you will work with these Users and Groups to enable permissions supporting the following business scenario:

Your company is growing its use of Amazon Web Services, and is using many Amazon EC2 instances and a great deal of Amazon S3 storage. You wish to give access to new staff depending upon their job function:


|User|In Group|Permissions|
|----|--------|-----------|
|user-1|S3-Support|Read-Only access to Amazon S3|
|user-2|EC2-Support|Read-Only access to Amazon EC2|
|user-3|EC2-Admin|View, Start and Stop Amazon EC2 instances|

&nbsp;
&nbsp;
## Task 2: Add Users to Groups

You have recently hired **user-1** into a role where they will provide support for Amazon S3. You will add them to the **S3-Support** group so that they inherit the necessary permissions via the attached *AmazonS3ReadOnlyAccess* policy.

<i class="fa fa-comment"></i> You can ignore any "not authorized" errors that appear during this task. They are caused by your lab account having limited permissions and will not impact your ability to complete the lab.

&nbsp;
&nbsp;

### Add user-1 to the S3-Support Group

24. In the left navigation pane, choose **User groups**.

    

25. Choose the **S3-Support** group.

    

26. Choose the **Users** tab.

    

27. In the **Users** tab, choose **Add users**.

    

28. In the **Add Users to S3-Support** window, configure the following:

    * Select <i class="fa fa-check-square-o"></i> **user-1**.

    * At the bottom of the screen, choose **Add Users**.

    In the **Users** tab you will see that user-1 has been added to the group.

&nbsp;
&nbsp;

### Add user-2 to the EC2-Support Group

You have hired **user-2** into a role where they will provide support for Amazon EC2.



29. Using similar steps to the ones above, add **user-2** to the **EC2-Support** group.

    user-2 should now be part of the **EC2-Support** group.

&nbsp;
&nbsp;

### Add user-3 to the EC2-Admin Group

You have hired **user-3** as your Amazon EC2 administrator, who manage your EC2 instances.



30. Using similar steps to the ones above, add **user-3** to the **EC2-Admin** group.

    user-3 should now be part of the **EC2-Admin** group.

    

31. In the navigation pane on the left, choose **User groups**.

    Each Group should now have a **1** in the Users column for the number of Users in each Group.

    If you do not have a **1** beside each group, revisit the above instructions above to ensure that each user is assigned to a User group, as shown in the table in the Business Scenario section.

&nbsp;
&nbsp;
## Task 3: Sign-In and Test Users

In this task, you will test the permissions of each IAM User.



32. In the navigation pane on the left, choose **Dashboard**.

    An **IAM users sign-in link** is displayed on the right. It will look similar to: *https&#58;//123456789012.signin.aws.amazon.com/console*

    This link can be used to sign-in to the AWS Account you are currently using.

    

33. Copy the **Sign-in URL for IAM users in this account** to a text editor.

    

34. Open a private (Incognito) window.

    **Mozilla Firefox**

    * Choose the menu bars <i class="fa fa-bars"></i> at the top-right of the screen
    * Select **New private window**

    **Google Chrome**

    * Choose the ellipsis <i class="fa fa-ellipsis-v"></i> at the top-right of the screen
    * Select **New Incognito Window**

    **Microsoft Edge**

    * Choose the ellipsis <i class="fa fa-ellipsis-h"></i> at the top-right of the screen
    * Choose **New InPrivate window**

    **Microsoft Internet Explorer**

    * Choose the **Tools** menu option

    * Choose **InPrivate Browsing**

      

35. Paste the **IAM users sign-in** link into the address bar of your private browser session and press **Enter**.

    Next, you will sign-in as **user-1**, who has been hired as your Amazon S3 storage support staff.

    

36. Sign-in with:

    * **IAM user name:** `user-1`

    * **Password:** `Lab-Password1`

      

37. In the **Services** menu, choose **S3**.

    

38. Choose the name of the bucket that exists in the account and browse the contents.

    Since your user is part of the **S3-Support** Group in IAM, they have permission to view a list of Amazon S3 buckets and the contents.

    Note: The bucket does not contain any objects.

    Now, test whether they have access to Amazon EC2.

    

39. In the **Services** menu, choose **EC2**.

    

40. In the left navigation pane, choose **Instances**.

    You cannot see any instances. Instead, you see a message that states *You are not authorized to perform this operation*. This is because this user has not been granted any permissions to access Amazon EC2.

    You will now sign-in as **user-2**, who has been hired as your Amazon EC2 support person.

    

41. Sign user-1 out of the **AWS Management Console** by completing the following actions:

    * At the top of the screen, choose **user-1**

    * Choose **Sign Out**

    <img src="https://s3-us-west-2.amazonaws.com/us-west-2-aws-training/awsu-spl/spl-66/images/user-1-sign-out.png" alt="screenshot" />

    

42. Paste the **IAM users sign-in** link into your private browser tab's address bar and press **Enter**.

    Note: This link should be in your text editor.

    

43. Sign-in with:

     * **IAM user name:** `user-2`

     * **Password:** `Lab-Password2`

       

44. In the **Services** menu, choose **EC2**.

    

45. In the navigation pane on the left, choose **Instances**.

    You are now able to see an Amazon EC2 instance because you have Read Only permissions. However, you will not be able to make any changes to Amazon EC2 resources.

    <span style="color:red"><i class="fa fa-exclamation-triangle"></i></span> If you cannot see an Amazon EC2 instance, then your Region may be incorrect. In the top-right of the screen, pull-down the Region menu and select the region that you noted at the start of the lab (for example, **N. Virginia**).

    

    - Select the instance named <i class="fa fa-check-square-o"></i> *LabHost*.

    

46. In the **Instance state** menu above, select **Stop instance**.

    

47. In the **Stop Instance** window, select **Stop**.

    You will receive an error stating *You are not authorized to perform this operation*. This demonstrates that the policy only allows you to view information, without making changes.

    

48. Choose the X to close the *Failed to stop the instance* message.

    Next, check if user-2 can access Amazon S3.

    

49. In the **Services**, choose **S3**.

    You will see the message **You don't have permissions to list buckets** because user-2 does not have permission to access Amazon S3.

    You will now sign-in as **user-3**, who has been hired as your Amazon EC2 administrator.

    

50. Sign user-2 out of the **AWS Management Console** by completing the following actions:

    * At the top of the screen, choose **user-2**

    * Choose **Sign Out**

    <img src="https://s3-us-west-2.amazonaws.com/us-west-2-aws-training/awsu-spl/spl-66/images/user-2-sign-out.png" alt="screenshot" />

    

51. Paste the **IAM users sign-in** link into your private window and press **Enter**.

    

52. Paste the sign-in link into the address bar of your private web browser tab again. If it is not in your clipboard, retrieve it from the text editor where you stored it earlier.

    

53. Sign-in with:

     * **IAM user name:** `user-3`

     * **Password:** `Lab-Password3`

       

54. In the **Services** menu, choose **EC2**.

    

55. In the navigation pane on the left, choose **Instances**.

    As an EC2 Administrator, you should now have permissions to Stop the Amazon EC2 instance.

    Select the instance named <i class="fa fa-check-square-o"></i> *LabHost* .

    <i class="fa fa-exclamation-triangle"></i> If you cannot see an Amazon EC2 instance, then your Region may be incorrect. In the top-right of the screen, pull-down the Region menu and select the region that you noted at the start of the lab (for example, **N. Virginia**).

    

56. In the **Instance state** menu, choose **Stop instance**.

    

57. In the **Stop instance** window, choose **Stop**.

    The instance will enter the *stopping* state and will shutdown.

    

58. Close your private browser window.

    


## Lab complete <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Congratulations! You have completed the lab.



59. Choose <span id="ssb_voc_grey">End Lab</span> at the top of this page, and then select <span id="ssb_blue">Yes</span> to confirm that you want to end the lab.

    A panel indicates that *You may close this message box now...*



60. Select the **X** in the top-right corner to close the panel.



## Conclusion

<i class="far fa-thumbs-up" style="color:blue"></i> Congratulations! You now have successfully:

  * Explored pre-created IAM users and groups

  * Inspected IAM policies as applied to the pre-created groups

  * Followed a real-world scenario, adding users to groups with specific capabilities enabled

  * Located and used the IAM sign-in URL

  * Experimented with the effects of policies on service access



© 2021 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

