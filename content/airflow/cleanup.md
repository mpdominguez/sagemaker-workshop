---
title: "Clean up"
chapter: false
weight: 17
---

Now to the final step, cleaning up the resources.

To avoid unnecessary charges on your AWS account do the following:

1. Destroy all of the resources created by the CloudFormation stack in Airflow set up by deleting the stack after you’re done experimenting with it. You can follow the steps here to delete the stack.
2. You have to manually delete the S3 bucket created by the CloudFormation stack because AWS CloudFormation can’t delete a non-empty Amazon S3 bucket.
