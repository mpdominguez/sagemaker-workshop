---
title: "Create the Notebook"
chapter: false
weight: 20
---

Follow this link to create a notebook instance:

https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances/create

We'll give a name to the notebook instance and in "Notebook instance type" we'll select

* ml.m4.4xlarge

Create an IAM role with access to: 

* Any S3 bucket
* Athena Full Access.

On the Git repositories tab, choose the option "Clone a public Git Repository to this notebook instance only" an paste the following repo url: https://github.com/githubmg/buildon-workshop

![Title Image](https://i.ibb.co/r2RVw50/Captura-de-pantalla-2019-11-05-a-la-s-6-25-03-p-m.png)

The rest of the data, let's leave it by default and click "Create notebook instance".

Now, we wait until the notebook instance is spinned up and click on the "Open Jupyter" link.
