RDS Backup / Restore / DR Summary

Amazon RDS performs and stores database backups and transaction logs for user-specified retention periods, allowing restoration to any second during a retention period up to the last 5 minutes. The automatic backup retention period can be configured for up to 35 days and are not enabled by default. As such, the automated backups must be configured to a user’s specification.

By default, Amazon RDS creates and saves automated backups securely in Amazon S3 (S3) for a user-specified retention period. New instances from database snapshots are available to create at any point. Snapshots serve as functional and operational full backups, and the account is only billed for incremental storage use.

Amazon RDS provides both full snapshots and PITR (Point in Time Recovery) backups 



Setup / Configuration

In all DB configurations, the process for enabling automated snapshots is the same (in this case, with an retention period of 7 days)
CLI:

aws rds modify-db-instance \
    --db-instance-identifier mydbinstance  \
    --backup-retention-period 7 \
    --apply-immediately

Restoring from snapshots

In all DB configurations, the process for restoring from a previous snapshot can be done one of several ways. In the AWS Console:

* Sign in to the AWS Management Console and open the Amazon RDS console at https://console.aws.amazon.com/rds/
* In the navigation pane, choose Snapshots.
* Choose the DB snapshot that you want to restore from.
* For Actions, choose Restore snapshot.
* On the Restore snapshot page, for DB instance identifier, enter the name for your restored DB instance.
* Specify other settings, such as allocated storage size.
* Choose Restore DB instance.

AWS CLI:

aws rds restore-db-instance-from-db-snapshot \
    --db-instance-identifier mynewdbinstance \
    --db-snapshot-identifier mydbsnapshot \
    --allocated-storage 100

Terraform:

provider "aws" {
  region = "us-east-1"
}
# Get latest snapshot from production DB
data "aws_db_snapshot" "mydbsnapshot" {
    most_recent            = true
    db_instance_identifier = "mydbinstance"
}
# Create new staging DB
resource "aws_db_instance" "mydbclone" {
# ... other configuration ....
  snapshot_identifier  = "${data.aws_db_snapshot.db_snapshot.id}"
  skip_final_snapshot = true
}

Notification Pipeline

Engineering handoff

Remediation Steps

ADO handoff



Limitations and Caveats

* AWS RDS snapshots are limited to 35 days of retention. For archival purposes longer than that, you will need to work with the appropriate TA in order to setup necessary configs



FAQ



References

1. https://aws.amazon.com/blogs/database/implementing-a-disaster-recovery-strategy-with-amazon-rds/
2. https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html
3. https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Tutorials.RestoringFromSnapshot.html
4. https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance
