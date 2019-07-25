---
title: "Schedule the State Machine"
chapter: false
weight: 18
---
Now we have to create a step function to orchestrate the execution of all the previous step.

Follow this link: https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines/create 

For step function we use the ASL markup language.

This is the resulting diagram:

![STEPFUNCTIONS](/images/steps.png)

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
              "Resource": "arn:aws:lambda:us-east-1:452432741922:function:lambdaModelTrain",
              "ResultPath": "$",
              "Next": "CheckStatusTraining"
            },
            "CheckStatusTraining": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:452432741922:function:lambdaModelAwait",
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
              "Resource": "arn:aws:lambda:us-east-1:452432741922:function:lambdaModelDeploy",
              "Next": "CheckStatusDeployment"
            },
            "CheckStatusDeployment": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:us-east-1:452432741922:function:lambdaModelAwait",
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
              "Resource": "arn:aws:lambda:us-east-1:452432741922:function:lambdaModelPredict",
              "End": true
            }
          }
        }
```
