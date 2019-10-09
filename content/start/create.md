---
title: "Create the Notebook"
chapter: false
weight: 20
---

Follow this link to create a notebook instance:

https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances/create

We'll give a name to the notebook instance and in "Notebook instance type" we'll select

* ml.m4.4xlarge

Create an IAM role with access to any s3 bucket and Athena Full Access

The rest of the data, let's leave it by default and click "Create notebook instance".

Now, we wait until the notebook instance is spinned up and click on the "Open Jupyter" link.
