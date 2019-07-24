---
title: "Use Model to Predict"
chapter: false
weight: 17
---

4.5 Use model to predict
Now we are going to use the deployed model to predict. 
This last lambda function doesn't take any parameters, but in this case we need to touch the default parameters of the lambda to configure Max Memory in 1024 MB and Timeout in 15 Mins.
```
import os
import io
import boto3
import json
import csv
from io import StringIO

# grab static variables
ENDPOINT_NAME = 'demobb-invoice-prediction'
runtime= boto3.client('runtime.sagemaker')
bucket = 'blackb-mggaska-implementation'
s3 = boto3.client('s3')
bucket ='blackb-mggaska-implementation'
key = 'to_predict.csv'
def lambda_handler(event, context):
    response = s3.get_object(Bucket=bucket, Key=key)
    content = response['Body'].read().decode('utf-8')
    results = []
    for line in  content.splitlines():
        response = runtime.invoke_endpoint(EndpointName=ENDPOINT_NAME,
                                               ContentType='text/csv',
                                               Body=line)
        result = json.loads(response['Body'].read().decode())
        results.append(result)
        i = 0
    multiLine = ""
    for item in results:
        if (i > 0):
            multiLine = multiLine + '\n'
        multiLine = multiLine + str(item)
        i+=1

    file_name = "predictions.csv"
    s3_resource = boto3.resource('s3')
    s3_resource.Object(bucket, file_name).put(Body=multiLine)


    event['status'] = 'Processed records ' + str(len(results))
    return event
```
