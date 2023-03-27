## Use cases

- backup and storage
- disaster recovery
- archive
- hybrid cloud storage
- application hosting
- media hosting
- data lakes & big data analytics
- software delivery


## S3 Buckets

- globally unique name, and **NO uppercase, No underscore**
- S3 looks like a global service but buckets are created in a region

## S3 Objects

- key is the full path ,e.g. s3://{bucket_name}/{the remaining path}
- max object size 5TB
- must use "multi-part upload" if the file size > 5 GB

## S3 Security

- User based policy - IAM
- **Resource based policy** 
	- bucket policies -  like [[2. IAM and AWS CLI#IAM policy json]], which can configures:
		- **User** access to S3
		- **Role** access to S3
		- **Cross account access**
	- Object Access Control List (ACL)
	- Bucket ACL
- Permission logic - (IAM permission ALLOW OR Resource policy ALLOW ) AND (NO explicit deny)
- Encryption
- Default layer from AWS - **Block Public Access** , can be toggled on/off

## Static website hosting in S3 
- need to off the **Block Public Access**

## S3 versioning

- enable at bucket level
- create running version number when file with same key being uploaded
- logic:
	- new versioned s3 object with **version id** as **Null**
	- suspending versioning will **NOT delete** the previous versions
 - deletion - it is adding a **delete marker** without physically deleting the file, if you toggle the display of version in UI, only when S3 versioning is enabled
 
## S3 Replication (CRR, SRR)

Steps:
	- **Must enable the versioning**
	- Whether it is Cross Region Replication, or Same Region Replication
	- IAM permission to S3 must be granted
	- Copy is **asynchronous**
	- **Only new objects** created after the enabling of replication will be replicated
	- However, you can replicate existing objects using **S3 Batch Replications**
	- **No chaining of replications** - Bucket A has replication in Bucket B, which has replication in Bucket C, new object created in Bucket A will **NOT** replicated in Bucket C

Use cases:
	- CRR - Compliance, lower latency needs, cross account needs
	- SRR - log aggregation, live replication between PROD, TEST environments

## S3 Storage Class

- can move s3 objects between classes manually or using **S3 Lifecycle configurations**
- classes are:
	- s3 standard - general purpose
		- for frequent accessing
		- low latency, high throughput
	- s3 standard - infrequent access (IA)
		- lower cost then s3 standard - GP
		- but **rapid access** when needed, so incurs cost when retrieval
		- Use case: Disaster Recovery, Backups
	- s3 one zone - IA
		- within single AZ
	- s3 glacier (low cost for **Storage + Retrieval**)
		- instance retrieval
		- s3 glacier flexible retrieval - you willing to wait for object retrieval
		- s3 glacier deep archive
	- s3 intelligent tiering - moves objects automatically between access tiers based on usage, tiers including - frequent access, infrequent access, archive instant etc
- Lifecycle rules:
	- transaction actions - move between classes
	- expiration actions - delete object after some time
	- rule can be set to s3 key with patterns
	- **leverage on aws tools - Storage Class Analysis** - gives you a initial lifecycle rules