# Apache Airflow 
​
Ming-Jer Lee(mingjerlee@mingjerlee.com)
​
## How You Run Your Repeitive Data Science Tasks?
​
*   Manually executing Jupyter notebook cell one by one?
*   Typing bunch of bash command to setup environment variable then executing the task?
*   Manually start a bash script that includes setting up environment and task?
*   Running through a scheduler like cron?
*   Using a job orchestrator?
​
## What is Airflow
​
Apache Airflow is a tool for scheduling, orchestrating and monitoring workflows programmatically. 
​
![](https://airflow.apache.org/_images/dags.png)
​
## Airflow Concepts
​
* DAG (Directed Acyclic Graph): a graph that glue all the tasks with inter-dependencies
* Operator: a template for a specific type of task to be executed. Ex: `BashOperator`, `EmailOperator`, `PythonOperator`, `HttpOperator`, `MsSqlOperator`, `S3ToGoogleCloudStorageOperator`, `SageMakerModelOperator`, `DatabricksSubmitRunOperator` ...
* Sensor: special operator which will only execute if a certain condition is met. Ex: `ExternalTaskSensor`, `HttpSensor`, `S3KeySensor`, ...
* Task: Instance of operator/sensor defined a unit of work to be executed
​
![](https://www.applydatascience.com/images/2018-12-11-airflow-concept/Screen%20Shot%202018-12-15%20at%208.18.10%20PM.png)
​
## Airflow Components
​
* WebUI: portal to view status of the DAGs
* Metadata DB: metadata store for job status, tasks, ...
* Scheduler: parse DAG bag, create DAG objects, trigger executors to execute tasks
* Executor: A message queuing process that orchestrates worker process to execute tasks. Ex: LocalExecutor, CeleryExecutor, KubernetesExecutor ...
​
![](https://miro.medium.com/max/1140/1*u6duhZD2J_i1zZ0Txq26Cg.png)
​
## Why use Airflow
​
* Easy to Setup Jobs:
DAGs are written in python, which is more easier than English.
​
* Run Task Whenever You Need:
Schedule with ron syntax, manually trigger(no need to figure how you run it 117 days ago).
​
* Visualization:
See all related task in single place, no more mysterious cron job.
​
* Secure:
Role-based access control(RBAC) via LDAP or OAuth. 
​
* Horizontal Scale:
EKS/AKS for KubernetesExecutor or Azure Autoscale/AWS Auto Scaling for CeleryExecutor or DaskExecutor ....
​
* Community Adoption(as of 9/19/2019):
​
|        | Azkaban | Luigi | Airflow |
|--------|---------|-------|---------|
|Adaption Companies | NA | 53+ | 310+ |
|GitHub Stars | 2801 | 12207 | 13963 |
|GitHub Commits | 2374 | 3831 | 7045 |
​
* Extensible:
Easy to define your own operator, executors.
​
## My Airflow Experience
​
* As Data Engineer
    + on-perm+2 clouds, 10+ databases, 10+ data engineer, 10+ nodes, 500+ dataset(web scrapping, vendor, ), 1000+ DAGs, 10000+ tasks per day
​
* As Data Scientist(ML Engineer)
    + on-perm+1 cloud, 10+ models, 100+ backtesting, 5+ dashboards per day
​
* At Home
    + Single Linux server
    + Collecting all my credit transactions using Plaid API 
    + Downloading videos from home security system and finding out all video with human/wildlife and my cat.
    + Updating my customized news feed(youtube video, hackernews, stock market news)
​
* Happy wife(an accountant)
    + Automated payroll process reduce 8 hours job(lots of Excel files and  operations) to 10 minutes (review the number).
    + Automated email process to clients
​
## Monitoring 
​
### Logging
​
With RBAC, Airflow log tells you who do what on which at when. 
Just three lines in the configuration, you can easily stream Airflow log to ElasticSearch. 
Or use Splunk or equivalent log management system...
​
### Job Fail
​
Email alert, PagerDuty, Jira ticket ...
​
### Airflow Health
​
Airflow has its own issues, like zombie tasks,webserver down, not picking up tasks ... 
Some tricks to making sure it is healthy
* Monitoring System
  + Grafana/DataDog to see some airflow metrics(uptime, number of task in queue, running, schedule delay)
* Running Heartbeating DAG
  + Execute simple task like `SELECT 1;` regularly.
  + If fail or is not running, your monitoring system should looks different.
​
![](https://grafana.com/api/dashboards/9672/images/6045/image)
​
## My Thought
​
* Airflow is not only for Data Engineering. It can be helpful for everybody. This is more like [Automate the Boring Stuff with Python](https://automatetheboringstuff.com
) with steroid.
* Unless you are 100% on single cloud and OK with vendor locked in. Each team should have their own Airflow instance (or your platform team need to give us access to it with proper RBAC).
* No short cut to learn Airflow. 
    + To use `BashOperator` you need to know how to write bash script and python. You won't magically know how to write bash script by using Airflow.
    + Source control your dag files, Dockerfiles ...
​
