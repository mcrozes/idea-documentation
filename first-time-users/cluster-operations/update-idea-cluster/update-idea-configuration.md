# Update IDEA configuration

{% hint style="info" %}
Use the **Config** command if you want to make a configuration change (e.g: enable SES, point DCV driver to a new version ...). Refer to [update-idea-configuration.md](update-idea-configuration.md "mention") for other types of updates.
{% endhint %}

You can retrieve the current configuration of your IDEA cluster by running `./idea-admin.sh config show` utility.&#x20;

This utility also support regular expression as part of the `--query/-q`argument. For example, run the command below to list all configuration related to the integrtion of AWS Backup

```
./idea-admin.sh config show \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION> \
  --query "(.*)backup(.*)"
+-------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+---------+
| Key                                                                           | Value                                                                                  | Version |
+-------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+---------+
| cluster.backups.backup_plan.arn                                               | arn:aws:backup:us-east-2:REDACTED:backup-plan:c80992c6-8d7c-4708-9182-0bf81f20b3d2     | 2       |
| cluster.backups.backup_plan.rules.default.completion_window_minutes           | 480                                                                                    | 1       |
| cluster.backups.backup_plan.rules.default.delete_after_days                   | 7                                                                                      | 1       |
| cluster.backups.backup_plan.rules.default.move_to_cold_storage_after_days     | -                                                                                      | 1       |
| cluster.backups.backup_plan.rules.default.schedule_expression                 | cron(0 5 * * ? *)                                                                      | 1       |
| cluster.backups.backup_plan.rules.default.start_window_minutes                | 60                                                                                     | 1       |
| cluster.backups.backup_plan.selection.tags                                    | - Key=idea:ClusterName,Value=<CLUSTER_NAME>                                            | 1       |
|                                                                               | - Key=idea:BackupPlan,Value=cluster                                                    |         |
|                                                                               |                                                                                        |         |
| cluster.backups.backup_vault.arn                                              | arn:aws:backup:us-east-2:REDACTED:backup-vault:idea-patchu-cluster-backup-vault        | 2       |
| cluster.backups.backup_vault.kms_key_id                                       | -                                                                                      | 1       |
| cluster.backups.backup_vault.removal_policy                                   | DESTROY                                                                                | 1       |
| cluster.backups.enable_restore                                                | True                                                                                   | 1       |
| cluster.backups.enabled                                                       | True                                                                                   | 1       |
| cluster.backups.role_arn                                                      | arn:aws:iam::REDACTED:role/idea-patchu-cluster-backup-role-us-east-2                   | 2       |
| cluster.logging.handlers.file.backupCount                                     | 15                                                                                     | 1       |
| vdc.vdi_host_backup.backup_plan.arn                                           | arn:aws:backup:us-east-2:REDACTED:backup-plan:d65c14d0-a970-4d3c-933e-4b658e1a6f3d     | 1       |
| vdc.vdi_host_backup.backup_plan.rules.default.completion_window_minutes       | 480                                                                                    | 1       |
| vdc.vdi_host_backup.backup_plan.rules.default.delete_after_days               | 7                                                                                      | 1       |
| vdc.vdi_host_backup.backup_plan.rules.default.move_to_cold_storage_after_days | -                                                                                      | 1       |
| vdc.vdi_host_backup.backup_plan.rules.default.schedule_expression             | cron(0 5 * * ? *)                                                                      | 1       |
| vdc.vdi_host_backup.backup_plan.rules.default.start_window_minutes            | 60                                                                                     | 1       |
| vdc.vdi_host_backup.backup_plan.selection.tags                                | - Key=idea:ClusterName,Value=<CLUSTER_NAME>                                            | 1       |
|                                                                               | - Key=idea:BackupPlan,Value=vdc                                                        |         |
|                                                                               |                                                                                        |         |
| vdc.vdi_host_backup.enabled                                                   | True                                                                                   | 1       |
+-------------------------------------------------------------------------------+----------------------------------------------------------------------------------------+---------+

```

&#x20;This command takes multiple output format (yaml/raw/table). To continue our example, let's pretend we want to disable the AWS Backup integration. First, query your IDEA configuration to verify if the integration is active

```
./idea-admin.sh config show \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION>
  --query "vdc.vdi_host_backup.enabled"
+-----------------------------+-------+---------+
| Key                         | Value | Version |
+-----------------------------+-------+---------+
| vdc.vdi_host_backup.enabled | True  | 1       |
+-----------------------------+-------+---------+
```

Alternatively, you can validate this setting via the web interface under "**Cluster Settings**"

![](<../../../.gitbook/assets/Screen Shot 2022-12-04 at 5.01.18 PM.png>)

To update this entry, run the `./idea-admin.sh config set` command

```
./idea-admin.sh config set \
  Key=vdc.vdi_host_backup.enabled,Type=bool,Value=False \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION>
+-----------------------------+-------+
| Key                         | Value |
+-----------------------------+-------+
| vdc.vdi_host_backup.enabled | False |
+-----------------------------+-------+
? Are you sure you want to update above config entries? Yes
updating config: vdc.vdi_host_backup.enabled = False

```

{% hint style="info" %}


entry must be of below format: Key=KEY\_NAME,Type=\[str|int|float|bool|list|list|list|list],Value=\[VALUE|\[VALUE1,VALUE2,...]] ... config key names cannot contain: comma(,), colon(:)

Examples:

1. To set a string config type: ./idea-admin.sh config set Key=global-settings.string\_val,Type=string,Value=stringcontent --cluster-name YOUR\_CLUSTER\_NAME --aws-region YOUR\_AWS\_REGION
2. To set an integer config type: ./idea-admin.sh config set Key=global-settings.int\_val,Type=int,Value=12 --cluster-name YOUR\_CLUSTER\_NAME --aws-region YOUR\_AWS\_REGION
3. To set a config with list of strings: ./idea-admin.sh config set "Key=my\_config.string\_list,Type=list,Value=value1,value2" --cluster-name YOUR\_CLUSTER\_NAME --aws-region YOUR\_AWS\_REGION
4. Update multiple config entries: ./idea-admin.sh config set Key=global-settings.string\_val,Type=string,Value=stringcontent\
   "Key=global-settings.integer\_list,Type=list,Value=1,2"\
   "Key=global-settings.string\_list,Type=list,Value=str1,str2"\
   \--cluster-name YOUR\_CLUSTER\_NAME\
   \--aws-region YOUR\_AWS\_REGION
{% endhint %}

You can now re-run the `./idea-admin.sh config show` command to validate the configuration in the IDEA database has been updated correctly

```
./idea-admin.sh config show \
  --cluster-name <CLUSTER_NAME> \
  --aws-region <REGION>
  --query "vdc.vdi_host_backup.enabled"
+-----------------------------+-------+---------+
| Key                         | Value | Version |
+-----------------------------+-------+---------+
| vdc.vdi_host_backup.enabled | False | 2       |
+-----------------------------+-------+---------+

```

