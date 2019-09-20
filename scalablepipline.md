# Scalable Pipeline

* User POST (image)
* Setup API (API Gateway, API Apps, FlaskRestPlus on Kubernetes) (message)
    + Publish to Streaming Service (Kinesis, HD Insight Kafka, Kafka)
* Scalable Inference Service (AWS Lambda, Azure Function, Kubernetes)
	+ Polling form streaming service 
	+ Publish to result Streaming Service 
* Notification system (PagerDuty, Jira, Twolio, Email ...)
