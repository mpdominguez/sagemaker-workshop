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
your_bucket = 'sales-prediction-mrtdom'
!wget https://ml-lab-mggaska.s3.amazonaws.com/sales-forecast/billing_sm.csv
!wget https://ml-lab-mggaska.s3.amazonaws.com/sales-forecast/reseller_sm.csv
!wget https://ml-lab-mggaska.s3.amazonaws.com/sales-forecast/awswrangler-0.0b2-py3.6.egg
```

Now we upload the data to an S3 location
```
import boto3, os
boto3.Session().resource('s3').Bucket(your_bucket).Object(os.path.join('billing', 'billing_sm.csv')).upload_file('billing_sm.csv')
boto3.Session().resource('s3').Bucket(your_bucket).Object(os.path.join('reseller', 'reseller_sm.csv')).upload_file('reseller_sm.csv')
boto3.Session().resource('s3').Bucket(your_bucket).Object(os.path.join('python', 'awswrangler-0.0b2-py3.6.egg')).upload_file('awswrangler-0.0b2-py3.6.egg')
```


