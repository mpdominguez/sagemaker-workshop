---
title: "Clone the Service Repos"
chapter: false
weight: 23
---
Now, let's open a Terminal in our notebook by going to "New/Terminal" and perform the following commands

```
cd ~/SageMaker
git clone https://github.com/githubmg/buildon-workshop
mv buildon-workshop/pure-sagemaker-workshop/ ~/SageMaker/1-pure-sagemaker-workshop
rm -rf ~/SageMaker/sagemaker-step-functions/
```
If we reload the Notebook, we'll see a new folder called "1-pure-sagemaker-workshop"



