![exam_logo](../../images/AWS-ML-Specialty.png)

# AWS Machine Learning Specialty

# Data Engineering :rocket:

## S3

- [x] Overview
- A storage for saving objects (files), Max object size is 5TB
- Objects (files) are organized in **buckets**(directories), buckets must have a *globally unique name*
- Objects (files) have a Key, the key is the **FULL** path. e.g <my_bucket>/my_folder/my_file.txt
- Supports partitioning, object tags (key/value pair)
- ideal candidate for building Data Lake

- [x] Storage class

|Storage class|Designed for|Access Frequency|Retrieve Time|Use cases|
|----|----|----|----|----|
|S3 Standard| General purpose, Frequently access data| Regularly| millisecond| Data analytics, or Machine Learning, etc.|
|S3 Standard-Infrequent Access (IA)| Long-lived, infrequently accessed data| Once a month|millisecond| Disaster Recovery, backups|
|S3 One Zone-IA| Recreatable, infrequently accessed data| Once a month|millisecond| secondary backup copies of on-premise data, or data you can create|
|S3 Glacier Instant Retrieval| Long-lived, archive data| Once a quarter|millisecond| Archiving/backup|
|S3 Glacier Flexible Retrieval| Long-lived, archive data| Once a year| minutes to hours| Archiving/backup|
|S3 Glacier Deep Archive| Long-lived, archive data| Once a year| hours| Archiving/backup|
|S3 Intelligent-Tiering| Data with unknown, changing, or unpredictable access patterns| 

Move objects between storage classes can be done *manually* or via S3 *lifecycle rules* configuration.

- [x] Security

Encryption:

- SSE-S3: encrypts S3 objects using keys handled & managed by AWS
- SSE-KMS: use AWS Key Management Service to manage encryption keys
- SSE-C: when you want to manage your own encryption keys
- Client Side Encryption: encrypt data outside of AWS before sending it to AWS

VPC Endpoint Gateway:

- Allow traffic to stay within your VPC (instead of going through public web)
- Make sure your private services (AWS SageMaker) can access S3
