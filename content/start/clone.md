---
title: "Clone the Service Repos"
chapter: false
weight: 23
---
Now, let's open a Terminal in our notebook by going to "New/Terminal" and perform the following commands

```
cd ~/SageMaker
git clone https://github.com/githubmg/sagemaker-step-functions 
mv sagemaker-step-functions/pure-sagemaker-workshop/ ~/SageMaker/1-pure-sagemaker-workshop
mv sagemaker-step-functions/implementation-with-step-func/ ~/SageMaker/2-implementation-with-step-functions
rm -rf ~/SageMaker/sagemaker-step-functions/
```
If we reload the Notebook, we'll see a new folder called "1-pure-sagemaker-workshop"

We will run all the notebooks there in order.
The First Notebook it can run without modifications.
In the Second Notebook it's important to:

* Create a bucket for this notebook (ie: "sales-prediction-mrtdom")
* Change the bucket name to some of your own in the step 3: bucket = 'sales-prediction-mrtdom' and from then it can run without modifications.
* Take special care in the last step, if you get an "Best training job not available for tuning job" it's mostly because the training jobs are still running. The process takes roughly 20 minutes so it's a good moment to get a cup of coffee or have a break.

The Third Notebook


