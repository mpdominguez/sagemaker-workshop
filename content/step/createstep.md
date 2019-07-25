---
title: "Create Step Function"
chapter: false
weight: 18
---
Now we have to create a step function to orchestrate the execution of all the previous step.

Follow this link: https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines/create 


This is the resulting diagram:

![STEPFUNCTIONS](/images/steps.png)

For step function we use the ASL markup language.
We have to change all the arn from 1111111111111 to our account number.

```
{
  "StartAt": "ETL",
  "States": {
    "ETL": {
        "Type": "Task",
        "Resource": "arn:aws:states:::glue:startJobRun.sync",
        "Parameters": {
          "JobName": "etlandpipeline"
        },
        "Next": "StartTrainingJob"
      },
      "StartTrainingJob": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:1111111111111:function:lambdaModelTrain",
              "ResultPath": "$",
              "Next": "CheckStatusTraining"
            },
            "CheckStatusTraining": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:1111111111111:function:lambdaModelAwait",
              "ResultPath": "$",
              "Next": "CheckTrainingBranch"
            },
        "CheckTrainingBranch": {
              "Type": "Choice",
              "Choices": [
                {
                  "Or": [{
                      "Variable": "$.status",
                      "StringEquals": "Completed"
                    }
                   ],
                  "Next": "StartDeployment"
                },
                {
                  "Or": [{
                      "Variable": "$.status",
                      "StringEquals": "InProgress"
                    }
                  ],
                  "Next": "WaitStatusTraining"
                }
              ]
            },

            "WaitStatusTraining": {
              "Type": "Wait",
              "Seconds": 60,
              "Next": "CheckStatusTraining"
            },

            "StartDeployment": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:1111111111111:function:lambdaModelDeploy",
              "Next": "CheckStatusDeployment"
            },
            "CheckStatusDeployment": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:1111111111111:function:lambdaModelAwait",
              "ResultPath": "$",
              "Next": "CheckDeploymentBranch"
            },

            "CheckDeploymentBranch": {
              "Type": "Choice",
              "Choices": [
                {
                  "Or": [{
                      "Variable": "$.status",
                      "StringEquals": "Creating"
                    }
                   ],
                  "Next": "WaitStatusDeployment"
                },
                {
                  "Or": [{
                      "Variable": "$.status",
                      "StringEquals": "InService"
                    }
                  ],
                  "Next": "StartPrediction"
                }
              ]
            },
            "WaitStatusDeployment": {
              "Type": "Wait",
              "Seconds": 60,
              "Next": "CheckStatusDeployment"
            },
            "StartPrediction": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:1111111111111:function:lambdaModelPredict",
              "End": true
            }
          }
        }
```
