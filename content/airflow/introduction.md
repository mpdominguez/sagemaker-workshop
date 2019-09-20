---
title: "Introduction"
chapter: false
weight: 11 
---

Introduction
ML workflows consist of tasks that are often cyclical and iterative to improve the accuracy of the model and achieve better results. We recently announced new integrations with Amazon SageMaker that allow you to build and manage these workflows:

- AWS Step Functions automates and orchestrates Amazon SageMaker related tasks in an end-to-end workflow. You can automate publishing datasets to Amazon S3, training an ML model on your data with Amazon SageMaker, and deploying your model for prediction. AWS Step Functions will monitor Amazon SageMaker and other jobs until they succeed or fail, and either transition to the next step of the workflow or retry the job. It includes built-in error handling, parameter passing, state management, and a visual console that lets you monitor your ML workflows as they run.
- Many customers currently use Apache Airflow, a popular open source framework for authoring, scheduling, and monitoring multi-stage workflows. With this integration, multiple Amazon SageMaker operators are available with Airflow, including model training, hyperparameter tuning, model deployment, and batch transform. This allows you to use the same orchestration tool to manage ML workflows with tasks running on Amazon SageMaker.

This exercise shows how you can build and manage ML workflows using Amazon Sagemaker and Apache Airflow. We’ll build a recommender system to predict a customer’s rating for a certain video based on the customer’s historical ratings of similar videos, as well as the behavior of other similar customers. We’ll use historical star ratings from over 2 million Amazon customers on over 160,000 digital videos. Details on this dataset can be found at its AWS Open Data page.
