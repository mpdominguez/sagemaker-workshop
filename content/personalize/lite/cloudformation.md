---
title: "Cloud Formation Wizard"
chapter: false
weight: 3
---

Follow along with the screenshots if you have any questions about these steps.

## Cloud Formation Wizard
Start by clicking Next at the bottom like shown:

![Image1](/images/img1.png)

In the next page you need to provide a unique S3 bucket name for your file storage, it is recommended to simply add your first name and last name to the end of the default option as shown below, after that update click Next again.

![Image2](/images/img2.png)

This page is a bit longer so scroll to the bottom to click Next.

![Image3](/images/img3.png)

Again scroll to the bottom, check the box to enable the template to create new IAM resources and then click Create Stack.

![Image4](/images/img4.png)

For a few minutes CloudFormation will be creating the resources described above on your behalf it will look like this while it is provisioning:

![Image5](/images/img5.png)

Once it has completed you'll see green text like below indicating that the work has been completed:

![Image6](/images/img6.png)

Now that you have your environment created, you need to save the name of your S3 bucket for future use, you can find it by clicking on the Outputs tab and then looking for the resource S3Bucket, once you find it copy and paste it to a text file for the time being.

![Image6](/images/img7.png)
