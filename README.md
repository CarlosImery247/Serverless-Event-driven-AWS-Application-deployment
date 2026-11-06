# Serverless-Event-driven-AWS-Application-deployment
  Made use of Amazon S3, AWS Lambda, and Amazon SNS to set up an automatic delivery system of .txt file word counts to a designated email address, triggered by the uploading of a file into a S3 bucket.

  In this lab, we developed an event-driven serverless application using Amazon S3, AWS Lambda and Amazon SNS. Amazon S3 acts as the input storage service. Whenever a user uploads a text file, an S3 Object Created event automatically triggers the Lambda function. The Lambda function is developed using Python and Boto3. 

  It reads the uploaded file from S3, counts the number of words and publishes the result to an SNS topic. Amazon SNS then delivers the word count result to subscribed email addresses. We can also configure SMS notifications. We use an IAM execution role to provide Lambda with the necessary permissions to access S3, publish messages to SNS and write logs to CloudWatch. 

Steps to replicate results:

1. Within the AWS Management Console, open Amazon Simple Notification Service (SNS) to create a topic.
   
      a. Select Standard topic
   
      b. Enter topic name - leave everything else at default values.

<img width="624" height="453" alt="SNS topic Creation" src="https://github.com/user-attachments/assets/f86d0209-5dfb-4439-a941-9ca43d6e8603" />


2. Create a subscription within SNS.
   
      a. Select the topic we just created.
   
      b. Click 'Create Subscription' and select Email protocol.
   
      c. Enter target email address and click 'Create Subscription'.
   
            I. Important to note that this will send a subscription confirmation to your target email. You must accept confirm the subscription, otherwise SNS will not send any notifications.

   <img width="651" height="705" alt="SNS subscriptions" src="https://github.com/user-attachments/assets/51d48eec-58bb-44a7-bb9a-263fbdf310ef" />


3.Creating the Lambda Function - Open Lambda within the AWS Management Console.

      a. Select 'Event Functions' on the left panel.

      b. Select 'Create Event Function' and have the following settings: Author from Scratch, WordCountFunction (function name), Architecture: x86_64, Execution role: Use and existing role - LambdaAccessRole 

          I. Note that this was done in a AWS Lab environment with roles already created. To recreate this you will need to create a role using AWS Identity and Access Management (IAM) to provide the following permissions:

          ~AWS LAmbdaBasicExecutionRole
          ~AmazonSNSFullAccess
          ~AmazonS3FullAccess
          ~CloudWatchFullAccess

4. Writing the Python code to be executed by the Lambda function.

      a. Within Lambda in the AWS Management Console, select the function we just created. Navigate to the 'Code' submenu and paste the contents of the 'lambda_function.py' file; there is no need to replace or plug any variables into the code.

      b. Click 'deploy' to save the code.

   <img width="624" height="473" alt="lambda code" src="https://github.com/user-attachments/assets/2f62d2df-37d5-4301-945d-b019b5c19f4d" />

5. Configuring the SNS Topic ARN in Lambda.

      a. Open the lambda function and select 'Configuration'.

      b. Select 'Environment Variables' and click 'Add environment variable'.

      c. Enter 'SNS_TOPIC_ARN' into the 'Key' field, and enter your SNS topic ARN into the 'Value' field. Lastly, select 'Save'.

       I. You can get the value fo yout SNS Topic ARN by going to SNS within your AWS Management COnsole and selecting 'Topics' on the left panel, and selecting the hyperlink on your SNS topic; there you should be able to copy your ARN value. 

<img width="651" height="705" alt="SNS subscriptions" src="https://github.com/user-attachments/assets/8db4fc35-ef50-47b1-bb61-04dd51216b0f" />

<img width="624" height="469" alt="lambda env variables" src="https://github.com/user-attachments/assets/c8b2d657-1908-44bd-9dcf-813f310a5f1d" />

6. Configure S3 as the Lambda Trigger

      a. Within Lambda, select your function, go to the function overview, and select 'Add trigger'.

      b. Select the source as S3 bucket, then select the bucket you created.

      c. Make sure event types is 'Select All object create events'. This sets the trigger condition to be whenever a file is uploaded to the S3 bucket.

      d. Lastly, make sure that the suffix is set to be .txt and that the 'recursive invocation' warning is acknowledged. then click 'Add'.

   <img width="624" height="550" alt="Trigger addition" src="https://github.com/user-attachments/assets/26e224ad-9878-4533-87de-4c7b36c40d73" />

7. Test the Lambda Function

      a. Create three text files (or download the sample files from this reporsitory) and upload them to your S3 bucket.

         I. To upload to a S3 bucket you must go to the S3 AWS Management Console Page, select your buckets and then select 'Upload'.

      b. Once uploaded, head over to Lambda > WordCountFunction > Monitor > view CloudWatch Logs

         I. Here you can see the logs of the Lambda function. these logs can contain error messages if something went wrong with the function, so make sure to pay close attention to their contents.

   <img width="654" height="355" alt="cloudwatch logs" src="https://github.com/user-attachments/assets/53440556-9724-4597-8b34-d8a332fee716" />

   <img width="624" height="496" alt="log content example" src="https://github.com/user-attachments/assets/7e291f10-cf9d-4ff8-9441-0323eb63e64c" />

      c. Check your target email with which you subscribed to the SNS topic for three notifications. The output should be the following:

   <img width="624" height="161" alt="SNS output" src="https://github.com/user-attachments/assets/e4e7ee0a-50da-4fd6-815a-0da9e885af40" />

      d. Once you receive these emails, the lab has been complete and our serverless event-driven application is functional.
   
  The key concepts demonstrated are serverless computing, event-driven architecture, IAM roles, S3 event notifications, Python automation and asynchronous notifications using SNS.
