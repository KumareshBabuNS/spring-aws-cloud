
# AWS with Spring-cloud 
REST API that copies s3 object from source bucket to target object and inserts status of this activity to dynamoDB.

## Prerequisites:
1. AWS account
2. Set up credentials to aws-credentials.yml
2. SQS queue (set name and queueUrl in application.yml)
3. SNS topic (set topic and topicArn properties in application.yml)
4. S3 buckets: source bucket and destination bucket (set s3.source.bucket and s3.destination.bucket)
5. Local dynamoDB 

This is a POC to explore APIs for all above AWS services.
External application provides S3 object name, and this object is transfered from source bucket to destination bucket. 
After this process is done, this file transfer request is inserted in Dynamodb with status. 

##### How it works:
* On application startup, SQS queue subscribes to SNS topic
* External Application can send s3 object name to SNS topic via REST call: 
	http://localhost:8080/spring-aws/messages/send/{message} 
	where message is s3Key that you want to transfer from bucket1 to bucket 2	
* As SQS is subscribing to SNS, this message is passed on to SQS. 	
* s3 object is moved from source bucket to destination bucket.
* After process is done, it creates an entry with upload status in dynamodb in table FileTransferRequest

