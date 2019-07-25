---
title: "Await Model Lambda"
chapter: false
weight: 16
---

1. Go to the AWS Console and under Services, select Lambda
2. Go to the Functions Pane and select Create Function
3. Author from scratch
4. Or follow this link https://console.aws.amazon.com/lambda/home?region=us-east-1#/create?firstrun=true
5. Parameters for the function:
    * Name: lambdaModelAwait
    * Runtime: Python 3.6
    * Executing role: Use an existing role and select the role you created in the previous step (workshop-role) - Create function

This lambda function, now receives the output of the previous step and allows us to check if the process is done or not. 
If it's done, it returns the name of the best training job.

The name of the training job can be found here:  https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/hyper-tuning-jobs

Previous response (for using in the test event):
```
    {
      "container": "811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest",
      "stage": "Training",
      "status": "InProgress",
      "name": "invoice-forecast11111111111111"
    }
```
In the code editor paste the following code:

```
import boto3
import os

sagemaker = boto3.client('sagemaker')


def lambda_handler(event, context):
    stage = event['stage']
    if stage == 'Training':
        name = event['name']
        training_details = describe_training_job(name)
        print(training_details)
        status = training_details['HyperParameterTuningJobStatus']
        if status == 'Completed':
            s3_output_path = training_details['TrainingJobDefinition']['OutputDataConfig']['S3OutputPath']
            model_data_url = os.path.join(s3_output_path, training_details['BestTrainingJob']['TrainingJobName'], 'output/model.tar.gz')
            event['message'] = 'HPO tunning job "{}" complete. Model data uploaded to "{}"'.format(name, model_data_url)
            event['model_data_url'] = model_data_url
            event['best_training_job'] = training_details['BestTrainingJob']['TrainingJobName']
        elif status == 'Failed':
            failure_reason = training_details['FailureReason']
            event['message'] = 'Training job failed. {}'.format(failure_reason)
    elif stage == 'Deployment':
        name = 'demobb-invoice-prediction'
        endpoint_details = describe_endpoint(name)
        status = endpoint_details['EndpointStatus']
        if status == 'InService':
            event['message'] = 'Deployment completed for endpoint "{}".'.format(name)
        elif status == 'Failed':
            failure_reason = endpoint_details['FailureReason']
            event['message'] = 'Deployment failed for endpoint "{}". {}'.format(name, failure_reason)
        elif status == 'RollingBack':
            event['message'] = 'Deployment failed for endpoint "{}", rolling back to previously deployed version.'.format(name)
    event['status'] = status
    return event


def describe_training_job(name):
    """ Describe SageMaker training job identified by input name.
    Args:
        name (string): Name of SageMaker training job to describe.
    Returns:
        (dict)
        Dictionary containing metadata and details about the status of the training job.
    """
    try:
        response = sagemaker.describe_hyper_parameter_tuning_job(
            HyperParameterTuningJobName=name
        )
    except Exception as e:
        print(e)
        print('Unable to describe hyperparameter tunning job.')
        raise(e)
    return response

def describe_endpoint(name):
    """ Describe SageMaker endpoint identified by input name.
    Args:
        name (string): Name of SageMaker endpoint to describe.
    Returns:
        (dict)
        Dictionary containing metadata and details about the status of the endpoint.
    """
    try:
        response = sagemaker.describe_endpoint(
            EndpointName=name
        )
    except Exception as e:
        print(e)
        print('Unable to describe endpoint.')
        raise(e)
    return response
```
First, you will see a response like
```
Response:
{
  "container": "811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest",
  "stage": "Training",
  "status": "InProgress",
  "name": "invoice-forecast20190702190151"
}
```
Once the training is completed, the state is going to change and you'll see the new status and the name of the best training job.
```
{
  "container": "811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest",
  "stage": "Training",
  "status": "Completed",
  "name": "invoice-forecast20190702190151",
  "message": "HPO tunning job \"invoice-forecast20190702190151\" complete. Model data uploaded to \"s3://blackb-mggaska-implementation/invoice-forecast/xgboost/invoice-forecast20190702190151-005-dde9844e/output/model.tar.gz\"",
  "model_data_url": "s3://blackb-mggaska-implementation/invoice-forecast/xgboost/invoice-forecast20190702190151-005-dde9844e/output/model.tar.gz",
  "best_training_job": "invoice-forecast20190702190151-005-dde9844e"
}
```
