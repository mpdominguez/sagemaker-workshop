---
title: "Deploy the App"
chapter: false
weight: 3
---
## Deploy the "Video Recommendation" Application

Whilst this application could be deployed anywhere, it uses both an EC2 Amazon Machine Image (AMI) and RDS Snapshot that have been stored in the North Virgina Region of AWS (us-east-1). Hence, please make sure that the Region selected in the AWS Console is alway US East (N.Virginia), as shown in the following diagram. The workshop will only function correctly if the EC2 configuration, CloudFormation template executiion and SageMaker notebook are all using this AWS Region.

![ChangeRegion](/images/changeRegion.png)

The application will run on an EC2 instance, but at some point we will need to connect to the server in order to carry out some configuration task. To do this we need to have an EC2 Key Pair configured on the server that you also have access to on your computer; hence, we need to create and download a new one. Click on EC2 from the list of all services by entering EC2 into the Find services box. This will bring you to the Amazon EC2 console home page.

![consoleEC2Select](/images/consoleEC2Select.png)

On the left-hand menu scroll down until you see Key Pairs and select it, and in the resulting dialog click on the Create Key Pair button. This will bring up a Create Key Pair dialog, where you need to enter the name of a new key pair - call it myLabKey and hit Create. This should automatically download the file, or you may need to manually do so.

![createKeyPair](/images/createKeyPair.png)


We are going to deploy a pre-built application via a CloudFormation template - this will be a fully-functioning recommendation system, allowing access to multiple Amazon Personalize features. But it has one drawback - there are no models built into it! So we will create them in this lab, and when they are ready we will re-configure this application to use them. But first we need to deploy this skeleton application: 

Follow this link to create the resources

[Create Cloudformation Stack](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=LabStack&region=us-east-1&templateURL=https://mrtdomshare.s3.amazonaws.com/cloudformation_template.yml)

The next screen asks for more configuration parameters, but only two of these are required: Stack name and KeyName. For Stack name enter something simple, such as LabStack, and select your previously-defined EC2 kay-pair, the click Next (not shown).

![cfnOtherParams](/images/cfnOtherParams.png)

There then follows two more screens. The first is called _Options_, but we have none to enter so just click on **Next**. The second is the final Review screen - **please verify** that the KeyName is the one that you just downloaded, and then click on **Create**. 

This will then go and create the environment, which will take around 10-15 minutes minutes. Unfortunately, we are creating IAM resources, so we cannot continue until it has completed - so read ahead and get a feel for what's coming up next.

