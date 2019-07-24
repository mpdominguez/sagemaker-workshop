---
title: "Train Model Lambda"
chapter: false
weight: 15
---

1. Go to the AWS Console and under Services, select Lambda
* Go to the Functions Pane and select Create Function
* Author from scratch
* Name it lambdaModelTrain, choose runtime Python 3.6 and as executing role, select the role you created in the previous step
This lambda function doesn't need to receive any parameters, but it should return the resulting hyperparameter tunning optimization job name, that we will use in the next lambda function to check status, the container it used and that the status is now In Progress...

```
import json
import boto3
import copy
from time import gmtime, strftime


region = boto3.Session().region_name    
smclient = boto3.Session().client('sagemaker')
role = 'arn:aws:iam::452432741922:role/service-role/AmazonSageMaker-ExecutionRole-20181022T121720'


bucket_path='s3://blackb-mggaska-implementation'
prefix = "invoice-forecast"

container = '811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest'


def lambda_handler(event, context):
        
    tuning_job_config = {
    "ParameterRanges": {
      "CategoricalParameterRanges": [],
      "ContinuousParameterRanges": [
        {
          "MaxValue": "1",
          "MinValue": "0",
          "Name": "eta"
        },
        {
          "MaxValue": "2",
          "MinValue": "0",
          "Name": "alpha"
        },
        {
          "MaxValue": "10",
          "MinValue": "1",
          "Name": "min_child_weight"
        }
      ],
      "IntegerParameterRanges": [
        {
          "MaxValue": "20",
          "MinValue": "10",
          "Name": "max_depth"
        }
      ]
    },
    "ResourceLimits": {
      "MaxNumberOfTrainingJobs": 5,
      "MaxParallelTrainingJobs": 5
    },
    "Strategy": "Bayesian",
    "HyperParameterTuningJobObjective": {
      "MetricName": "validation:mae",
      "Type": "Minimize"
    }
    }
  
    training_job_definition = \
      { 
        "AlgorithmSpecification": {
            "TrainingImage": container,
            "TrainingInputMode": "File"
        },
        "RoleArn": role,
        "OutputDataConfig": {
            "S3OutputPath": bucket_path + "/"+ prefix + "/xgboost"
        },
        "ResourceConfig": {
            "InstanceCount": 2,   
            "InstanceType": "ml.m4.xlarge",
            "VolumeSizeInGB": 5
        },
        "StaticHyperParameters": {
            "objective": "reg:linear",
            "num_round": "100",
            "subsample":"0.7",
            "eval_metric":"mae"
        },
        "StoppingCondition": {
            "MaxRuntimeInSeconds": 86400
        },
        "InputDataConfig": [
            {
                "ChannelName": "train",
                "DataSource": {
                    "S3DataSource": {
                        "S3DataType": "S3Prefix",
                        "S3Uri": bucket_path + '/train/',
                        "S3DataDistributionType": "FullyReplicated" 
                    }
                },
                "ContentType": "text/csv",
                "CompressionType": "None"
            },
            {
                "ChannelName": "validation",
                "DataSource": {
                    "S3DataSource": {
                        "S3DataType": "S3Prefix",
                        "S3Uri": bucket_path + '/validation/',
                        "S3DataDistributionType": "FullyReplicated"
                    }
                },
                "ContentType": "text/csv",
                "CompressionType": "None"
            }
            ]
        }
    
    tuning_job_name = prefix + strftime("%Y%m%d%H%M%S", gmtime())
    
    event["container"] = container
    event["stage"] = "Training"
    event["status"] = "InProgress"
    event['name'] = tuning_job_name
    
    smclient.create_hyper_parameter_tuning_job(HyperParameterTuningJobName = tuning_job_name,
                                           HyperParameterTuningJobConfig = tuning_job_config,
                                           TrainingJobDefinition = training_job_definition)

    # output
    return event
```

Test the lambda function
Create a test event with no parameters save and test. In your AWS console under SageMaker>Hyperparamter tunning jobs you should see the HPO runnning.


