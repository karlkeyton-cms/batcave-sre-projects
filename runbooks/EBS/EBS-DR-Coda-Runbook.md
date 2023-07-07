EBS Backup / Restore / DR Summary

Velero Automated Backups and Snapshots

The batCAVE team uses Velero to automate EBS Snapshots and is specialized to take backups of Kubernetes resources. Velero is automatically configured to take a backup every 12 hours. The backups are stored in an S3 bucket called ${batcave_cluster_name}-batcave-velero-storage (e.g. ispg-prod-batcave-velero-storage). The time to live on these backups has been configured for 48 hours. Velero also creates EBS snapshots for each EBS instance. These snapshots are taken every 12 hours and are stored in EC2 → Snapshots.

Setup / Configuration

Velero:



Restoring from snapshots

The process for restoring an EBS snapshot is fundamentally no different than creating an EBS volume — the only additional step is specifying the snapshot id to be used in the creation of the volume. The simplest form of this from the AWS CLI looks like:

aws ec2 create-volume \
    --snapshot-id snap-<SNAPSHOT ID> \
    --availability-zone us-east-1a

Notification Pipeline

Engineering handoff

Remediation Steps

ADO handoff



Limitations and Caveats



FAQ



References

1. https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html
2. https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html#ebs-create-volume-from-snapshot
3. https://aws.amazon.com/blogs/containers/backup-and-restore-your-amazon-eks-cluster-resources-using-velero/
