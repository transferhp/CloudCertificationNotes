![exam_logo](../../images/AWS-ML-Specialty.png)

# AWS Machine Learning Specialty

# Data Engineering :rocket:

## S3

:white_check_mark: Overview

- A storage for saving objects (files), Max object size is 5TB
- Objects (files) are organized in **buckets**(directories), buckets must have a *globally unique name*
- Objects (files) have a Key, the key is the **FULL** path. e.g <my_bucket>/my_folder/my_file.txt
- Supports partitioning, object tags (key/value pair)
- ideal candidate for building Data Lake

:white_check_mark: Storage class

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

:white_check_mark: Security

Encryption:

- SSE-S3: encrypts S3 objects using keys handled & managed by AWS
- SSE-KMS: use AWS Key Management Service to manage encryption keys
- SSE-C: when you want to manage your own encryption keys
- Client Side Encryption: encrypt data outside of AWS before sending it to AWS

VPC Endpoint Gateway:

- Allow traffic to stay within your VPC (instead of going through public web)
- Make sure your private services (AWS SageMaker) can access S3

### Kinesis

:white_check_mark: Overview

- Alternative to Apache Kafka
- Works for "real-time" big data (aka streaming data)
- Data automatically replicated to 3 AWS Availability Zone (AZ)
- Includes 4 key services:
- [x] **Kinesis Streams**: low latency streaming ingest at scale
- [x] **Kinesis Analytics**: perform real-time analytics on stream using SQL
- [x] **Kinesis Firehose**: Load streams into S3, Redshift, ElasticSearch & Splunk
- [x] **Kinesis Video Streams**: stream videos in real-time

![Kinesis Structure](../../images/Kinesis%20Structure.png)

:white_check_mark: Kinesis Streams (<mark>real-time application</mark>)

- Divide streams in ordered **Shards/Partitions**
- Data retention is 24 hours (default), up to 365 days, which supports data replay capability
- Immutability (data cannot be deleted once inserted)
- Records up to 1MB in size (Great for *Small and fast dataset*)
- Scales ONLY if you add shards over time

:white_check_mark: Kinesis Data Analytics (<mark>Streaming data ETL, Continuous metric generation</mark>)

- Serverless; scales automatically
- Query streaming data using SQL/Flink
- User IAM permission to access streaming source and destinations
- Supports schema discovery
- Contains 2 machine learning algorithms: RANDOM_CUT_FOREST (anomaly detection for numeric columns) and HOTSPOTS (locate dense regions)

:white_check_mark: Kinesis Firehose (<mark>Delivery service</mark>)

- Write out data in big batch and deliver data to target destination *near real-time*
- Fully managed service, auto-scaling
![Kinesis Firehose](../../images/Kinesis%20Firehose.png)

:white_check_mark: Kinesis Video Streams

- Keep data for 1 hour to 10 years
- Video playback capability

### Glue

:white_check_mark: Glue Data Catalog

- Metadata repository for all your tables
- Automated schema inference
- Schemas are versioned

:white_check_mark: Glue Crawlers

- Help build Glue Data Catalog
- Go through your data to infer schemas ans partitions
- Work for: S3, Redshift, Amazon RDS
- Run on a Schedule or on Demand
- Need an IAM role/credentials to access the data source

:white_check_mark: Glue ETL

- Runs on a serverless Apache Spark Cluster
- Job can be written in Python, Scala, Spark or Pyspark
- Glue Scheduler to schedule the jobs
- Glue Triggers to automate job runs based on "events"

:white_check_mark: Redshift

- Data warehouse for SQL analytics (OLAP)
- Load data from S3 to Redshift
- Use Redshift Spectrum to query data directly in S3 (No loading)

:white_check_mark: RDS, Aurora

- Relational data store, SQL (OLTP)
- Must provision servers in advance

:white_check_mark: DynamoDB

- NoSQL database

:white_check_mark: ElasticSearch

- Index for your data
- Search capability

:white_check_mark: ElatiCache

- data cache technology

:white_check_mark: AWS Batch

- Batch jobs run as Docker containers - not just for data
- Manages EC2 instances for you

:white_check_mark: AWS Data Pipelines

- A specialized workflow for **working with data**
- Orchestration of data ETL jobs
- Directly work with S3, EMR, DynamoDB, Redshift or RDS

:white_check_mark: AWS Step Functions (Orchestrator)

- A *genetic* way to design and orchestrate workflows (NOT ONLY for data)
- Provides Error handling and Retry mechanism outside of the code
- Provides ability to Audit the history of workflows
- Can "wait" for arbitrary amount of time

## Data Exploratory Analysis

### Data Types

:white_check_mark: Numerical (quantitative data)

- Discrete data: integer based
- Continuous data: infinite number of possible values

:white_check_mark: Categorical (qualitative data)

:white_check_mark: Ordinal 

- mixture of numerical and categorical
- categorical data has mathematical meaning (eg. movie rating of 5 is better than 1)

### Data Distributions

:white_check_mark: Normal distribution 

- Bell curve that centred around 0
- Works with continuous data
- Gives the probability of a data point falling within some given range of a given value

:white_check_mark: Poisson Distribution

- Works with discrete data

:white_check_mark: Binomial Distribution

- Multiple trials of discrete events, such as flipping a coin
- Works with discrete data 

:white_check_mark: Bernoulli Distribution

- Single trial (special case of binomial distribution)
- Works with discrete data

### AWS Athena

- Serverless interactive queries of S3 data (No need to load data!)
- Supports CSV, JSON, ORC, Parquet, Avro
- Save money by using columnar formats (ORC, Parquet)
- Uses for ad-hoc query

### AWS QuickSight

- A serverless visualization tool
- Allows *limited* ETL
- Dataset are imported into **SPICE** (10GB of SPICE per user)
- Machine Learning capabilities in QuickSight: Anomaly detection, Forecasting and Auto-narratives

### Types of visualization

- Bar charts (For comparison or distribution)
- Line charts (For changes over time)
- Scatter plots, Heat maps (For correlation)
- Pie charts, Tree maps (For aggregation)
- Pivot table (For tabular data)

### AWS EMR

- Managed Hadoop framework on EC2 instances
- EMR clusters include three type of nodes: **master** node (manges the cluster), **core** node (hosts HDFS data) and task node (runs task without hosting data, good use of *spot instances*)
- Transient cluster (spin up Spot instances for temporary capacity) vs Long-Running cluster (use reserved instances for saving cost) 

### Imputing Missing Data

:x: Replacement with *mean* value

- Only works on column level, misses **correlations between columns**
- Can't use on categorical features
- Bias the whole feature value distribution (Median maybe a good choice when outliers are present)

:x: Dropping

- Always a bad choice but as a quick and dirty solution

:white_check_mark: Imputing with Machine Learning

- KNN (Find K similar rows and average their values), but only works for numerical data as it relies on certain *distance* metric
- Deep Learning (Train a DL model to impute data), works well for categorical data but it is complicated
- Regression (**MICE**, Multiple Imputation by Chained Equations), find linear or non-linear relationships between the missing feature and other features

:white_check_mark: Collect more better quality data

### Handling unbalanced data

- Oversampling: Randomly copy minority samples
- Undersampling: Remove majority samples (*Throwing away data is not always a goos choice*)
- SMOTE: Both generates new samples of minority class using KNN and undersamples majority class

### Handling outliers

- Identify how extreme a data point is by checking out *"how many standard deviations" away from the mean it is* 
- Responsibly remove outliers from your training data

### Binning

- Transform numerical data to ordinal data
- Bucket numerical data together based on ranges of values
- Quantile binning (**even sizes in each bin**)
