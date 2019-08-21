---
title: "Setup your Jupyter Notebook"
chapter: false
weight: 3
---
## Launching a Jupyter Notebook using Amazon SageMaker 

Click on Amazon SageMaker from the list of all services by entering Sagemaker into the Find services box. This will bring you to the Amazon SageMaker console homepage. In another browser tab or window, navigate to the IAM console homepage, as we'll need that shortly.

![consoleSMSelect](/images/consoleSMSelect.png)

To create a new Jupyter notebook instance, go to Notebook instances in the Amazon SageMaker console, and click the Create notebook instance button at the top of the browser window.

![Picture02](/images/Picture02.png)

Type [Name]-lab-notebook into the Notebook instance name text box, and then ml.t2.medium into the Notebook instance type field. Note, for this lab the majority of the work is performed by the Amazon Personalize service and not by your notebook instance, so there is no need to launch a large, compute-optimized C5 or GPU-based instance type - please just use the instance type specified here, as that's all that you need, and using these more powerful, more expensive instance families will not actually help you to complete the workshop any faster.



In the IAM role field in Permissions and encryption section choose Enter a custom IAM role ARN. Now go over into your open IAM tab or window, click in the Search IAM field, and then enter AmazonLab as the search term. Select the full role name for SageMaker, being careful not to select the Delete or Edit options.

![findSagemakerRole](/images/findSagemakerRole.png)

On the resulting detail screen copy the Role ARN value - this role will give your SageMaker notebook sufficient IAM priviledges, and full access to both the Personalize and S3 services. Note, for a Production system you would most likely restrict this role to specific S3 buckets as per your internal requirements, but this workshop allows access to any S3 bucket. You can now close this tab or window

Go back to your SageMaker screen, and in the Custom IAM role ARN field paste in the IAM ARN that you copied in the previous step, scroll down and click on Create Notebook Instance

Wait until the notebook instance status is InService. This will take a few minutes once the creation process has started. Then click on Open Jupyter - whilst you're waiting you can perform step #1 of the next section to copy some files from Git


![openNotebook](/images/openNotebook.png)

### Downloading required additional files
We need to download two files before starting work, which are all stored within the Lab's Git repository:

the Notebook file that contains the lab - personalize_sample_notebook.ipynb
a file that is part of the MovieLens dataset - u.item - that has been edited slightly to remove some control characters that cause on of the pandas library calls to fail, and also to include working URLs for the various movie posters that our application will show later
Go to the Git repository address, https://github.com/drandrewkane/AI_ML_Workshops, navigate to the Lab 6 and download the files called u.item and personalize_sample_notebook.ipynb respectively. Use any method that you are comfortable with, but do not clone the whole repository as it is quite large - for instance, try opening the files within Git in RAW format and saving them locally (be careful to maintain the correct file extentions)
In the notebook, assuming the status is now InService, and click on the Upload button, and in the dialog select the two files from the location that you stored them and upload them.


![uploadFiles](/images/uploadFiles.png)

Click on each of the two Upload buttons to actually upload the files, waiting for the first to complete before starting the second.

![uploadFiles2](/images/uploadFiles2.png)

Once both are upload you can click on the notebook .ipynb file and the lab notebook will open, and you can now begin to work through the lab notebook.

### Working Through a Jupyter Notebook

A notebook consisted of a number of cells; in SageMaker these will typically either be Code or Markdown cells. Markdown is used to allow for documentation to be defined inline with the code, giving the author a rich set of markdown formatting options. The first cell in this notebook, which is called Get the Personalize boto3 Client, is Markdown, and if you select any cell then the whole cell is highlighted.

![cellTypes](/images/cellTypes.png)

The first Markdown cell describes what the following Code cell is going to do – for the sake of this lab you do not have to understand the code that is being run in the Code cell, rather you should just appreciate what the notebook is doing and how you interact with a Jupyter notebook.

![loadBoto3Pre](/images/loadBoto3Pre.png)

To the left of a Code module is a set of empty braces [ ]. By highlighting the cell and then selecting the Run command in the menu bar, the Jupyter notebook will execute this code, outputting and code outputs to the notebook screen and keeping any results data internally for re-use in future steps. Do this now to execute the first code cell.

_Note: if a Markdown cell is highlighted, then clicking Run will move the highlight to the next cell_


Whilst the code is executing the braces will change to be ```[*]```, indicating that it is executing, and once complete will change to [1]. Future cells will have increasing numbers inside the braces, and this helps you see the order in which cells have been exected within the notebook. Directly below the code, but still within the Code cell, is the output from the code execution - this will include any error messages that your code has thrown. In this example, the code execurion successfully created the specified bucket in S3.

![loadBoto3Post](/images/loadBoto3Post.png)

Now please continue to work through the notebook lab - read the comments prior to each Code cell in order to get an understanding as to what is going on, as these explain why we are doing each step and how it ties in to using the Amazon Personalize service.




