---
title: "Cleanup the Workspace"
chapter: false
weight: 10
---
Clean Up:

* Delete Glue Tables https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=tables
* Delete Glue Crawler https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=crawlers 
* Delete Glue ETL https://us-east-1.console.aws.amazon.com/glue/home?region=us-east-1#etl:tab=jobs
* Delete Lambda Functions https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions
* Delete Cloudwatch Rule https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#rules:
* Delete State Machine https://console.aws.amazon.com/states/home?region=us-east-1#/statemachines
* Delete IAM Roles https://console.aws.amazon.com/iam/home?region=us-east-1#/roles
* Delete Endpoint Configurations https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/endpointConfig
* Delete Endpoints https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/endpoints
* Delete Inference Models https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/models
* Terminate Notebook Instance https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances
* Delete Created Bucket https://s3.console.aws.amazon.com/s3/home?region=us-east-1#
* Remove AWSGlueServiceRole-billing-crawler-role https://console.aws.amazon.com/iam/home?region=us-east-1#/roles
* Remove AWSGlueServiceRole-reseller-crawler-role https://console.aws.amazon.com/iam/home?region=us-east-1#/roles
* Run ```drop table implementationdb``` in https://us-east-1.console.aws.amazon.com/athena/home?region=us-east-1#query
