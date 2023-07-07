Summary

AWS RDS is a managed database service that AWS offers for hosting databases for MSSQL, MySQL, and Postgres. Runbooks provide detailed procedures for configuring backups, triaging outages, and performing restoration activities to mitigate data loss and return to service as soon as possible. This runbook ensures AWS RDS configuration, maintenance, and Disaster Recovery procedures are performed consistently and efficiently in the event of an outage or disaster. 



---

RDS Automated Backups

Amazon RDS performs and stores database backups and transaction logs for user-specified retention periods, allowing restoration to any second during a retention period up to the last 5 minutes. The automatic backup retention period can be configured for up to 35 days and are not enabled by default. As such, the automated backups must be configured to a user’s specification. By default, Amazon RDS creates and saves automated backups securely in Amazon S3 (S3) for a user-specified retention period. New instances from database snapshots are available to create at any point. Snapshots serve as functional and operational full backups, and the account is only billed for incremental storage use.

For longer storage periods, S3 Replication is the easiest and recommended process to trigger database snapshots to store in S3. These database snapshots are user-initiated backups stored in S3 and kept until explicitly deleted, and new instances can be created at any time.

User-Specification

batCAVE Technical Advisors (TAs) work closely with development teams to identify their official data retention policy, and help configure their RDS automated backup retention period, or S3 replication if they require longer than 35 days. Depending on the user’s need, batCAVE can set up RDS to cover multiple RDS availability zones (multi-AZ), though the default is single-zone.

Mutli-AZ Deployments

Multi-AZ deployments provide enhanced availability and durability for database instances, making them a natural fit for production database workloads. When provisioning a Multi-AZ database instance, Amazon RDS synchronously replicates the accompanying data to a standby instance in a different Availability Zone (AZ).

S3 Replication

AWS S3 has the option to automatically turn on replication from one S3 to another, even across regions or availability zones. This feature allows replications from the S3 location where current RDS Automated Backups are stored, to another S3 location for longer retention.

S3 Replication Process

1. Initially standup and configure a new RDS Database.
  1. Create the RDS service using the batCAVE Terraform Template.
  2. Configure the default backup retention to 35 days.
  3. Configure the initial database snapshot S3 location.
  4. Configure the number of Availability Zones the customer has agreed on (customer assumes the cost).
  5. Configure S3 Replication for longer term database backup storage (more than 35 days).
  6. Create a new S3 bucket specifically for longer-term RDS backup storage.
    * On the initial database snapshot S3 location, select to use S3 Replication, and specify the new S3 bucket as the target. This can be set to the default object retention to either one year, or forever, or anything in-between, depending on the user’s data backup needs. Usually, a years worth of backups is sufficient for most compliance requirements. 
      * Make sure a full replication is conducted, including meta data, so that each copy is stand alone and separate from the original.
2. Setup S3 Replication
  1. Setup the second S3 bucket to replicate to.
  2. In the settings for the first S3 bucket, turn on replication, and specify the second S3 bucket as the location to replicate to.
3. Set up secrets / credentials
  1. Secrets and credentials are accomplished with Terraform RDS modules.
  2. Secret rotation for the database can also be done with Terraform.
4. Setup database access for users and applications
Cluster/Pod Access

    Service Account and IAM Policy

    Network Policy

User Query Access



Upgrade an existing RDS Database


Create manual database snapshots

  Go into the RDS settings, and select to create a Manual Snapshot

Database is Down

  Check that AWS shows the database as running

  Look at the error logs in AWS RDS to identify the reason the database went down

  Try to restart the database


Database is unreachable


Database is corrupted

  If we confirm that the database is corrupted, and when it became corrupted, we can pull the last database snapshot that happened prior to the corruption, and restore the database to that snapshot. Snapshots are taken every 5 minutes, so within 5 minutes before an event that corrupted the database, we can recover to.

  You can find additional information at https://coda.io/d/_dqXGSkczFtl/_su20I#_luqxM 


Restore Database

  Identify when the database was corrupted, or at least, what the most recent time period was that the database was fine and not corrupted

  Based on that time frame, go into RDS settings, and look at the list of backups from the last 35 days, and choose the most recent 5 minute interval snapshot that matches the timeframe when the database was healthy and not corrupted

  In the RDS settings, select to restore that particular snapshot

  Once it completes the restore, confirm the database is up and usable

  For all of the above, work with the ADO as needed
