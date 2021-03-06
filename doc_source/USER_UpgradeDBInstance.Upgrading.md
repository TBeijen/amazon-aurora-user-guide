# Upgrading a DB Cluster Engine Version<a name="USER_UpgradeDBInstance.Upgrading"></a>

Amazon RDS keeps your DB cluster up\-to\-date by providing newer versions of each supported database engine\. Newer versions can include bug fixes, security enhancements, and other improvements for the database engine\. When Amazon RDS supports a new version of a database engine, you can choose how and when to upgrade your database DB clusters\.

There are two kinds of upgrades: *major version upgrades* and *minor version upgrades*\. In general, a major engine version upgrade can introduce changes that are not compatible with existing applications\. In contrast, a minor version upgrade includes only changes that are backward\-compatible with existing applications\.

The version numbering sequence is specific for each database engine\. For example, Aurora PostgreSQL 9\.6 and 10\.4 are major engine versions and upgrading from any 9\.6 version to any 10\.4 version is a major version upgrade\. Aurora PostgreSQL version 9\.6\.6 and 9\.6\.9 are minor versions and upgrading from 9\.6\.6 to 9\.6\.9 is a minor version upgrade\. To determine the version of an Aurora DB cluster, follow the instructions in [Amazon Aurora Updates](Aurora.Updates.md)\.

For major version upgrades, you can restore a snapshot of the DB cluster and specify a higher major engine version\. For minor version upgrades of an Aurora PostgreSQL DB cluster, you can manually modify the engine version, or you can choose to enable auto minor version upgrades\. 

**Topics**
+ [Manually Upgrading the Engine Version](#USER_UpgradeDBInstance.Upgrading.Manual)
+ [Automatically Upgrading the Minor Engine Version](#USER_UpgradeDBInstance.Upgrading.AutoMinorVersionUpgrades)

## Manually Upgrading the Engine Version<a name="USER_UpgradeDBInstance.Upgrading.Manual"></a>

To perform a major version upgrade of a DB cluster, you can restore a snapshot of the DB cluster and specify a higher major engine version\. For information about restoring a DB cluster, see [Restoring from a DB Cluster Snapshot](USER_RestoreFromSnapshot.md)\.

To perform a minor version upgrade of an Aurora PostgreSQL DB cluster, use the following instructions for the AWS Management Console, the AWS CLI, or the RDS API\.

### Upgrading the Engine Version of a DB Cluster Using the Console<a name="USER_UpgradeDBInstance.Upgrading.Manual.Console"></a>

**To upgrade the engine version of a DB cluster by using the console**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Databases**, and then choose the DB cluster that you want to upgrade\. 

1. Choose **Modify**\. The **Modify DB cluster** page appears\.

1. For **DB engine version**, choose the new version\.

1. Choose **Continue** and check the summary of modifications\. 

1. To apply the changes immediately, choose **Apply immediately**\. Choosing this option can cause an outage in some cases\. For more information, see [Modifying an Amazon Aurora DB Cluster](Aurora.Modifying.md)\. 

1. On the confirmation page, review your changes\. If they are correct, choose **Modify DB Cluster** to save your changes\. 

   Alternatively, choose **Back** to edit your changes, or choose **Cancel** to cancel your changes\. 

### Upgrading the Engine Version of a DB Cluster Using the AWS CLI<a name="USER_UpgradeDBInstance.Upgrading.Manual.CLI"></a>

To upgrade the engine version of a DB cluster, use the CLI [modify\-db\-cluster](https://docs.aws.amazon.com/cli/latest/reference/rds/modify-db-cluster.html) command\. Specify the following parameters: 
+ `--db-cluster-identifier` – the name of the DB cluster\. 
+ `--engine-version` – the version number of the database engine to upgrade to\. For information about valid engine versions, use the AWS CLI [ describe\-db\-engine\-versions](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-engine-versions.html) command\.
+ `--no-apply-immediately` – apply changes during the next maintenance window\. To apply changes immediately, use `--apply-immediately`\. 

**Example**  
For Linux, OS X, or Unix:  

```
1. aws rds modify-db-cluster \
2.     --db-cluster-identifier <mydbcluster> \
3.     --engine-version <new_version> \
4.     --no-apply-immediately
```
For Windows:  

```
1. aws rds modify-db-cluster ^
2.     --db-cluster-identifier <mydbcluster> ^
3.     --engine-version <new_version> ^
4.     --no-apply-immediately
```

### Upgrading the Engine Version of a DB Cluster Using the RDS API<a name="USER_UpgradeDBInstance.Upgrading.Manual.API"></a>

To upgrade the engine version of a DB cluster, use the [ ModifyDBCluster](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference//API_ModifyDBCluster.html) action\. Specify the following parameters: 
+ `DBClusterIdentifier` – the name of the DB cluster, for example *`mydbcluster`*\. 
+ `EngineVersion` – the version number of the database engine to upgrade to\. For information about valid engine versions, use the [ DescribeDBEngineVersions](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference//API_DescribeDBEngineVersions.html) operation\.
+ `ApplyImmediately` – whether to apply changes immediately or during the next maintenance window\. To apply changes immediately, set the value to `true`\. To apply changes during the next maintenance window, set the value to `false`\. 

## Automatically Upgrading the Minor Engine Version<a name="USER_UpgradeDBInstance.Upgrading.AutoMinorVersionUpgrades"></a>

A *minor engine version* is an update to a DB engine version within a major engine version\. For example, a major engine version might be 5\.7 with the minor engine versions 5\.7\.4 and 5\.7\.5 within it\. 

If you want Amazon RDS to upgrade the DB engine version of a database automatically, you can enable auto minor version upgrades for the database\. When a minor engine version is designated as the preferred minor engine version, each database that meets both of the following conditions is upgraded to the minor engine version automatically:
+ The database is running a minor version of the DB engine that is lower than the preferred minor engine version\.
+ The database has auto minor version upgrade enabled\.

You can control whether auto minor version upgrade is enabled for a DB instance when you perform the following tasks:
+ [Creating an Amazon Aurora DB cluster](Aurora.CreateInstance.md)
+ [Modifying an Amazon Aurora DB cluster](Aurora.Modifying.md)
**Note**  
Modify the primary DB instance to control whether auto minor version upgrade is enabled for the entire DB cluster\.
+ [Restoring a DB cluster from a snapshot](USER_RestoreFromSnapshot.md)
**Note**  
When you restore a DB cluster from a snapshot, you can control whether auto minor version upgrade is enabled for the DB cluster only when you use the console\.
+ [Restoring a DB cluster to a specific time](USER_PIT.md)
**Note**  
When you restore a DB cluster to a specific time, you can control whether auto minor version upgrade is enabled for the DB cluster only when you use the console\.

When you perform these tasks, you can control whether auto minor version upgrade is enabled for the DB cluster in the following ways:
+ Using the console, set the **Auto minor version upgrade** option\.
+ Using the AWS CLI, set the `--auto-minor-version-upgrade|--no-auto-minor-version-upgrade` option\.
+ Using the RDS API, set the `AutoMinorVersionUpgrade` parameter\.

You can get an Amazon RDS event notification when a new minor engine version upgrade is available for one of your databases\. To get notifications, subscribe to Amazon RDS event notification through the Amazon Simple Notification Service \(Amazon SNS\)\. For more information, see [Using Amazon RDS Event Notification](USER_Events.md)\.

To determine whether a maintenance update, such as a DB engine version upgrade, is available for your DB cluster, you can use the console, AWS CLI, or RDS API\. You can also upgrade the DB engine version manually and adjust the maintenance window\. For more information, see [Maintaining an Amazon Aurora DB Cluster](USER_UpgradeDBInstance.Maintenance.md)\.