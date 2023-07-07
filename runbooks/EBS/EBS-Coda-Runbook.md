Purpose

  This runbook is dedicated to performing AWS EBS configuration, maintenance, and Disaster Recovery, activities. AWS EBS is a scalable, high-performance block-storage service that is attached to Amazon EC2 instances to provide storage volumes. EBS volumes are attached to EC2 nodes that make up the Kubernetes Cluster. For EBS volumes previously created, basic account and access information will be located in the ADO’s Home Base, and the steps for how to do activities you may need to do, can be found there.

Overall Configuration

  The batCAVE EKS cluster is made up of Utility Belt EC2 instances and ADO specific instances. Each EC2 instance has an attached EBS volume. At the Kubernetes level, the batCAVE Landing Zone provisions storage class(es) that orchestrates the creation of Persistent Volumes from these EBS volumes. At the application level, applications create Persistent Volume Claims that are associated with the Application pods and are also bound to the Persistent Volumes that the Storage Class provisions.

General EBS Backup and Recovery Notes

  Velero Automated Backups and Snapshots

  the batCAVE team uses Velero to automate EBS Snapshots and is specialized to take backups of Kubernetes resources. Velero is automatically configured to take a backup every 12 hours. The backups are stored in an S3 bucket called ${batcave_cluster_name}-batcave-velero-storage (e.g. ispg-prod-batcave-velero-storage). The time to live on these backups has been configured for 48 hours. Velero also creates EBS snapshots for each EBS instance. These snapshots are taken every 12 hours and are stored in EC2 → Snapshots.

  ADO Needs

  The Technical Advisors will need to work with the ADO to identify their official data retention policy, and help them configure their EBS automated backup retention period. If multi-region backup storage location is required, we can also set up S3 replication.

Initially standup and configure EBS Volumes, StorageClasses, and Persistent Volumes
  * EBS volumes are specified in the batcave-landing-zone/infra/<ado>/<env>/eks-cluster/terragrunt.hcl file
  * Storage Classes are configured in the apps/<ado>/<env>reclaimpolicy-patch.yaml . They are deployed during the deploy.sh script
  * Storage Classes automatically provision Persistent Volumes assuming the correct Storage Class is set in the Persistent Volume Claim yaml file.


Setup S3 Replication

  We recommend using S3 replication. AWS documentation on this here: https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html

Upgrade an existing EBS Volume

  batCAVE uses gp2 or gp3 EBS volumes. Modifications to these volumes (e.g. type or storage amount) can be modified following instructions here: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/requesting-ebs-volume-modifications.html

Create manual EBS snapshots and backups

  You can run manual Velero backup and snapshot commands. See the example section for how to execute this here: https://velero.io/docs/v1.1.0/locations/

EBS or Persistent Volume not working as expected

  See instructions on diagnosing the issue and troubleshooting in the general Disaster Recovery document here: https://coda.io/d/_dqXGSkczFtl/_su20I#_luiHV
