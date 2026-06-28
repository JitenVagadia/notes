# RDS - Relational Database Service
* Allows to create databases in cloud, **managed** by AWS
  * Postgres, MySQL, Oracle, Aurora (AWS Proprietary)
* Pros:
  * Automated Provisioning, patching
  * configurable/auto backups, allow restoring
  * Monitoring dashboards
  * Multi AZ setup
  * scaling
  * AYSNC read replicas max 15, within/cross AZ, cross region (same region -> no network fee)
  * Multi AZ (for DR), sync replication
    * one DNS, auto failover
  * single AZ to multi AZ setup
    * internally takes snapshot, restores in another AZ, syncs
## Aurora
* Amazon Proprietary
* Postgres, MySQL drivers work with Aurora
* cloud optimized, better performance
* auto storage scaling
* insta failover, HA native
  * 6 copies across 3 AZ
    * writer endpoint pointing to master, reader end point connected to all
    * master takes the write, sync success on 4 for write
    * read from 3
  * supports cross region replication
* costs a bit more
* At rest encryption (need to configure)
  * AWS KMS
  * should define at launch time
  * master not encrypted -> cant encrypt replicas
  * to encrypt the un encrypted, take snapshot then restore as encrypted
* in flight encryption by default, use AWS root tls certs to connect
* IAM roles, SGs can apply
* can enable Audit logs and send to cloud watch
* RDS Proxy:
  * fully managed proxy
  * pool and share db connections
  * reduce stress on db connections
  * serverless, autoscaling, HA, multi AZ
  * enforce IAM auth on DB, store secret in AWS secrets manager
  * not public
  * can only access from vpc