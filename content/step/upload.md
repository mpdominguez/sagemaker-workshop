---
title: "Upload the data to S3"
chapter: false
weight: 10 
---

First you need to create a bucket for this experiment. Upload the data from the following public location to your own S3 bucket. To facilitate the work of the crawler use two different prefixs (folders): one for the billing information and one for reseller.

We can execute this on the console of the Jupyter Notebook or we can just execute it directly:

### Download the data
Your Bucket Name
```
your_bucket = '<YOUR BUCKET NAME e.g., my-sagemaker-bucket>'
!wget https://github.com/mpdominguez/sagemaker-workshop/blob/master/assets/awswrangler-1.9.6-py3.6.egg
!wget https://github.com/mpdominguez/sagemaker-workshop/blob/master/assets/reseller_sm.csv
!wget https://github.com/mpdominguez/sagemaker-workshop/blob/master/assets/billing_sm.csv
```

Now we upload the data to an S3 location
```
import boto3, os
boto3.Session().resource('s3').Bucket(your_bucket).Object(os.path.join('billing', 'billing_sm.csv')).upload_file('billing_sm.csv')
boto3.Session().resource('s3').Bucket(your_bucket).Object(os.path.join('reseller', 'reseller_sm.csv')).upload_file('reseller_sm.csv')
boto3.Session().resource('s3').Bucket(your_bucket).Object(os.path.join('python', 'awswrangler-1.9.6-py3.6.egg')).upload_file('awswrangler-1.9.6-py3.6.egg')
```
