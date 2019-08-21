---
title: "Cleaning up"
chapter: false
weight: 8
---

### Terminating the Notebook Instance

Open the Amazon SageMaker console and click on Notebook instances
Find the notebook instance listed as [Name]-lab-notebook, select its radio button and then click the Actions dropdown.

![terminateNotebook](/images/terminateNotebook.png)

Click Stop to stop the Notebook Instance. This does not delete the underlying data and resources. After a few minutes the instance status will change to Stopped, and you can now click on the Actions dropdown again, but this time select Delete.
Note that by selecting the name of the Notebook instance on this dialog you are taken to a more detailed information page regarding that instance, which also has Stop and Delete buttons present – notebooks can also be deleted using this method.


