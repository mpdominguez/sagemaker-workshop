---
title: "Putting it all together"
chapter: false
weight: 16
---

Airflow DAG integrates all the tasks we’ve described as a ML workflow. Airflow DAG is a Python script where you express individual tasks with Airflow operators, set task dependencies, and associate the tasks to the DAG to run on demand or at a scheduled interval. The Airflow DAG script is divided into following sections.
1. Set DAG with parameters such as schedule interval, concurrency, etc.
```
dag = DAG(
    dag_id='sagemaker-ml-pipeline',
    default_args=args,
    schedule_interval=None,
    concurrency=1,
    max_active_runs=1,
    user_defined_filters={'tojson': lambda s: JSONEncoder().encode(s)}
)
```
2. Set up training, tuning, and inference configurations for each operator using Amazon SageMaker Python SDK for Airflow
3. Create individual tasks with Airflow operators that define trigger rules and associate them with the DAG object. Refer to the previous section for defining these individual tasks.
4. Specify task dependencies.
```
init.set_downstream(preprocess_task)
preprocess_task.set_downstream(prepare_task)
prepare_task.set_downstream(branching)
branching.set_downstream(tune_model_task)
branching.set_downstream(train_model_task)
tune_model_task.set_downstream(batch_transform_task)
train_model_task.set_downstream(batch_transform_task)
batch_transform_task.set_downstream(cleanup_task)
```

After the DAG is ready, deploy it to the Airflow DAG repository using CI/CD pipelines. If you followed the setup outlined in Airflow setup, the CloudFormation stack deployed to install Airflow components will add the Airflow DAG to the repository on the Airflow instance that has the ML workflow for building the recommender system.

![Personalize](/images/sagemaker-airflow-6.gif)

After triggering the DAG on demand or on a schedule, you can monitor the DAG in multiple ways: tree view, graph view, Gantt chart, task instance logs, etc.
