**Amazon Aurora MySQL-Compatible Edition version 2 (with MySQL 5.7 compatibility) is planned to reach the end of standard support on October 31, 2024.**


If you have a MySQL 5.7–compatible cluster and want to upgrade it to a MySQL–8.0 compatible cluster, you can do so by running an upgrade process on the cluster itself. This kind of upgrade is an in-place upgrade, in contrast to upgrades that you do by creating a new cluster. This technique keeps the same endpoint and other characteristics of the cluster. The upgrade is relatively fast because it doesn't require copying all your data to a new cluster volume. This stability helps to minimize any configuration changes in your applications. It also helps to reduce the amount of testing for the upgraded cluster. This is because the number of DB instances and their instance classes all stay the same.

The in-place upgrade mechanism involves shutting down your DB cluster while the operation takes place. Aurora performs a clean shutdown and completes outstanding operations such as transaction rollback and undo purge. For more information, see How the Aurora MySQL in-place major version upgrade works.

The in-place upgrade method is convenient, because it is simple to perform and minimizes configuration changes to associated applications. For example, an in-place upgrade preserves the endpoints and set of DB instances for your cluster. However, the time needed for an in-place upgrade can vary depending on the properties of your schema and how busy the cluster is. Thus, depending on the needs for your cluster, you can choose among the upgrade techniques:

In-place upgrade
Blue/Green Deployment
Snapshot restore


#####################################################################################
We are goigng with Snapshot restore:

## Existing cluster of Aurora V2:  Engine version 5.7.mysql_aurora.2.11.5
## New cluster to be created Aurora V3: Engine version 8.0.mysql_aurora.3.07.0
#####################################################################################

**Process:**
- choose non-businees hours maintenance window and Pause the cluster to avoid any writes/transactions
- Create a manual snapshot of the existing aurorav2 cluster with a unique name across the environments. Lets say BETA & PROD, You should create the snapshot in both accounts with same name for easy code managment while passing snapshot Identifier.
- Create a secrete and mention DatabaseName &  Cluster name and cusome values which you like. Here we are fetching these values though code from SM.
- Create a CFN/CDK code base and map correct secret & snapshot values
- Push the changes, it will create new cluster with v3 engine.
- Test the accessibility, as this is the copy of existing cluster - try to use existing login creds and make sure its accessible.
- After everything is validated and fine the proceed with Post deployment steps
  
**Post Deployment Steps:**
    Let V2 cluster name : myaurora
    Let V3 cluster name : myaurorav3
- Rename the existing v2 cluster to something else. Let's say "myaurora" to "myrdsaurora"
- Rename the newly created V3 cluster to old v2 cluster name. let's say "myaurorav3" to "myaurora".
- Now the existing v2 cluster and new v3 cluster end points (writer & reader) will get changed based on the cluster names modified.
Finally:
- YOU HAVE UPGRADED FROM AURORA V2 TO AURORA V3 ENGINE FOR YOUR CLUSTER
- 
##########################################################################################

Useful Docs to go through:

Upgrading the major version of an Amazon Aurora MySQL DB cluster:- 
https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Updates.MajorVersionUpgrade.html
https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-restore-snapshot.html

Aurora MySQL configuration parameters:-
https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Reference.ParameterGroups.html#AuroraMySQL.Reference.Parameters.Cluster

Syntax:- AWS::RDS::DBCluster
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbcluster.html#aws-resource-rds-dbcluster-syntax

SnapshotIdentifier:-
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbcluster.html#cfn-rds-dbcluster-snapshotidentifier

EngineVersion:-
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbcluster.html#cfn-rds-dbcluster-engineversion

Supported DB engines for DB instance classes:-
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.html





