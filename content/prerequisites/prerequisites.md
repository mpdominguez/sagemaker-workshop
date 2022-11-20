+++
title = "Environment"
chapter = false
weight = 10
+++


### AWS Account

In order to complete this workshop you'll need an AWS Account, and an AWS IAM user in that account with at least full permissions to the following AWS services:

- AWS IAM
- Amazon S3
- Amazon SageMaker
- AWS Cloud9

**Use Your Own Account**: The code and instructions in this workshop assume only one student is using a given AWS account at a time. If you try sharing an account with another student, you'll run into naming conflicts for certain resources. You can work around these by appending a unique suffix to the resources that fail to create due to conflicts, but the instructions do not provide details on the changes required to make this work. Use a personal account or create a new AWS account for this workshop rather than using an organization’s account to ensure you have full access to the necessary services and to ensure you do not leave behind any resources from the workshop.

**Costs**: Some, but NOT all, of the resources you will launch as part of this workshop are eligible for the AWS free tier if your account is less than 12 months old. See the [AWS Free Tier page](https://aws.amazon.com/free/) for more details. An example of a resource that is **not** covered by the free tier is the ml.m4.xlarge notebook instance used in some workshops. To avoid charges for endpoints and other resources you might not need after you've finished a workshop, please refer to the [Cleanup Module](../cleanup.html).


### AWS Region

SageMaker is not available in all AWS Regions at this time.  Accordingly, we recommend running this workshop in one of the following supported AWS Regions:  N. Virginia, Oregon, Ohio, or Ireland.

Once you've chosen a region, you should create all of the resources for this workshop there, including a new Amazon S3 bucket and a new SageMaker notebook instance. Make sure you select your region from the dropdown in the upper right corner of the AWS Console before getting started.

![Region selection](/images/region-selection.png)


### Browser

We recommend you use the latest version of Chrome or Firefox to complete this workshop.


### AWS Command Line Interface

To complete certain workshop modules, you'll need the AWS Command Line Interface (CLI) and a Bash environment. You'll use the AWS CLI to interface with SageMaker and other AWS services.

For these workshops, AWS Cloud9 is used to avoid problems that can arise configuring the CLI on your machine. AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It has the AWS CLI pre-installed so you don’t need to install files or configure your laptop to use the AWS CLI. For Cloud9 setup directions for these workshops, see [Cloud9 Setup](cloud9.html). Do NOT attempt to use a locally installed AWS CLI during a live workshop because there is insufficient time during a live workshop to resolve related issues.


### Text Editor

For any workshop module that requires use of the AWS Command Line Interface (see above), you also will need a **plain text** editor for writing Bash scripts. Any editor that inserts Windows or other special characters potentially will cause scripts to fail. AWS Cloud9 includes a text editor.
