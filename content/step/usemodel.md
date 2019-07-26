---
title: "Use Model to Predict"
chapter: false
weight: 17
---

In this lambda function,  we are going to use the deployed model to predict. 

 1. Go to the AWS Console and under Services, select Lambda
 2. Go to the Functions Pane and select Create Function
 3. Author from scratch
 4. Or follow this link https://console.aws.amazon.com/lambda/home?region=us-east-1#/create?firstrun=true
 5. Parameters for the function:
     * Name: lambdaModelPredict
     * Runtime: Python 3.6
     * Executing role: Use an existing role and select the role you created in the previous step (workshop-role) - Create function

This last lambda function doesn't take any parameters, but in this case we need to touch the default parameters of the lambda to configure Max Memory in 1024 MB and Timeout in 15 Mins.

__Change the variable bucket for your bucket name__
```
import os
import io
import boto3
import json
import csv
from io import StringIO

# grab static variables
sagemaker = boto3.client('sagemaker')
ENDPOINT_NAME = 'demobb-invoice-prediction'
runtime= boto3.client('runtime.sagemaker')
bucket = 'blackb-mggaska-implementation'
s3 = boto3.client('s3')
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
    # Deleting Endpoint
    sagemaker.delete_endpoint(EndpointName=ENDPOINT_NAME)
    return event
```
