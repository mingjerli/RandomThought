
# Datalake 

## Tier of Data

### Raw Data

Everything the team is collecting (including anything from other team's production database)

Store in S3 like bucket storage

### Sanitized Data

Use for model training
Store in S3, Databases(SQL, NoSQL, Graph ...)

### Enhanced Data

Ready to share.
Serving BI tools dashboard, flask app, Tableau, Apache Superset

Store in S3, Databases, indexing with ElasticSearch

Use Airflow to orchestrate all ETL jobs
    + Lot's of operator, native Kubernetes support

## Workflow Management

* Apache Airflow for batch type 
* Apache Kafka for streaming type

Tasks are run through AWS Lambda, Azure Function or Kubernetes services.

## Query Pattern

* AdHoc/EDA
    + Serverless, minimal engineering setup to query the data
    + High latency
    + Expensive for well defined query pattern
    + AWS Athena/Data Lake Analytics

* Recurrent bursting(ETL/Model training/Rollup Aggregations/single project) queries
    + Cheaper than serverless solution for known query pattern
    + High latency
    + Engineering setup to query(need to load into specific format or spin up query engine)
    + Snowflake/Presto/Spark/Drill/EMR/HDInsight ...

* Recurrent consisting(BI tools/multiple projects)
    + Database(depend on use cases and query pattern) 
    + Low latency
    + SQL, Mongo, Neo4j, ...

## Non-tabular data(images, videos, sound)

Spark if not fit within on machine, else python.

## ML Workflow

SageMaker or Azure ML Studio, or JupyterHub (python, spark, julia, R kernels) on Kubernetes for different instance type.

## Data Version

* Airflow dags and dockerfiles (for Kubernetes Executor) need to be source control.
* At raw data level, 
	+ write-only ingestion with timestamp
	+ maintaining a latest view of the datalake (don't use s3 versioning your Athena cannot query older version)
	+ sanitized and enhanced data can be reproduce at a desire timesamp with timestamp filter query
* At sanitized and enhanced level,
    + Use S3 version and Database snapshot.

## Monitoring

### Operationa

* Airflow retries or fails trigger Email, PagerDuty, Jira or Twilio.
* Output Airflow job/task statistics to Grafana like dashboard for management level.

### Quality

* `great_expectations` for unittest like before promoting data to production(only at enhanced level).
* Timeseries forecasting on table metadata/aggregation values and report anomaly. 
    + My work before, scalable timeseries forcasting and using z-score and autocorrelation lag to detect anomaly, also collecting user feedback as supervised learning problem to avoid false alarm.

## Discovery

* Elasticsearch index
* Metadata(tagging, labeling) collection.
