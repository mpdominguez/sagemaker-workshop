---
title: "Orchestration"
chapter: false
weight: 13 
---

Now it's time to create all the necesary steps to schedule the training, deployment and inference with the model. For this, we are going to use an architecture similar to the serverless sagemaker orchestration but adapted to our specific problem.

First, we need to create lambda functions capables of:

1. Training the model
2. Awaiting for training
3. Deploying the model
4. Awaiting for deploy
5. Predict, save predictions and delete the endpoint

So, let's do it
