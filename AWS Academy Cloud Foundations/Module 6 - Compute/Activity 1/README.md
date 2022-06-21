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
  #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
  }
</style>

# Activity: AWS Elastic Beanstalk

<!-- Note to translators: This activity is unique to this course. -->

&nbsp;
&nbsp;

## Overview

  This activity provides you with an Amazon Web Services (AWS) account where an AWS Elastic Beanstalk environment has been pre-created for you. You will deploy code to it and observe the AWS resources that make up the Elastic Beanstalk environment.

&nbsp;

### Duration

This activity takes approximately **30 minutes** to complete.

&nbsp;
&nbsp;
## Accessing the AWS Management Console

1. At the top of these instructions, choose <span id="ssb_voc_grey">Start Lab</span> to launch your lab.

    A **Start Lab** panel opens, and it displays the lab status.

2. Wait until you see the *Lab status: in creation* message. To close the **Start Lab** panel, choose the **X** .

3. At the top of these instructions, choose <span id="ssb_voc_grey">AWS</span>

    The AWS Management Console opens in a new browser tab. The system  automatically logs you in.

    **Tip**: If a new browser tab does not open, a banner or icon will typically be at the top of your browser, indicating that your browser is preventing the website from opening pop-up windows. Choose the banner or icon and choose **Allow pop ups.**

4. Arrange the **AWS Management Console** tab so that it displays alongside these instructions. Ideally, you will be able to see both browser tabs at the same time, which makes it easier to follow the activity steps.

&nbsp;
&nbsp;
## Task 1: Access the Elastic Beanstalk environment

5. In the **AWS Management Console**, from the **Services** menu, choose **Elastic Beanstalk**.

    A page titled **All environments** should open, and it should show a table that lists the details for an existing Elastic Beanstalk application.  

    **Note**: If the status in the **Health** column is not Green, it has not finished starting yet. Wait a few moments, and it should change to Green.

    <img src="../../images/application.png" alt="elastic-beanstalk-app" width="600" style="border:1px solid black">

6. Under the **Environment name** column, choose on the name of the environment.

    The **Dashboard** page for your Elastic Beanstalk environment opens.

7. Notice that the page shows that the health of your application is Green (good).

    The Elastic Beanstalk environment is ready to host an application. However, it does not yet have running code.

8. Near the top of the page, choose the URL (the URL ends in *elasticbeanstalk.com*).

    When you choose the URL, a new browser tab opens. However, you should see that it displays an *"HTTP Status 404 - Not Found"* message. *This behavior is expected* because this application server doesn't have an application running on it yet. Return to the Elastic Beanstalk console.

    In the next step, you will deploy code in your Elastic Beanstalk environment.

&nbsp;
&nbsp;
## Task 2: Deploy a sample application to Elastic Beanstalk

9. To download a sample application, choose this link:
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip

<!--the zip file is linked in this documentation page: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-getstarted.html-->

10. Back in the Elastic Beanstalk Dashboard, choose **Upload and Deploy**.

11. Choose **Browse or Choose File**, then navigate to and open the **tomcat.zip** file that you just downloaded.

12. Choose **Deploy**.

    It will take a minute or two for Elastic Beanstalk to update your environment and deploy the application.

    **Note**: If you see a warning in the Elastic Beanstalk dashboard page that an instance profile is required to integrate with the AWS X-Ray service, you can ignore the warning.

13. After the deployment is complete, choose the URL value near the top of the screen (or, if you still have the browser tab that displayed the 404 status, refresh that page).

    The web application that you deployed displays.

    <img src="../../images/web-app.png" alt="web-app" width="600" style="border:1px solid black">

    Congratulations, you have successfully deployed an application on Elastic Beanstalk!

14. Back in the Elastic Beanstalk console, choose **Configuration** in the left pane.

    Notice the details here.

    For example, in the **Instances** row, it indicates the Monitoring interval, EC2 Security groups, and Root volume type details of the Amazon Elastic Compute Cloud (Amazon EC2) instances that are hosting your web application.

15. Scroll to the bottom of the page to the **Database** row.

    The **Database** row does not have any details because the environment does not include a database.

16. In the **Database** row, choose **Edit**.

    Note that you could easily add a database to this environment if you wanted to: you only need to set a few basic configurations and choose **Apply**. (However, for the purposes of this activity, you do not need to add a database.)

17. In the left panel, choose **Monitoring**.

    Browse through the charts to see the kinds of information that are available to you.

&nbsp;
&nbsp;
## Task 3: Explore the AWS resources that support your application

18. From the **Services** menu, choose **EC2**

19. Choose **Instances**.

    Note that two instances are running (they both contain *samp* in their names). Both instances support your web application.

20. If you want to continue exploring the Amazon EC2 service resources that were created by Elastic Beanstalk, feel free to explore them. You will find:

    - A *security group* with port 80 open
    - A *load balancer* that both instances belong to
    - An *Auto Scaling group* that runs from two to six instances, depending on the network load

     Though Elastic Beanstalk created these resources for you, you still have access to them.

&nbsp;
&nbsp;
## Activity complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the activity.

21. At the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then to confirm that you want to end the activity, choose <span id="ssb_blue">Yes</span>.  

    A panel appears, with a message that indicates: *DELETE has been initiated... You may close this message box now.*

22. To close the panel, go to the top-right corner and choose the **X**.

For feedback, suggestions, or corrections, email us at: *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;
