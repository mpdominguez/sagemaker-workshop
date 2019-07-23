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



