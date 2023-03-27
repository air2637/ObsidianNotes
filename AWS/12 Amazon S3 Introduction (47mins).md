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
