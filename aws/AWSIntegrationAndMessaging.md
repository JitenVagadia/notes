# AWS Integration & Messaging
* SNS -> pub/sub, SQS -> queue, Kinesis -> real time streaming
* SQS:
  * fully managed
  * 4 day default retention, 14 max
  * 1M max message size
  * sub 10ms latency
  * may deliver dupl messages, or out of order messages
  * use SDK sendMessage API to produce
  * for consumers, poll max 10 at a time, explicitly need to delete using DeleteMessage API
  * can setup a cloudwatch alarm on queue length, to scale for any ASG
  * Security:
    * inflight: https, at rest: KMS keys, client encryption
    * Access control: IAM policies
    * IAM policy = grant an identity permission to use a queue.
    * SQS access policy = grant a queue permission to be used by someone.
  * message visibility timeout: message invisible to other consumers for 30 sec, once fetched by any 1
    * consumer can call ChangeMessageVisibility to get more time
  * can configure DLQ: after MaximumReceives, goes to DLQ
    * Redrive to Source: manual intervention to put that message back to source or somewhere else
  * can configure DelaySeconds: messages once pushed are only available after x seconds for consumption
  * can enable long polling at queue level and also at api level (ReceiveMessageWaitTimeSeconds)
  * Message much bigger than 1M? can use SQS Extended Client (Java Library)
    * uses configured s3 bucket to do it, client side
  * offers 2 types: FIFO / Standard
  * FIFO:
    * Ordering by Message Group ID, mandatory param
      * ordering inside group guaranteed, 
    * dedup by dedup id or hash
* SNS:
  * pub/sub
  * direct publish -> sends message directly to some endpoint (email,sms,push)
  * topic publish -> deliver to all subscribers simultaneously
  * Security:
    * Encryption: inflight (https), at rest (KMS), client side
    * IAM Policy, SNS Access policy
  * can pipe: (some app) -> SNS ->> (multiple SQSs) ->> (multiple diff apps)
    * SNS->(multiple SQSs), typical fan out
  * cross region delivery (for sns->sqs, sns doesn't fully support cross region for everything)
  * SNS to Amazon S3 through Kinesis Data Firehose, possible
  * Has FIFO type
    * similar to sqs
    * can have sqs standard and fifo as subscribers
  * subscriptions can have filter policy
* Kinesis Data Streams:
  * collect and store streaming data
  * 365 day retention, can't DELETE data
  * can replay data
  * ordering guarantee for same Partition ID
  * capacity modes:
    * provisioned -> choose number of shards(each has 1mb/s (or 1000rps) in 2mb/s out)
    * on demand -> no provision choosing, default 4 MB/s in, auto scale
* Amazon Data Firehose (once called Kinesis Data Firehose):
  * loads streaming data into a destination
  * fully managed, auto scale, serverless, payperuse
  * near real time with buffering feature
  * supports wide varieties of data encoding
  * aws lambda support for custom data transformations
  * no data storage, no replay
* common patterns:
  * KDS for processing, fire hose at the end for storage
  * firehose only when no processing needed like WAF logs, cloudwatch logs, etc
  * KDS only: want to do some parallel work real time on events
* SNS is a notification system (no replay, no persistence, subscriber down-> message lost) — event notification, fire and forget, push to subscribers 
* KDS is a data pipeline (consumer down -> records accumulate) — event streaming, data persists, consumers pull at their own pace