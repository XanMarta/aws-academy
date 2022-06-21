# Module 13 – Guided Lab 2: Implementing a Serverless Architecture on AWS

[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

##Scenario

You are creating an inventory tracking system. Stores from around the world will upload an inventory file to Amazon S3. Your team wants to be able to view the inventory levels and send a notification when inventory levels are low.

In this lab, you will:

- You will *upload* an *inventory file* to an Amazon S3 bucket.
- This upload will *trigger a Lambda function* that will read the file and insert items into an *Amazon DynamoDB table*.
- A serverless, web-based dashboard application will use Amazon Cognito to authenticate to AWS. The application will then gain access to the DynamoDB table to display inventory levels.
- Another Lambda function will receive updates from the DynamoDB table. This function will send a message to an *SNS topic* when an inventory item is out of stock.
- Amazon SNS will then *send you a notification through short message service (SMS) or email* that requests additional inventory.

<br/>
##Lab overview

Traditionally, applications run on servers. These servers can be physical (or bare metal). They can also be virtual environments that run on top of physical servers. However, you must purchase and provision all these types of servers, and you must also manage their capacity. In contrast, you can run your code on AWS Lambda without needing to pre-allocate servers. With Lambda, you only need to provide the code and define a trigger. The Lambda function can run when it is needed, whether it is once per week or hundreds of times per second. You only pay for what you use.

This lab demonstrates how to trigger a Lambda function when a file is uploaded to Amazon Simple Storage Service (Amazon S3). The file will be loaded into an Amazon DynamoDB table. The data will be available for you to view on a dashboard page that retrieves the data directly from DynamoDB. This solution *does not use Amazon Elastic Compute Cloud (Amazon EC2)*. It is a *serverless solution* that *automatically scales when it is used*. It also incurs *little cost* when it is in use. When it is idle, there is _practically no cost_  because will you only be billed for data storage.



After completing this lab, you should be able to:

- Implement a serverless architecture on AWS
- Trigger Lambda functions from Amazon S3 and Amazon DynamoDB
- Configure Amazon Simple Notification Service (Amazon SNS) to send notifications



At the **end** of this lab, your architecture will look like the following example:

![Workflow](images/module-13-guided-lab-architecture.png)

<br/>
##Duration

This lab takes approximately **40 minutes** to complete.

<br/>
## AWS service restrictions

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
## Task 1: Creating a Lambda function to load data

In this task, you will create *a Lambda function* that will process an inventory file. The Lambda function will read the file and insert information into a DynamoDB table.

![Lambda function](images/module-13-guided-lab-task-1.png)

5. In the **AWS Management Console**, on the <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> menu, choose **Lambda**.

6. Choose <span id="ssb_lambda_orange">Create function</span>

   <i class="fas fa-info-circle"></i> *Blueprints* are code templates for writing Lambda functions. Blueprints are provided for standard Lambda triggers, such as creating Amazon Alexa skills and processing Amazon Kinesis Data Firehose streams. This lab provides you with a pre-written Lambda function, so you will use the **Author from scratch** option.

7. Configure the following settings:

    - **Function name:** `Load-Inventory`
    - **Runtime:** _Python 3.7_
    - Expand <i class="fas fa-caret-right"></i> **Choose or create an execution role**.
    - **Execution role:** _Use an existing role_
    - **Existing role:** _Lambda-Load-Inventory-Role_

    This role gives the Lambda function permissions so that it can access Amazon S3 and DynamoDB.

8. Choose <span id="ssb_lambda_orange">Create function</span>

9. Scroll down to the **Code source** section, and in the **Environment** pane, choose lambda_function.py.

10. In the code editor, delete all the code.

11. In the **Code source** editor, copy and paste the following code:

   ```bash
   # Load-Inventory Lambda function
   #
   # This function is triggered by an object being created in an Amazon S3 bucket.
   # The file is downloaded and each line is inserted into a DynamoDB table.
   import json, urllib, boto3, csv
   # Connect to S3 and DynamoDB
   s3 = boto3.resource('s3')
   dynamodb = boto3.resource('dynamodb')
   # Connect to the DynamoDB tables
   inventoryTable = dynamodb.Table('Inventory');
   # This handler is run every time the Lambda function is triggered
   def lambda_handler(event, context):
     # Show the incoming event in the debug log
     print("Event received by Lambda function: " + json.dumps(event, indent=2))
     # Get the bucket and object key from the Event
     bucket = event['Records'][0]['s3']['bucket']['name']
     key = urllib.parse.unquote_plus(event['Records'][0]['s3']['object']['key'])
     localFilename = '/tmp/inventory.txt'
     # Download the file from S3 to the local filesystem
     try:
       s3.meta.client.download_file(bucket, key, localFilename)
     except Exception as e:
       print(e)
       print('Error getting object {} from bucket {}. Make sure they exist and your bucket is in the same region as this function.'.format(key, bucket))
       raise e
     # Read the Inventory CSV file
     with open(localFilename) as csvfile:
       reader = csv.DictReader(csvfile, delimiter=',')
       # Read each row in the file
       rowCount = 0
       for row in reader:
         rowCount += 1
         # Show the row in the debug log
         print(row['store'], row['item'], row['count'])
         try:
           # Insert Store, Item and Count into the Inventory table
           inventoryTable.put_item(
             Item={
               'Store':  row['store'],
               'Item':   row['item'],
               'Count':  int(row['count'])})
         except Exception as e:
            print(e)
            print("Unable to insert data into DynamoDB table".format(e))
       # Finished!
       return "%d counts inserted" % rowCount
   ```

   Examine the code. It performs the following steps:

   - Download the file from Amazon S3 that triggered the event
   - Loop through each line in the file
   - Insert the data into the DynamoDB *Inventory* table

12. Choose <span id="ssb_white">Deploy</span> to save your changes.

    Next, you will configure Amazon S3 to trigger the Lambda function when a file is uploaded.

<br/>
## Task 2: Configuring an Amazon S3 event

Stores from around the world provide inventory files to load into the inventory tracking system. Instead of uploading their files via FTP, the stores can upload them directly to Amazon S3. They can upload the files through a webpage, a script, or as part of a program. When a file is received, it triggers the Lambda function. This Lambda function will then load the inventory into a DynamoDB table.

![Lambda function](images/module-13-guided-lab-task-2.png)

In this task, you will create an S3 bucket and configure it to trigger the Lambda function.

13. On the <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> menu, choose **S3**.

14. Choose <span id="ssb_orange"><i class="fas fa-plus"></i>Create bucket</span>

    Each bucket must have a unique name, so you will add a random number to the bucket name. For example: *inventory-123*

15. For **Bucket name** enter: `inventory-<number>` (Replace _<number>_ with a random number)

16. Choose <span id="ssb_orange">Create bucket</span>

     <i class="fas fa-comment"></i> You might receive an error that states: *The requested bucket name is not available*. If you get this error, choose the first **Edit** link, change the bucket name, and try again until the bucket name is accepted.

    You will now configure the bucket to automatically trigger the Lambda function when a file is uploaded.

17. Choose the name of your _inventory-<number>_ bucket.

18. Choose the **Properties** tab.

19. Scroll down to **Event notifications**.

    You will configure an event to trigger when an object is created in the S3 bucket.

20. Click  <span id="ssb_white">Create event notification</span> then configure these settings:

    - **Name:** `Load-Inventory`
    - **Event types:** <i class="far fa-check-square"></i> _All object create events_
    - **Destination:** _Lambda Function_
    - **Lambda function:** _Load-Inventory_
    - Choose <span id="ssb_orange">Save changes</span>

    When an object is created in the bucket, this configuration tells Amazon S3 to trigger the _Load-Inventory_ Lambda function that you created earlier.

    Your bucket is now ready to receive inventory files!

<br/>
## Task 3: Testing the loading process

You are now ready to test the loading process. You will upload an inventory file, then check that it loaded successfully.

![Load Test](images/module-13-guided-lab-task-3-1.png)

21. Download the inventory files by opening the context (right-click) menu for these links:

    [inventory-berlin.csv](../../../scripts/inventory-berlin.csv)

    [inventory-calcutta.csv](../../../scripts/inventory-calcutta.csv)

    [inventory-karachi.csv](../../../scripts/inventory-karachi.csv)

    [inventory-pusan.csv](../../../scripts/inventory-pusan.csv)

    [inventory-shanghai.csv](../../../scripts/inventory-shanghai.csv)

    [inventory-springfield.csv](../../../scripts/inventory-springfield.csv)



These files are the inventory files that you can use to test the system. They are comma-separated values (CSV) files. The following example shows the contents of the Berlin file:


   ```plain
     store,item,count
     Berlin,Echo Dot,12
     Berlin,Echo (2nd Gen),19
     Berlin,Echo Show,18
     Berlin,Echo Plus,0
     Berlin,Echo Look,10
     Berlin,Amazon Tap,15
   ```

22. In the console, return to your S3 bucket by choosing the **Objects** tab.

23. Choose <span id="ssb_orange">Upload</span>

24. Choose <span id="ssb_white">Add files</span>, and select one of the inventory CSV files. (You can choose any inventory file.)

25. Choose <span id="ssb_orange">Upload</span> 

    Amazon S3 will automatically trigger the Lambda function, which will load the data into a DynamoDB table.

    A serverless Dashboard application has been provided for you to view the results.

26. At the top of these instructions, choose the **Details** button, and to the right of **AWS**, choose the **Show** button.

27. From the **Credentials** window, copy the **Dashboard** URL.

28. Open a new web browser tab, paste the URL, and press ENTER.

    The dashboard application will open and display the inventory data that you loaded into the bucket. The data is retrieved from DynamoDB, which proves that the upload successfully triggered the Lambda function.

   ![Dashboard](images/module-13-guided-lab-task-3-2.png)



<i class="fas fa-exclamation-triangle"></i> If the dashboard application does not display any information, ask your instructor to help you diagnose the problem.

The dashboard application is served as a static webpage from Amazon S3. The dashboard authenticates via Amazon Cognito as an _anonymous user_, which provides sufficient permissions for the dashboard to retrieve data from DynamoDB.

You can also view the data directly in the DynamoDB table.

29. On the <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> menu, choose **DynamoDB**.

30. In the left navigation pane, choose **Tables**.

31. Choose the **Inventory** table.

32. Choose the **Items** tab.

    The data from the inventory file will be displayed. It shows the store, item and inventory count.

<br/>
## Task 4: Configuring notifications

You want to notify inventory management staff when a store runs out of stock for an item. For this serverless notification functionality, you will use **Amazon SNS**.

![Load Test](images/module-13-guided-lab-task-4.png)

Amazon SNS is a flexible, fully managed publish/subscribe messaging and mobile notifications service. It delivers messages to subscribing endpoints and clients. With Amazon SNS, you can fan out messages to a large number of subscribers, including distributed systems and services, and mobile devices.

33. On the <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> menu, choose **Simple Notification Service**.

34. In the **Create topic** box, for **Topic name**, enter: `NoStock`

35. Choose <span id="ssb_orange">Next step</span>

36. Scroll to the bottom of the page.

37. Choose <span id="ssb_orange">Create topic</span>

    To receive notifications, you must *subscribe* to the topic. You can choose to receive notifications via several methods, such as SMS and email.  

38. In the lower half of the page, choose <span id="ssb_orange">Create subscription</span> and configure these settings:

    - **Protocol:** _Email_
    - **Endpoint:** Enter your email address
    - Choose <span id="ssb_orange">Create subscription</span>

    <i class="fas fa-comment"></i> After you create an email subscription, you will receive a confirmation email message. Open the message and choose the _Confirm subscription_ link.

    Any message that is sent to the SNS topic will be forwarded to your email.

<br/>
## Task 5: Creating a Lambda function to send notifications

You could modify the existing _Load-Inventory_ Lambda function to check inventory levels while the file is being loaded. However, this configuration is not a good architectural practice. Instead of overloading the _Load-Inventory_ function with business logic, you will create another Lambda function that is triggered when data is loaded into the DynamoDB table. This function will be triggered by a _DynamoDB stream_.

This architectural approach offers several benefits:

- Each Lambda function performs a single, specific function. This practice makes the code simpler and more maintainable.
- Additional business logic can be added by creating additional Lambda functions. Each function operates independently, so existing functionality is not impacted.

In this task, you will create another Lambda function that looks at inventory while it is loaded into the DynamoDB table. If the Lambda function notices that an item is out of stock, it will send a notification through the SNS topic you created earlier.

![Load Test](images/module-13-guided-lab-task-5.png)

39. On the <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> menu, choose **Lambda**.

40. Choose <span id="ssb_lambda_orange">Create function</span> and configure these settings:

    - **Function name:** `Check-Stock`
    - **Runtime:** _Python 3.7_
    - Expand <i class="fas fa-caret-right"></i> **Choose or create an execution role**.
    - **Execution role:** _Use an existing role_
    - **Existing role:** _Lambda-Check-Stock-Role_
    - Choose <span id="ssb_lambda_orange">Create function</span>

    This role was configured with permissions to send a notification to Amazon SNS.

41. Scroll down to the **Code source** section, and in the **Environment** pane, choose lambda_function.py.

42. In the code editor, delete all the code.

43. Copy the following code, and in the **Code Source** editor, paste the copied code:

    ```bash
    # Stock Check Lambda function
    #
    # This function is triggered when values are inserted into the Inventory DynamoDB table.
    # Inventory counts are checked and if an item is out of stock, a notification is sent to an SNS Topic.
    import json, boto3
    # This handler is run every time the Lambda function is triggered
    def lambda_handler(event, context):
      # Show the incoming event in the debug log
      print("Event received by Lambda function: " + json.dumps(event, indent=2))
      # For each inventory item added, check if the count is zero
      for record in event['Records']:
        newImage = record['dynamodb'].get('NewImage', None)
        if newImage:      
          count = int(record['dynamodb']['NewImage']['Count']['N'])  
          if count == 0:
            store = record['dynamodb']['NewImage']['Store']['S']
            item  = record['dynamodb']['NewImage']['Item']['S']  
            # Construct message to be sent
            message = store + ' is out of stock of ' + item
            print(message)  
            # Connect to SNS
            sns = boto3.client('sns')
            alertTopic = 'NoStock'
            snsTopicArn = [t['TopicArn'] for t in sns.list_topics()['Topics']
                            if t['TopicArn'].lower().endswith(':' + alertTopic.lower())][0]  
            # Send message to SNS
            sns.publish(
              TopicArn=snsTopicArn,
              Message=message,
              Subject='Inventory Alert!',
              MessageStructure='raw'
            )
      # Finished!
      return 'Successfully processed {} records.'.format(len(event['Records']))
    ```

    Examine the code. It performs the following steps:

    - Loop through the incoming records

    - If the inventory count is zero, send a message to the _NoStock_ SNS topic

    You will now configure the function so it triggers when data is added to the _Inventory_ table in DynamoDB.

44. Choose <span id="ssb_lambda_white">Deploy</span> to save your code changes

45. Scroll to the **Designer** section (which is at the top of the page).

46. Choose <span id="ssb_lambda_white"><i class="fas fa-plus"></i>Add trigger</span> and then configure these settings:

    - **Select a trigger:** _DynamoDB_
    - **DynamoDB Table:** _Inventory_
    - Choose <span id="ssb_lambda_orange">Add</span>

    You are now ready to test the system!

<br/>

## Task 6: Testing the System

You will now upload an inventory file to Amazon S3, which will trigger the original _Load-Inventory_ function. This function will load data into DynamoDB, which will then trigger the new _Check-Stock_ Lambda function. If the Lambda function detects an item with zero inventory, it will send a message to Amazon SNS. Then, Amazon SNS will notify you through SMS or email.

47. On the <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> menu, choose **S3**.

48. Choose the name of your _inventory-<number>_ bucket.

49. Choose **Upload** and upload a different inventory file.

50. Return to the **Inventory System Dashboard** and refresh <i class="fas fa-sync"></i> the page.

    You should now be able to use the **Store** menu to view the inventory from both stores.

    Also, you should receive a *notification through SMS or email* that the store has an out-of-stock item (each inventory file has one item that is out of stock).

    <i class="fas fa-comment"></i> If you did not receive a notification, wait a few minutes and upload a different inventory file. The DynamoDB trigger can sometimes take a few minutes to enable.

51. Try to upload multiple inventory files at the same time. What do you think will happen?

<br/>
## Submitting your work

52. At the top of these instructions, choose <span id="ssb_blue">Submit</span> to record your progress and when prompted, choose **Yes**.

53. If the results don't display after a couple of minutes, return to the top of these instructions and choose <span id="ssb_voc_grey">Grades</span>

    **Tip**: You can submit your work multiple times. After you change your work, choose **Submit** again. Your last submission is what will be recorded for this lab.

54. To find detailed feedback on your work, choose <span id="ssb_voc_grey">Details</span> followed by <i class="fas fa-caret-right"></i> **View Submission Report**.

<br/>

## Lab complete <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Congratulations! You have completed the lab.

55. Choose <span id="ssb_voc_grey">End Lab</span> at the top of this page, and then select <span id="ssb_blue">Yes</span> to confirm that you want to end the lab.

    A panel indicates that *DELETE has been initiated... You may close this message box now.*

56. Select the **X** in the top right corner to close the panel.





*©2020 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*