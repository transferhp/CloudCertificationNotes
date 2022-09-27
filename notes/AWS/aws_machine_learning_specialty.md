
![exam_logo](../../images/AWS-ML-Specialty.png)

# AWS Machine Learning Specialty

Following notes are taken when I enrolled in Udemy course AWS Certified Machine Learning Specialty 2022 - Hands on.

# Table of Contents
<!-- vscode-markdown-toc -->
* 1. [Data Engineering :rocket:](#DataEngineering:rocket:)
	* 1.1. [S3](#S3)
	* 1.2. [Kinesis](#Kinesis)
	* 1.3. [Glue](#Glue)
* 2. [Data Exploratory Analysis :rocket:](#DataExploratoryAnalysis:rocket:)
	* 2.1. [Data Types](#DataTypes)
	* 2.2. [Data Distributions](#DataDistributions)
	* 2.3. [AWS Athena](#AWSAthena)
	* 2.4. [AWS QuickSight](#AWSQuickSight)
	* 2.5. [Types of visualization](#Typesofvisualization)
	* 2.6. [AWS EMR](#AWSEMR)
	* 2.7. [Imputing Missing Data](#ImputingMissingData)
	* 2.8. [Handling unbalanced data](#Handlingunbalanceddata)
	* 2.9. [Handling outliers](#Handlingoutliers)
	* 2.10. [Binning](#Binning)
* 3. [General Deep Learning :rocket:](#GeneralDeepLearning:rocket:)
	* 3.1. [Activation Functions](#ActivationFunctions)
	* 3.2. [CNN](#CNN)
	* 3.3. [RNN](#RNN)
	* 3.4. [Modern NLP with BERT and GPT, and Transfer Learning](#ModernNLPwithBERTandGPTandTransferLearning)
	* 3.5. [Deep Learning on EC2 and EMR](#DeepLearningonEC2andEMR)
	* 3.6. [Tunning Neural Networks](#TunningNeuralNetworks)
	* 3.7. [Regularization for Neural Networks](#RegularizationforNeuralNetworks)
	* 3.8. [Fixes for vanishing gradients](#Fixesforvanishinggradients)
	* 3.9. [Measuring Models](#MeasuringModels)
	* 3.10. [Ensemble method](#Ensemblemethod)
* 4. [Amazon SageMaker :rocket:](#AmazonSageMaker:rocket:)
	* 4.1. [Built-in algorithms](#Built-inalgorithms)
	* 4.2. [Automatic Model Tuning](#AutomaticModelTuning)
	* 4.3. [Apache Spark](#ApacheSpark)
	* 4.4. [SageMaker Autopilot/AutoML](#SageMakerAutopilotAutoML)
	* 4.5. [SageMaker Model Monitor](#SageMakerModelMonitor)
* 5. [High-level ML services :rocket:](#High-levelMLservices:rocket:)
	* 5.1. [Amazon Comprehend](#AmazonComprehend)
	* 5.2. [Amazon Translate](#AmazonTranslate)
	* 5.3. [Amazon Transcribe](#AmazonTranscribe)
	* 5.4. [Amazon Polly](#AmazonPolly)
	* 5.5. [Amazon Rekognition](#AmazonRekognition)
	* 5.6. [Amazon Forecast](#AmazonForecast)
	* 5.7. [Amazon Lex](#AmazonLex)
	* 5.8. [Amazon Personalize](#AmazonPersonalize)
	* 5.9. [TorchServe](#TorchServe)
	* 5.10. [AWS Neuron](#AWSNeuron)
	* 5.11. [AWS Panorama](#AWSPanorama)
	* 5.12. [Contact Lens](#ContactLens)
	* 5.13. [Amazon Augmented AI](#AmazonAugmentedAI)
* 6. [ML Implementation and Operations :rocket:](#MLImplementationandOperations:rocket:)
	* 6.1. [SageMaker on the edge](#SageMakerontheedge)
	* 6.2. [SageMaker Security practices](#SageMakerSecuritypractices)
		* 6.2.1. [Protect data at rest in SageMaker](#ProtectdataatrestinSageMaker)
		* 6.2.2. [Protect data at transit in SageMaker](#ProtectdataattransitinSageMaker)
	* 6.3. [SageMaker VPC](#SageMakerVPC)
	* 6.4. [SageMaker logging and monitoring](#SageMakerloggingandmonitoring)
	* 6.5. [SageMaker resource management](#SageMakerresourcemanagement)
		* 6.5.1. [Instance types practices](#Instancetypespractices)
		* 6.5.2. [Availability Zones (AZ) practices](#AvailabilityZonesAZpractices)
	* 6.6. [Inference Pipeline](#InferencePipeline)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

##  1. <a name='DataEngineering:rocket:'></a>Data Engineering :rocket:

###  1.1. <a name='S3'></a>S3

:white_check_mark: Overview

* A storage for saving objects (files), Max object size is 5TB
* Objects (files) are organized in **buckets**(directories), buckets must have a *globally unique name*
* Objects (files) have a Key, the key is the **FULL** path. e.g <my_bucket>/my_folder/my_file.txt
* Supports partitioning, object tags (key/value pair)
* ideal candidate for building Data Lake

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

* SSE-S3: encrypts S3 objects using keys handled & managed by AWS
* SSE-KMS: use AWS Key Management Service to manage encryption keys
* SSE-C: when you want to manage your own encryption keys
* Client Side Encryption: encrypt data outside of AWS before sending it to AWS

VPC Endpoint Gateway:

* Allow traffic to stay within your VPC (instead of going through public web)
* Make sure your private services (AWS SageMaker) can access S3

###  1.2. <a name='Kinesis'></a>Kinesis

:white_check_mark: Overview

* Alternative to Apache Kafka
* Works for "real-time" big data (aka streaming data)
* Data automatically replicated to 3 AWS Availability Zone (AZ)
* Includes 4 key services:
* [x] **Kinesis Streams**: low latency streaming ingest at scale
* [x] **Kinesis Analytics**: perform real-time analytics on stream using SQL
* [x] **Kinesis Firehose**: Load streams into S3, Redshift, ElasticSearch & Splunk
* [x] **Kinesis Video Streams**: stream videos in real-time

![Kinesis Structure](../../images/Kinesis%20Structure.png)

:white_check_mark: Kinesis Streams (<mark>real-time application</mark>)

* Divide streams in ordered **Shards/Partitions**
* Data retention is 24 hours (default), up to 365 days, which supports data replay capability
* Immutability (data cannot be deleted once inserted)
* Records up to 1MB in size (Great for *Small and fast dataset*)
* Scales ONLY if you add shards over time

:white_check_mark: Kinesis Data Analytics (<mark>Streaming data ETL, Continuous metric generation</mark>)

* Serverless; scales automatically
* Query streaming data using SQL/Flink
* User IAM permission to access streaming source and destinations
* Supports schema discovery
* Contains 2 machine learning algorithms: RANDOM_CUT_FOREST (anomaly detection for numeric columns) and HOTSPOTS (locate dense regions)

:white_check_mark: Kinesis Firehose (<mark>Delivery service</mark>)

* Write out data in big batch and deliver data to target destination *near real-time*
* Fully managed service, auto-scaling
![Kinesis Firehose](../../images/Kinesis%20Firehose.png)

:white_check_mark: Kinesis Video Streams

* Keep data for 1 hour to 10 years
* Video playback capability

###  1.3. <a name='Glue'></a>Glue

:white_check_mark: Glue Data Catalog

* Metadata repository for all your tables
* Automated schema inference
* Schemas are versioned

:white_check_mark: Glue Crawlers

* Help build Glue Data Catalog
* Go through your data to infer schemas ans partitions
* Work for: S3, Redshift, Amazon RDS
* Run on a Schedule or on Demand
* Need an IAM role/credentials to access the data source

:white_check_mark: Glue ETL

* Runs on a serverless Apache Spark Cluster
* Job can be written in Python, Scala, Spark or Pyspark
* Glue Scheduler to schedule the jobs
* Glue Triggers to automate job runs based on "events"

:white_check_mark: Redshift

* Data warehouse for SQL analytics (OLAP)
* Load data from S3 to Redshift
* Use Redshift Spectrum to query data directly in S3 (No loading)

:white_check_mark: RDS, Aurora

* Relational data store, SQL (OLTP)
* Must provision servers in advance

:white_check_mark: DynamoDB

* NoSQL database

:white_check_mark: ElasticSearch

* Index for your data
* Search capability

:white_check_mark: ElatiCache

* data cache technology

:white_check_mark: AWS Batch

* Batch jobs run as Docker containers - not just for data
* Manages EC2 instances for you

:white_check_mark: AWS Data Pipelines

* A specialized workflow for **working with data**
* Orchestration of data ETL jobs
* Directly work with S3, EMR, DynamoDB, Redshift or RDS

:white_check_mark: AWS Step Functions (Orchestrator)

* A *genetic* way to design and orchestrate workflows (NOT ONLY for data)
* Provides Error handling and Retry mechanism outside of the code
* Provides ability to Audit the history of workflows
* Can "wait" for arbitrary amount of time

##  2. <a name='DataExploratoryAnalysis:rocket:'></a>Data Exploratory Analysis :rocket:

###  2.1. <a name='DataTypes'></a>Data Types

:white_check_mark: Numerical (quantitative data)

* Discrete data: integer based
* Continuous data: infinite number of possible values

:white_check_mark: Categorical (qualitative data)

:white_check_mark: Ordinal

* mixture of numerical and categorical
* categorical data has mathematical meaning (eg. movie rating of 5 is better than 1)

###  2.2. <a name='DataDistributions'></a>Data Distributions

:white_check_mark: Normal distribution

* Bell curve that centred around 0
* Works with continuous data
* Gives the probability of a data point falling within some given range of a given value

:white_check_mark: Poisson Distribution

* Works with discrete data

:white_check_mark: Binomial Distribution

* Multiple trials of discrete events, such as flipping a coin
* Works with discrete data

:white_check_mark: Bernoulli Distribution

* Single trial (special case of binomial distribution)
* Works with discrete data

###  2.3. <a name='AWSAthena'></a>AWS Athena

* Serverless interactive queries of S3 data (No need to load data!)
* Supports CSV, JSON, ORC, Parquet, Avro
* Save money by using columnar formats (ORC, Parquet)
* Uses for ad-hoc query

###  2.4. <a name='AWSQuickSight'></a>AWS QuickSight

* A serverless visualization tool
* Allows *limited* ETL
* Dataset are imported into **SPICE** (10GB of SPICE per user)
* Machine Learning capabilities in QuickSight: Anomaly detection, Forecasting and Auto-narratives

###  2.5. <a name='Typesofvisualization'></a>Types of visualization

* Bar charts (For comparison or distribution)
* Line charts (For changes over time)
* Scatter plots, Heat maps (For correlation)
* Pie charts, Tree maps (For aggregation)
* Pivot table (For tabular data)

###  2.6. <a name='AWSEMR'></a>AWS EMR

* Managed Hadoop framework on EC2 instances
* EMR clusters include three type of nodes: **master** node (manges the cluster), **core** node (hosts HDFS data) and task node (runs task without hosting data, good use of *spot instances*)
* Transient cluster (spin up Spot instances for temporary capacity) vs Long-Running cluster (use reserved instances for saving cost)

###  2.7. <a name='ImputingMissingData'></a>Imputing Missing Data

:x: Replacement with *mean* value

* Only works on column level, misses **correlations between columns**
* Can't use on categorical features
* Bias the whole feature value distribution (Median maybe a good choice when outliers are present)

:x: Dropping

* Always a bad choice but as a quick and dirty solution

:white_check_mark: Imputing with Machine Learning

* KNN (Find K similar rows and average their values), but only works for numerical data as it relies on certain *distance* metric
* Deep Learning (Train a DL model to impute data), works well for categorical data but it is complicated
* Regression (**MICE**, Multiple Imputation by Chained Equations), find linear or non-linear relationships between the missing feature and other features

:white_check_mark: Collect more better quality data

###  2.8. <a name='Handlingunbalanceddata'></a>Handling unbalanced data

* Oversampling: Randomly copy minority samples
* Undersampling: Remove majority samples (*Throwing away data is not always a goos choice*)
* SMOTE: Both generates new samples of minority class using KNN and undersamples majority class

###  2.9. <a name='Handlingoutliers'></a>Handling outliers

* Identify how extreme a data point is by checking out *"how many standard deviations" away from the mean it is*
* Responsibly remove outliers from your training data

###  2.10. <a name='Binning'></a>Binning

* Transform numerical data to ordinal data
* Bucket numerical data together based on ranges of values
* Quantile binning (**even sizes in each bin**)

##  3. <a name='GeneralDeepLearning:rocket:'></a>General Deep Learning :rocket:

###  3.1. <a name='ActivationFunctions'></a>Activation Functions

* Define the output of a node/neuron given its input signals
* Non-linear activation functions: works for back-propagation and multi-layers (e.g. ReLU, Leaky ReLU, PReLU and Maxout)
* Softmax (used on the final output layer for multiple classification problem)

###  3.2. <a name='CNN'></a>CNN

* "feature-location invariant", which means it can find features by scanning the whole data (e.g. image)
* Works for data in the format of width x length x color channels

###  3.3. <a name='RNN'></a>RNN

* Works for time-series data or data that consists of sequences of arbitrary length
* LSTM, GRU (simplified LSTM)

###  3.4. <a name='ModernNLPwithBERTandGPTandTransferLearning'></a>Modern NLP with BERT and GPT, and Transfer Learning

* Transformer (adopts "self-attention" mechanism)
* BERT (transformer-based natural language processing model)
* GPT (Generative Pre-trained Transformer)
* Transfer Learning (takes pre-trained model and fine-tune on it for our own use case)

    :white_check_mark: Use a low learning rate

    :white_check_mark: Add new layers to the top of a frozen model

###  3.5. <a name='DeepLearningonEC2andEMR'></a>Deep Learning on EC2 and EMR

* EMR supports MXNet and GPU instance types (P3, P2 and G3)
* EC2 instances can be launched using **Deep Learning AMI** to train DL models

###  3.6. <a name='TunningNeuralNetworks'></a>Tunning Neural Networks

* Batch size

    :white_check_mark: small batch size tend to not got stuck in local minimum

* Learning rate

    :x: small learning rate increases training time

    :x: large learning rate overshoots the optimal solution

###  3.7. <a name='RegularizationforNeuralNetworks'></a>Regularization for Neural Networks

* Dropout (drop neurons at random at each training step)
* Early stopping

###  3.8. <a name='Fixesforvanishinggradients'></a>Fixes for vanishing gradients

:white_check_mark: Uses ReLU as activation function

:white_check_mark: Uses specific architectures (e.g. LSTM and ResNet)

:white_check_mark: multi-level heirarchy (breaks whole network into sub-networks and trained individually)

###  3.9. <a name='MeasuringModels'></a>Measuring Models

* Precision (AKA Correct Positives)

    :white_check_mark: uses it if you care more *False Positives*

* Recall (Sensitivity, True Positive Rate)

    :white_check_mark: uses it if you care more *False Negatives*
  
* ROC (plot of true positive rate vs false positive rate at different threshold)

* AUC (Area under ROC curve)

    :white_check_mark: uses to compare different classifiers

* Specificity (True negative rate)

* F1 score

    :white_check_mark: use it if you care both *precision* AND *recall*

###  3.10. <a name='Ensemblemethod'></a>Ensemble method

* Bagging

    Train N different models by random sampling the original data with replacement into N folds.

    :white_check_mark: avoid overfitting

    :white_check_mark: easier to parallelize

* Boosting

    Train models in sequential. Each model will take into account the previous model's prediction and adjust weights to each data point.

    :white_check_mark: achieve high accuracy

##  4. <a name='AmazonSageMaker:rocket:'></a>Amazon SageMaker :rocket:

* Data preparation

  * Data sources:

    **S3** (RecordIO/Protobuf for built-in algorithms), can also load from *Athena*, *EMR*, *Redshift* and Amazon *Keyspaces DB*

  * Processing tools:

    Spark, scikit_learn, numpy and pandas (used in notebook)

* Training

  * training data (*URL of S3 bucket*)
  * ML compute resources
  * output location (*URL of S3 bucket*)
  * training code (comes from a Docker image registered with *ECR* path)
    * Built-in algorithms
    * Spark MLLib
    * Custom Python Tensorflow/MXNet code
    * Custom code lives in Docker image
    * Algorithm purchased from AWS market

* Deployment

  * model artifact saved to S3
  * individual prediction (**Persistent endpoint**)
  * batch prediction (**SageMaker Batch Transform**)
  * Lots of cool options:
    * Inference Pipelines for more complex processing
    * deploying to edge devices (**SageMaker Neo**)
    * accelerating deep learning inference (**Elastic Inference**)
    * automatic scaling (increase # of endpoints)

###  4.1. <a name='Built-inalgorithms'></a>Built-in algorithms

* Linear Learner

  * input:
    * RecordIO in float 32 (Recommended)
    * CSV (first column is the label followed by feature data)
    * Pipe mode works best for large dataset

  * normalize data upfront or let linear learner normalize your data for you

  * shuffle data to get good results

  * offer L1 and L2 regularization

  * Hyperparameters: **multi-class weights**, **learning rate** and **batch size**

  * benefits from more than one machine other than multiple GPU on one machine

* XGBoost

  * input:
    * CSV
    * LibSVM
    * RecordIO
    * Parquet

* Seq2Seq

  * input:
    * RecordIO
    * Tokens are integer

  * **blue score** and **perplexity** are well suited for measuring machine translation problem

  * only support training on one machine

* DeepAR

  * used for forecasting one-dimensional time series data

  * input:
    * Json Line format
    * Gzip file
    * Parquet

* BlazingText

  * used for supervised text classification (sentence) and Word2Vec (words)

  * input:

    For text classification:
    * One sentence per line
    * First word in the sentence is the string *\_\_label\_\_* followed by the label

    For word2vec:
    * a text file with one training sentence per line

* Object2Vect

  * similar to word2vec, but generalized to handle things other than words

  * input:
    * data must be tokenized into integers

* Object Detection

  * identify all objects in an image with bounding boxes, classes are accompanied by confidence scores

  * input:

    * RecordIO
    * image format (JSON file for annotation data)

* Image Classification

  * assign one or more labels to an image

  * input:

    * MXNet RecordIO
    * raw jpg or png images (requires .lst files)
    * augmented manifested image format enable Pipe mode

* Semantic Segmentation

  * pixel-level object classification

  * input:

    * JPG or PNG with annotations
    * augmented manifested image with Pipe mode

* Random Cut Forest

  * used for anomaly detection
  * **unsupervised** algorithm

  * input:
    * RecordIO or CSV (File or Pipe mode)

  * uses CPU for training and inference

* Neural Topic Modelling

  * classifies documents into topics
  * **unsupervised** algorithm (topics are not known in advance)

  * input:
    * RecordIO or CSV (File or Pipe mode)
    * words must be tokenized into integers (cannot pass text file directly)

* LDA

  * **unsupervised** clustering algorithm (not deep learning)
  * input:
    * RecordIO (Pipe mode)
    * CSV

* KNN

  * k-nearest neighbors (classification or regression)
  * input:
    * RecordIO or CSV (first column is label)
    * File or Pipe mode

* K-means

  * **unsupervised** clustering algorithm that divides data into K groups
  * input:
    * RecordIO or CSV (File or Pipe mode)

* PCA

  * used for dimension reduction
  * **unsupervised** algorithm
  * input:
    * RecordIO or CSV (File or Pipe mode)

* Factorization Machine

  * **supervised** algorithm to deal with *sparse* data
  * limited to pair-wise interactions (e.g. user -> item)
  * input:
    * RecordIO with float 32 format
  * application: recommender system

* IP Insights

  * **unsupervised** algorithm for IP usage pattern (neural network underhood)
  * input:
    * CSV only (entity and IP address)

* Reinforcement Learning
  
  * used for making agent explores "space" more efficiently
  * Q-learning
    * start off with Q values of 0
    * bad things happen after a given state/action, reduces Q
    * rewards happen after a given state/action, increases Q
  * distributed training with multi-core and multi-instance

###  4.2. <a name='AutomaticModelTuning'></a>Automatic Model Tuning

* learns as it goes (don't have to try every possible combination)
* best practices:

    :x: don't optimize too many hyperparameters at once

    :x: limit your value ranges

    :x: don't run too many training jobs concurrently

###  4.3. <a name='ApacheSpark'></a>Apache Spark

* connect a SageMaker notebook to a remote EMR cluster running spark
* call fit on *SageMakerEstimator* (KMeans, PCA, XGBoost) to get a *SageMakerModel*
* call transform on the *SageMakerModel* to make inferences

###  4.4. <a name='SageMakerAutopilotAutoML'></a>SageMaker Autopilot/AutoML

* automates algorithm selection, data preprocessing and model tuning

###  4.5. <a name='SageMakerModelMonitor'></a>SageMaker Model Monitor

* get alerts via CloudWatch
* visualize data drift, detect anomalies & outliers
* no code needed
* integrates with **Clarify** to detect potential bias

##  5. <a name='High-levelMLservices:rocket:'></a>High-level ML services :rocket:

###  5.1. <a name='AmazonComprehend'></a>Amazon Comprehend

* used for Natural Language Processing stuff

  * detect entities
  * detect key phrase
  * sentiment analysis
  * detect syntax

###  5.2. <a name='AmazonTranslate'></a>Amazon Translate

* Provides deep learning based **translation** services

###  5.3. <a name='AmazonTranscribe'></a>Amazon Transcribe

* Provides **Speech to Text** services

###  5.4. <a name='AmazonPolly'></a>Amazon Polly

* Provides **Text to Speech** services
* internally use *Lexicons* to customize pronunciation of specific words or phrases, use *SSML* to control how the text words been pronounced and use *Speech Marks*to encode when sentences/words starts and ends in the audio stream. 

###  5.5. <a name='AmazonRekognition'></a>Amazon Rekognition

* Provides **Computer Vision** services

###  5.6. <a name='AmazonForecast'></a>Amazon Forecast

* Provides **time-series analysis**
* has following algorithms:
  
  * CNN-QR (CNN backend)

    :white_check_mark: best for large dataset

    :white_check_mark: accepts related historical time series data & *metadata*

  * DeepAR+ (RNN backend)

    :white_check_mark: best for large datasets

    :white_check_mark: accepts related forward-looking time series data & *metadata* 

  * Prophet

    :white_check_mark: additive model with non-linear trends and seasonality
  * NPTS

    :white_check_mark: best for non-parametric time series data

    :white_check_mark: good for *sparse* data
  * ARIMA

    :white_check_mark: best for simple dataset (< 100 time series data)
  * ETS 

    :white_check_mark: best for simple dataset (< 100 time series data)

###  5.7. <a name='AmazonLex'></a>Amazon Lex

* Natural-language chatbot engine

###  5.8. <a name='AmazonPersonalize'></a>Amazon Personalize

* Fully-managed recommender engine
* provides API access (Feed in data via S3/API with explicit schema defined in Avro format)
* get recommendations/personalized ranking

###  5.9. <a name='TorchServe'></a>TorchServe

* Model serving framework for Pytorch

###  5.10. <a name='AWSNeuron'></a>AWS Neuron

* SDK for optimizing ML inferences on AWS Inferentia chips

###  5.11. <a name='AWSPanorama'></a>AWS Panorama

* Brings computer vision to the edge cameras

###  5.12. <a name='ContactLens'></a>Contact Lens

* Provides customer support in call center

###  5.13. <a name='AmazonAugmentedAI'></a>Amazon Augmented AI

* Human review of ML predictions

##  6. <a name='MLImplementationandOperations:rocket:'></a>ML Implementation and Operations :rocket:

* All models in SageMaker are hosted in Docker containers
```python
from sagemaker.estimator import Estimator

estimator=Estimator(image_name="YOUR IMAGE NAME",
                    ROLE="SageMakerRole",
                    train_instance_count=1,
                    train_instance_type="local")
estimator.fit()
```
* Structure of a training container
```
/opt/ml
|---input
|   |---config
|   |   |---hyperparameters.json
|   |   |---resourceConfig.json
|   |
|   |---data
|       |---<channel_name>
|           |---<input data>
|
|---model
|
|---code
|   |---<script files>
|
|---output
    |---failure

```
* Structure of a deployment container
```
/opt/ml
|----model
     |---<model files>
```
* Test out multiple models on live traffic using **Production Variants** (A/B tests)

###  6.1. <a name='SageMakerontheedge'></a>SageMaker on the edge

* SageMaker Neo 
  * consists of a **compiler** and **runtime** library
  * optimizes code for specific edge devices
  * paris with **AWS IoT Greengrass** (train a model on cloud SageMaker, compile the model with Neo and deploy it to actual edge devices using IoT Greengrass)

###  6.2. <a name='SageMakerSecuritypractices'></a>SageMaker Security practices

* use IAM
* use MFA
* use TLS/SSL to connect everything
* use *CloudTrail* to log API and use activity
* use encryption

####  6.2.1. <a name='ProtectdataatrestinSageMaker'></a>Protect data at rest in SageMaker

* use AWS Key Management Service (KMS) to encrypt everything in SageMaker (notebooks, SageMaker jobs, everything under `opt/ml` and `tmp` in Docker container)
* use standard S3 encryption to encrypt S3 bucket (training data and hosting models) 


####  6.2.2. <a name='ProtectdataattransitinSageMaker'></a>Protect data at transit in SageMaker

* use TLS/SSL to encrypt all traffic
* IAM roles are assigned to SageMaker to access resources
* inter-container traffic encryption (can increase training time and cost with deep learning)

###  6.3. <a name='SageMakerVPC'></a>SageMaker VPC

* training jobs are ran in a Virtual Private Cloud (VPC)
* training jobs read data from S3 (If use private VPC, then need to set up S3 VPC endpoint)
* notebooks are internet-enabled by default (If disabled, your VPC needs an interface endpoint or NAT Gateway to allow outbound connections)
* Training and Inference containers are internet-enabled by default (can be turned off for network isolation)

###  6.4. <a name='SageMakerloggingandmonitoring'></a>SageMaker logging and monitoring

* use **CloudWatch** to log, monitor and set alarm
* use **CloudTrail** to record actions from users, roles and services within SageMaker (log files are sent to S3 for auditing)

###  6.5. <a name='SageMakerresourcemanagement'></a>SageMaker resource management

####  6.5.1. <a name='Instancetypespractices'></a>Instance types practices

* use GPU instances (P2 or P3) for training deep learning models
* use compute instances (C4 or C5) for inference
* use **EC2 Spot instances** for training (save up to 90% cost), but spot instances can be interrupted!
* use **Elastic Inference** to reduce deep learning inference cost (deploy on CPU instance but with a Elastic Inference accelerator)

####  6.5.2. <a name='AvailabilityZonesAZpractices'></a>Availability Zones (AZ) practices

* distribute instances across availability zones
* deploy multiple instances for each production endpoint (resilient to failure)
* configure VPC with at least two subnets, each in a different AZ

###  6.6. <a name='InferencePipeline'></a>Inference Pipeline

* use to chain multiple inference containers into one pipeline of results