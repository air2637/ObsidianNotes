*Storage gateway*
- Tape Gateway
- File Gateway
- Volume Gateway - connect on-premises app to cloud storage.
	- caching data locally
	- full volume stored in s3

*Long polling of SQS*
- SQS sends response after it collect at least 1 message but up to a **specified** number of messages. 
- can help reduce the cost as less number of empty retrievals

*Rollover of DB periodically*
- Schedule EventBridge **Cron expression** to invoke a Lambda function that runs the DB rollover job

*AWS System Manager*
- group resources, like EC2 instances, s3 buckets, or RDS instances by **application**
- view operation data
- take action on the group of resources

Resource performance -> **cloudwatch**
account-specific activity / audits -> **cloudtrail**
resource-specific history, audit, compliance -> **Config**

*AWS Transfer Family*
- file transfers directly into/ out of s3, EFS and not other storage services

*SCP*
- affect all users and roles in attached accounts, including the root user
- effective permission = intersection of scp and IAM permission policy
- do **NOT affect service linked role**

*Delay Queue*
- postpone the delivery of new messages to a queue for X seconds, where max X = 15 mins
- use it when you consumer application needs additional time to process message

*EFS*
- only compatible with linux based AMIs

*RDS with multi-AZs*
- it automatically create a **primary db** and synchronously replicate the date to a **standby** instance in another AZ
- failover takes 1 to 2 mins

*Origin Access Identify*
- CloudFront provides 2 ways to send authenticated requests to an S3 origin:
	- Origin Access Control (more preferred)
		 - works for bucket launched after 2022, where object encrypted
		 - support Dynamic requests to s3 (PUT, POST etc)
	- Origin Access Identify

*Cross zone load balancing*
- To make the load balancing distribute traffic equally among all nodes regardless which AZ it belongs to

*Route 53*
- Alias Record: Redirect queries to selected aws resources (s3, cloudFront, another record in the same Route  53 hosted zone)