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
      a. Select the topic we just created
      b. Click 'Create Subscription' and select Email protocol.
      c. Enter target email address and click 'Create Subscription'.
            I. Important to note that this will send a subscription confirmation to your target email. You must accept confirm the subscription, otherwise SNS will not send any notifications.

3.Creating the Lambda Function - Open Lambda within the AWS Management Console.


  
  The key concepts demonstrated are serverless computing, event-driven architecture, IAM roles, S3 event notifications, Python automation and asynchronous notifications using SNS.
