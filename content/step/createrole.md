---
title: "Create Role"
chapter: false
weight: 14
---
Create a role with SageMaker and S3 access
To execute this lambdas we are going to need a role SageMaker and S3 permissions.

Follow this link:

https://console.aws.amazon.com/iam/home?region=us-east-1#/roles$new?step=type

* Select Lambda - Next
* In Permissions: Select AmazonSageMakerFullAccess and AmazonS3FullAccess 
* In Tags: Next
* In Name: workshop-role - Create role
* Make sure AmazonSageMakerFullAccess and AmazonS3FullAccess are in place
* Go to the Trust relationships tab and click __Edit trust relationship__
Delete the Policy Document and paste:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "lambda.amazonaws.com",
          "sagemaker.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

* Click on "Update Trust Policy"
