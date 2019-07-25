---
title: "Deploy Model Lambda"
chapter: false
weight: 16
---

Deploy Model In SageMaker: Lambda Function

In this lambda function, we are going to need to use the best training job from the previous step to deploy a predictor.

1. Go to the AWS Console and under Services, select Lambda
2. Go to the Functions Pane and select Create Function
3. Author from scratch
4. Or follow this link https://console.aws.amazon.com/lambda/home?region=us-east-1#/create?firstrun=true
5. Parameters for the function:
    * Name: lambdaModelDeploy
    * Runtime: Python 3.6
    * Executing role: Use an existing role and select the role you created in the previous step (workshop-role) - Create function

The ARN for the variable EXECUTION_ROLE can be found here: https://console.aws.amazon.com/iam/home?region=us-east-1#/roles/workshop-role

```
import boto3
import os

sagemaker = boto3.client('sagemaker')
EXECUTION_ROLE = 'arn:aws:iam::1111111111112:role/service-role/AmazonSageMaker-ExecutionRole-111111111111'
INSTANCE_TYPE = 'ml.m4.xlarge'
container = '811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest'

def lambda_handler(event, context):
    best_training_job = event['best_training_job']
    endpoint = 'demobb-invoice-prediction'
    model_data_url = event['model_data_url']
    print('Creating model resource from training artifact...')
    create_model(best_training_job, container, model_data_url)
    print('Creating endpoint configuration...')
    create_endpoint_config(best_training_job)
    print('There is no existing endpoint for this model. Creating new model endpoint...')
    create_endpoint(endpoint, best_training_job)
    event['stage'] = 'Deployment'
    event['status'] = 'Creating'
    event['message'] = 'Started deploying model "{}" to endpoint "{}"'.format(best_training_job, endpoint)
    return event

def create_model(name, container, model_data_url):
    """ Create SageMaker model.
    Args:
        name (string): Name to label model with
        container (string): Registry path of the Docker image that contains the model algorithm
        model_data_url (string): URL of the model artifacts created during training to download to container
    Returns:
        (None)
    """
    try:
        sagemaker.create_model(
            ModelName=name,
            PrimaryContainer={
                'Image': container,
                'ModelDataUrl': model_data_url
            },
            ExecutionRoleArn=EXECUTION_ROLE
        )
    except Exception as e:
        print(e)
        print('Unable to create model.')
        raise(e)

def create_endpoint_config(name):
    """ Create SageMaker endpoint configuration.
    Args:
        name (string): Name to label endpoint configuration with.
    Returns:
        (None)
    """
    try:
        sagemaker.create_endpoint_config(
            EndpointConfigName=name,
            ProductionVariants=[
                {
                    'VariantName': 'prod',
                    'ModelName': name,
                    'InitialInstanceCount': 1,
                    'InstanceType': INSTANCE_TYPE
                }
            ]
        )
    except Exception as e:
        print(e)
        print('Unable to create endpoint configuration.')
        raise(e)
def create_endpoint(endpoint_name, config_name):
    """ Create SageMaker endpoint with input endpoint configuration.
    Args:
        endpoint_name (string): Name of endpoint to create.
        config_name (string): Name of endpoint configuration to create endpoint with.
    Returns:
        (None)
    """
    try:
        sagemaker.create_endpoint(
            EndpointName=endpoint_name,
            EndpointConfigName=config_name
        )
    except Exception as e:
        print(e)
        print('Unable to create endpoint.')
        raise(e)
```


On your SageMaker console you should see an endpoint with status creating. Once you test the output it should look like this:
You can configure the test with this parameters, taking care of changing the parameters __name__ , __model_data_url__ and __best_training_job__ received in the output of the test of lambdaModelAwait

```
{
  "container": "811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest",
  "stage": "Deployment",
  "status": "Creating",
  "name": "invoice-forecast20190702190151",
  "message": "Started deploying model \"invoice-forecast20190702190151-005-dde9844e\" to endpoint \"demobb-invoice-prediction\"",
  "model_data_url": "s3://blackb-mggaska-implementation/invoice-forecast/xgboost/invoice-forecast20190702190151-005-dde9844e/output/model.tar.gz",
  "best_training_job": "invoice-forecast20190702190151-005-dde9844e"
}
```
This is a good chance to test the Await function this time with Deployment stage.
If you create a new test event on the lambdaAwaitModel you should see a response like this:
Response:
```
{
  "container": "811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest",
  "stage": "Deployment",
  "status": "Creating",
  "name": "invoice-forecast20190702190151",
  "message": "Started deploying model \"invoice-forecast20190702190151-005-dde9844e\" to endpoint \"demobb-invoice-prediction\"",
  "model_data_url": "s3://blackb-mggaska-implementation/invoice-forecast/xgboost/invoice-forecast20190702190151-005-dde9844e/output/model.tar.gz",
  "best_training_job": "invoice-forecast20190702190151-005-dde9844e"
}
```
And you can check the progress in the AWS Console: https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/endpoints

After the model is in service you should see something like this:

```
{
  "container": "811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest",
  "stage": "Deployment",
  "status": "InService",
  "name": "invoice-forecast20190702190151",
  "message": "Deployment completed for endpoint \"demobb-invoice-prediction\".",
  "model_data_url": "s3://blackb-mggaska-implementation/invoice-forecast/xgboost/invoice-forecast20190702190151-005-dde9844e/output/model.tar.gz",
  "best_training_job": "invoice-forecast20190702190151-005-dde9844e"
}
```
