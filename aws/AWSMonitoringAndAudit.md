# AWS Monitoring, Troubleshooting & Audit
* Cloudwatch:
  * metrics - collect and track
    * Provides metrics for every service in AWS
    * metric means a variable thats monitored, has timestamps
    * belongs to namespaces
    * Dimensions (instance id, environment) up to 30
    * can create dashboard of metrics
    * can push custom metrics using PutMetricData api
      * has StorageResolution param, to control the granularity of stored data with respect to time
      * works for data points 2 weeks in past, 2 hours in future
  * logs - monitor, analyze, store log files
    * log group - an app, log stream - an app instance
    * expiration policies
    * can send logs to, s3 (export, not real time at all), (kds, kdf, lambda, opensearch) using subscriptions(cross account also possible for kds, kdf), has subscription filter
    * logs encrypted by default, can setup kms based encryption with own key
    * source for logs: sdk, cloudwatch agents, elastic beanstalk, ecs, lambda, vpc flow logs, cloudTrail, and many more
      * by default no logs from ec2 go to cloudwatch, need setup, need agent on ec2 to push log files, agent also works with on prem infra
      * cloud watch logs agent: can only send to cloudwatch logs
      * cloud watch unified agent: can send to cloudwatch logs, also collects additional system level metrics, central config using SSM Parameter Store
    * CloudWatch Logs Insights - similar to splunk
    * metric filter: how do i setup a alarm or maybe some push to sns? metric filter
      * cw logs -> metric filters -> cw alarm -> sns
    * alarms: trigger notifications for any metric
      * using which, can start/stop/restart ec2 instances, scale up/down ASGs, send notification to SNS(with which we can do pretty much anything)
      * composite alarms (and or conditions)
  * events (amazon eventbridge, former name cloudwatch events)- send notifications when events happen
    * react to cron job/ iam event
  * alarms - react to events/metrics
* X - Ray: troubleshoot app performance and errors, distributed tracing
* CloudTrail: monitoring of API calls, Audit changes to AWS Resources by your users

 