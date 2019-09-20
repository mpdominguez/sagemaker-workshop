---
title: "Airflow Concepts"
chapter: false
weight: 13
---

Before implementing the solution, let’s get familiar with Airflow concepts. If you are already familiar with Airflow concepts, skip to the Airflow Amazon SageMaker operators section.

Apache Airflow is an open-source tool for orchestrating workflows and data processing pipelines. Airflow allows you to configure, schedule, and monitor data pipelines programmatically in Python to define all the stages of the lifecycle of a typical workflow management.

##### Airflow nomenclature
- DAG (Directed Acyclic Graph): DAGs describe how to run a workflow by defining the pipeline in Python, that is configuration as code. Pipelines are designed as a directed acyclic graph by dividing a pipeline into tasks that can be executed independently. Then these tasks are combined logically as a graph.
- Operators: Operators are atomic components in a DAG describing a single task in the pipeline. They determine what gets done in that task when a DAG runs. Airflow provides operators for common tasks. It is extensible, so you can define custom operators. Airflow Amazon SageMaker operators are one of these custom operators contributed by AWS to integrate Airflow with Amazon SageMaker.
- Task: After an operator is instantiated, it’s referred to as a “task.”
- Task instance: A task instance represents a specific run of a task characterized by a DAG, a task, and a point in time.
- Scheduling: The DAGs and tasks can be run on demand or can be scheduled to be run at a certain frequency defined as a cron expression in the DAG.

##### Airflow architecture
The following diagram shows the typical components of Airflow architecture.

![Personalize](/images/sagemaker-airflow-2.gif)

- Scheduler: The scheduler is a persistent service that monitors DAGs and tasks, and triggers the task instances whose dependencies have been met. The scheduler is responsible for invoking the executor defined in the Airflow configuration.
- Executor: Executors are the mechanism by which task instances get to run. Airflow by default provides different types of executors and you can define custom executors, such as a Kubernetes executor.
- Broker: The broker queues the messages (task requests to be executed) and acts as a communicator between the executor and the workers.
- Workers: The actual nodes where tasks are executed and that return the result of the task.
- Web server: A web server to render the Airflow UI.
- Configuration file: Configure settings such as executor to use, airflow metadata database connections, DAG, and repository location. You can also define concurrency and parallelism limits, etc.
- Metadata database: Database to store all the metadata related to the DAGS, DAG runs, tasks, variables, and connections.

##### Airflow Amazon SageMaker operators
Amazon SageMaker operators are custom operators available with Airflow installation allowing Airflow to talk to Amazon SageMaker and perform the following ML tasks:

- **SageMakerTrainingOperator:** Creates an Amazon SageMaker training job.
- **SageMakerTuningOperator:** Creates an AmazonSageMaker hyperparameter tuning job.
- **SageMakerTransformOperator:** Creates an Amazon SageMaker batch transform job.
- **SageMakerModelOperator:** Creates an Amazon SageMaker model.
- **SageMakerEndpointConfigOperator:** Creates an Amazon SageMaker endpoint config.
- **SageMakerEndpointOperator:** Creates an Amazon SageMaker endpoint to make inference calls.


