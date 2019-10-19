---
title: "Cloudformation Template"
chapter: false
weight: 19
---
In case that you want to deploy the stack without going step by step, here is the Cloudformation stack:

https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=LabStack&region=us-east-1&templateURL=https://mrtdomshare.s3.amazonaws.com/sagemakerworkshop-step/sagemaker-step-functions.yml

The only step that you have to make manually is uploading the data to S3 (the Billing and Reseller folder with the csv files)

Then you go to step functions, and execute it manually to see the whole process
