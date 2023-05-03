*DynamoDB*
- key-value , document NoSQL db
- fully managed, 
- multi-region: you can specify which region you want the replica to be
- multi-master/active: when one table is unavailable, the query will be routed to the other

https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html

*RedShift*
- fully managed
- petabyte-scale cloud based data warehouse
- **Redshift Spectrum** for querying data from files in s3 without need to load them into a Redshift table

*ElastiCache*
- in-memory data store
- used as **caching layer** in front of a **relational db**
- use cases: caching, session store, gaming, geospatial services, realtime analytics

*To speed up boot up time of Elastic Beanstalk*
- Elastic Beanstalk - you upload your code, while Elastic Beanstalk handles the deployment, including **capacity provisioning, load balancing, auto-scaling, application health monitoring etc**. You still have the full control over the resources if you want. 
- You can either use a standard Elastic beanstalk AMI or your own.
- you can speed up the boot up by having a golden AMI and user data for dynamic installation
	- Golden AMI - an AMI that you standardize through configurations, consistent security patching, and hardening.
	- User data 
		- run scripts after the instances starts
		- script is run at root user level

*You have ec2 web server , RDS PostgreSQL DB. the DB resides in private subnet with inbound rule from the EC2 web server only. What can you to enhance the security in accessing the DB?**
- Use SSL for data in transit - to encrypt the connection between application and the postgreSQL.
- IAM db authentication - with this enabled, you don't need use a password for connecting to DB. It is just for authentication.
- You can never SSH into an RDS DB

*Use a mix of on-demand and spot instances across multiple instance types*

- Keyword here is the mix of on-demand and spot instances - that you can only launched with **A launch template** but not a **launch configuration**
- **Launch template** is always preferred over a launch configuration. In addition, it provides **versioning** feature.

https://docs.aws.amazon.com/autoscaling/ec2/userguide/launch-templates.html

*Copy s3 data from one region to another*
- s3 sync command -:
	- it copies data presents in one bucket but NOT in another
	- on a versioned bucket, it only copies the current version object - but not the previous versions
	- You can always run the s3 sync command again if it fail in the middle
 - s3 batch replication:
	- to automatically replicate new objects as they are written to the bucket, use **live replication**
	- to replicate exiting objects to a different bucket on demand, use **s3 batch replication**
 - s3 transfer acceleration - enables fast, easy, secure transfers of files over **long distance between client and an s3 bucket**
 https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html

*Caching is done with a Redis ElastiCache cluster, you want the authentication to Redis, using username, password combination*
- Redis Authentication - enable Redis to require a token (password) before allowing other operations
- IAM authentication is NOT supported by ElastiCache

*To stream existing data files and ongoing updates from s3 to Kinesis Data Streams*
- AWS DMS - Data migration service, that supports the migration of data with **source** be:
	- relational db
	- data warehouse
	- streaming platform
	- other data store in aws cloud
 - DMS requires no code implementation for the migration task
 - alternatively, the object change in s3 can be monitored by:
	 - Cloudtrail with object level monitoring on s3 -> EventBridge event
	 - or S3 event notificaiton -> 
	 - the event -> lambda -> Kinesis Data streams: it requires lambda code to write data to Kinesis Data streams

*Name the 2 services supports VPC Gateway Endpoints*
- VPC endpoint (through PrivateLink) can be of type:
	- interface endpoint
		- powered by elastic network interface (ENI)
		- support most AWS services
	- gateway endpoint
		- you specific in route table with target to the support aws services - **DynamoDB, S3**
https://digitalcloud.training/vpc-interface-endpoint-vs-gateway-endpoint-in-aws/

*What can be the data sources for GuardDuty?*
- GuardDuty:
	- machine learning powered
	- sources: 
		- AWS CloudTrail events
		- VPC Flow logs
		- DNS logs

*High network usage in distributing static content of the application* 
- distributing(saving??) static file on s3 for lowering the cost

*You have S2S VPN connections and you want to estabilish the connectivity for all branches*
- VPN CloudHub
	- hub and spoke model
	- Virtual Private Gateway + Customer Gateway at each branch

*A fleet of ec2 instances, want to get notified via Email when CPU utilization breaches the threshold*
- Cloudwatch alarms on CPU utilization breach
- SNS to send email

*A DB query on a recommandation application*
- Neptune:
	- fully managed **graph db**
	- for recommandation engines, fraud detection, knowlege graph etc

*VPC configurations supported*

only supports 4 types of configurations:
1. VPC with a single public subnet + internet gateway -> single tier, public facing web app
2. VPC with both public and private subnet -> multi-tiers
3. VPC with both public and private subnet + S2S VPN -> extend to your corporate Data Cetre
4. VPC with private subnet + S2S VPN -> similar to type #3, but with no exposure to public internet

*Use permission boundary to control maximum permission*
- effective permission = intersection of **Permission Boundary** and **Permission Policy**

*Placement Group and its strategies*
Placement Group is to influence the placement of a group of **interdependent instances** to meet your workload. with 3 strategies:
1. Cluster placement group:
	1. low latency network, as it is single rack
	2. single AZ
2. Partition placement group
	1. different partition, so instances in a partition do not share rack with instance in another partition
	2. multi AZ
3. Spread placement group - 
	1. placed on distinct stacks, with each stack having its own **network, power source**
	2. 7 running instances per AZ, per group

*Global Accelerator*
- route user traffic to the optimal endpoint
- good for non-http use cases - UDP, MQTT, etc
- HTTP as well!
- specifically if the use case requires static IP
- fast regional failover

*CloudFront*
- for content delivery network globally
- regional edge caches data
- support HTTP/ RTMP based requests
- **Origin failover** - CloudFront to retrieve data from an origin you defined

*CloudWatch alarm action*
- the CloudWatch alarm action can auto stop , terminate, reboot, or recover EC2 instance

*Replicate data from Oracle DB, PostgreSQL, consolidate them into Data Warehouse - RedShift*
- AWS DMS to replicate the data and consolidate into RedShift
- **AWS EMR** - big data platform
- **AWS Glue** - fully managed ETL data processing

*S3 Bucket Policy*
- Allow/Deny of objects in a bucket
- control for users of your account/ other account (in contract to IAM policy - grant users within your account; ACL - does support other account, but grant account level but not user level)

*Direct Connect and DB query in a VPC*
- lowest cost setup is *Deploy the query tool in the VPC, while access the query tool via a Direct Connect network* , data transfering over Direct Connect is **LOWER** then via internet

*Aurora Replica*
- spread the load of read-only connections to Aurora Replicas under the reader endpoint. 
- if the writer instance becomes unavailable, a reader instance will be placed as the writer
- 15 Aurora Replicas per AZ
- Aurora Replica share the same data volume as the primary instance -> no replication lag

*General Read Replica*
- scaling for read-heavy db workloads
- for businesss reporting / data warehousing by running the queries on read replica
- may use read replica for DR either in same region / different region
-  asynchronous replicates , even in different region


*RDS with multi-AZs*
- it automatically create a **primary db** and synchronously replicate the date to a **standby** instance in another AZ
- failover takes 1 to 2 mins


*AWS Schema Conversion Tool + AWS DMS*
- step1: AWS SCT to convert source schema and code to match that of target db
- step2: AWS DMS to migrate data from source to target db

AWS Schema Conversion Tool also supports the transforming of **Stored procedure, secondary database objects**

*DAX*
- fully managed, in-memory cache for **DynamoDB**
- client SDK to point your existing DynamoDB API calls to **DAX Cluster**, and let DAX to handle the rest

*Advantages of NAT instance over NAT Gateway*
- you can assign the SG 
- port forwarding
- can use bastion server

*Facts about lambda*
- you can have up to 5 lambda layer per function
- you can package and deploy lambda function as container image

*Routing in NLB*
- if you specify the target using instance ID - the NLB rewrite teh destination IP address with **Primary private IP address**
- if you specify the target using IP address - the IP you specified is used, this is for catering the scenario where **multiple app on an instance to use the same port**

*Standard / One-zone Infrequent Access*

- Standard IA comparing to One-zone IA, that replicate data in minimum of 3 AZs.
- No matter standard/ One zone IA, that object has to stay for 30 days before moving to the class