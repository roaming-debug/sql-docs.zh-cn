---
title: SqlPackage.exe
description: 了解如何通过 SqlPackage.exe 自动执行数据库开发任务。 查看示例和可用参数、属性和 SQLCMD 变量。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 8555a183dc1f888e6ae80b78999b5fc234b431cf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594410"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

SqlPackage.exe 是一个命令行实用工具，可自动处理以下数据库开发任务  ：  
  
- [版本](#version)：返回 SqlPackage 应用程序的生成号。  在版本 18.6 中添加。

- [提取](sqlpackage-extract.md)：从活动的 SQL Server 或 Azure SQL 数据库创建数据库快照 (.dacpac) 文件。  
  
- [发布](sqlpackage-publish.md)：增量更新数据库架构以便匹配源 .dacpac 文件的架构。 如果该数据库在服务器上不存在，则发布操作将创建它。 否则，将更新现有数据库。  
  
- [导出](sqlpackage-export.md)：将活动数据库（包括数据库架构和用户数据）从 SQL Server 或 Azure SQL 数据库导出到 BACPAC 包（.bacpac 文件）。  
  
- [导入](sqlpackage-import.md)：将架构和表数据从 BACPAC 包导入到 SQL Server 或 Azure SQL 数据库的实例中的新用户数据库中。  
  
- [DeployReport](sqlpackage-deploy-drift-report.md)：创建将由发布操作完成的更改的 XML 报表。  
  
- [DriftReport](sqlpackage-deploy-drift-report.md)：创建自注册数据库注册以来已对其做出的更改的 XML 报表。  
  
- [脚本](sqlpackage-script.md)：创建 Transact-SQL 增量更新脚本，该脚本可更新目标的架构以匹配源的架构。  
  
通过 SqlPackage.exe 命令行，可以指定这些操作以及特定于操作的参数和属性  。  

[下载最新版本](sqlpackage-download.md)  。 有关最新版本的详细信息，请参阅[发行说明](release-notes-sqlpackage.md)。
  
## <a name="command-line-syntax"></a>命令行语法

SqlPackage.exe 使用在命令行上指定的参数、属性和 SQLCMD 变量启动指定的操作  。  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>用法示例

**使用带有 SQL 脚本输出的 .dacpac 文件生成数据库之间的比较结果**

首先，创建包含最新数据库更改的 .dacpac 文件：

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
创建包含数据库目标（没有更改）的 .dacpac 文件：

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

创建一个 SQL 脚本，用于生成两个 .dacpac 文件之间的差异：

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```

 ## <a name="version"></a>版本

将 sqlpackage 版本显示为生成号。  可在交互提示和[自动管道](sqlpackage-pipelines.md)中使用。

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>退出代码

用于返回以下退出代码的命令：

- 0 = 成功
- 非零 = 失败


## <a name="parameters"></a>参数
某些参数在 SqlPackage 操作之间共享。 下面是汇总参数的表，如需详细信息，请单击特定操作页。

| 参数 | 缩写 | [提取](sqlpackage-extract.md#parameters-for-the-extract-action) | [发布](sqlpackage-publish.md#parameters-for-the-publish-action) | [导出](sqlpackage-export.md#parameters-for-the-export-action) | [导入](sqlpackage-import.md#parameters-for-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-parameters) | [DriftReport](sqlpackage-deploy-drift-report.md#driftreport-action-parameters) | [脚本](sqlpackage-script.md#parameters-for-the-script-action) |
|---|---|---|---|---|---|---|---|---|
|**/AccessToken:**|**/at**| x | x | x | x | x | x | x |
|**/ClientId:**|**/cid**| | x | | | | | |
|**/DeployScriptPath:**|**/dsp**| | x | | | | | x |
|**/DeployReportPath:**|**/drp**| | x | | | | | x |
|**/Diagnostics:**|**/d**| x | x | x | x | x | x | x |
|**/DiagnosticsFile:**|**/df**| x | x | x | x | x | x | x |
|**/MaxParallelism:**|**/mp**| x | x | x | x | x | x | x |
|**/OutputPath:**|**/op**|  |  |  | | x | x | x |
|**/OverwriteFiles:**|**/of**| x | x | x | | x | x | x |
|**/Profile:**|**/pr**| | x | | | x | | x |
|**/Properties:**|**/p**| x | x | x | x | x | | x |
|**/Quiet:**|**/q**| x | x | x | x | x | x | x |
|**/Secret:**|**/secr**| | x | | | | | |
|**/SourceConnectionString:**|**/scs**| x | x | x | | x | | x | x |
|**/SourceDatabaseName:**|**/sdn**| x | x | x | | x | | x |
|**/SourceEncryptConnection:**|**/sec**| x | x | x | | x | | x |
|**/SourceFile:**|**/sf**| | x | | x | x | | x |
|**/SourcePassword:**|**/sp**| x | x | x | | x | | x |
|**/SourceServerName:**|**/ssn**| x | x | x | | x | | x |
|**/SourceTimeout:**|**/st**| x | x | x | | x | | x |
|**/SourceTrustServerCertificate:**|**/stsc**| x | x | x | | x | | x |
|**/SourceUser:**|**/su**| x | x | x | | x | | x |
|**/TargetConnectionString:**|**/tcs**| | | | x | x | x | x |
|**/TargetDatabaseName:**|**/tdn**| | x | | x | x | x | x |
|**/TargetEncryptConnection:**|**/tec**| | x | | x | x | x | x |
|**/TargetFile:**|**/tf**| x | | x | | x | | x |
|**/TargetPassword:**|**/tp**| | x | | x | x | x | x |
|**/TargetServerName:**|**/tsn**| | x | | x | x | x | x |
|**/TargetTimeout:**|**/tt**| | x | | x | x | x | x |
|**/TargetTrustServerCertificate:**|**/ttsc**| | x | | x | x | x | x |
|**/TargetUser:**|**/tu**| | x | | x | x | x | x |
|**/TenantId:**|**/tid**| x | x | x | x | x | x | x |
|**/UniversalAuthentication:**|**/ua**| x | x | x | x | x | x | x |
|**/Variables:**|**/v**| | | | | x | | x |

## <a name="properties"></a>属性
某些属性在 SqlPackage 操作之间共享。  下面是汇总属性的表，如需详细信息，请单击特定操作页。

| 属性 | [提取](sqlpackage-extract.md#properties-specific-to-the-extract-action) | [发布](sqlpackage-publish.md#properties-specific-to-the-publish-action) | [导出](sqlpackage-export.md#properties-specific-to-the-export-action) | [导入](sqlpackage-import.md#properties-specific-to-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-properties) | [脚本](sqlpackage-script.md#properties-specific-to-the-script-action) |
|---|---|---|---|---|---|---|
|AdditionalDeploymentContributorArguments=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributors=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributorPaths=(STRING)| | x | | | x | x |
|AllowDropBlockingAssemblies=(BOOLEAN)| | x | | | x | x |
|AllowIncompatiblePlatform=(BOOLEAN)| | x | | | x | x |
|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)| | x | | | x | x |
|BackupDatabaseBeforeChanges=(BOOLEAN)| | x | | | x | x |
|BlockOnPossibleDataLoss=(BOOLEAN 'True')| | x | | | x | x |
|BlockWhenDriftDetected=(BOOLEAN 'True')| | x | | | x | x |
|CommandTimeout=(INT32 '60')| x | x | x | x | x | x |
|CommentOutSetVarDeclarations=(BOOLEAN)| | x | | | x | x |
|CompareUsingTargetCollation=(BOOLEAN)| | x | | | x | x |
|CreateNewDatabase=(BOOLEAN)| | x | | | x | x |
|DacApplicationDescription=(STRING)| x | | | | | |
|DacApplicationName=(STRING)| x | | | | | |
|DacMajorVersion=(INT32 '1')| x | | | | | |
|DacMinorVersion=(INT32 '0')| x | | | | | |
|DatabaseEdition=(ENUM 'Default')| | x | | x | x | x |
|DatabaseLockTimeout=(INT32 '60')| x | x | x | | x | x |
|DatabaseMaximumSize=(INT32)| | x | | x | x | x |
|DatabaseServiceObjective=(STRING)| | x | | x | x | x |
|DeployDatabaseInSingleUserMode=(BOOLEAN)| | x | | | x | x |
|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterReplicatedObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotDropObjectType=(STRING)| | x | | | x | x |
|DoNotDropObjectTypes=(STRING)| | x | | | x | x |
|DropConstraintsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropDmlTriggersNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropIndexesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropObjectsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropPermissionsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropRoleMembersNotInSource=(BOOLEAN)| | x | | | x | x |
|DropStatisticsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|ExcludeObjectType=(STRING)| | x | | | x | x |
|ExcludeObjectTypes=(STRING)| | x | | | x | x |
|ExtractAllTableData=(BOOLEAN)| x | | | | | |
|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')| x | | | | | |
|ExtractReferencedServerScopedElements=(BOOLEAN 'True')| x | | | | | |
|ExtractUsageProperties=(BOOLEAN)| x | | | | | |
|GenerateSmartDefaults=(BOOLEAN)| | x | | | x | x |
|IgnoreAnsiNulls=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreAuthorizer=(BOOLEAN)| | x | | | x | x |
|IgnoreColumnCollation=(BOOLEAN)| | | | | x | x |
|IgnoreColumnOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreComments=(BOOLEAN)| | x | | | x | x |
|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreDdlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDdlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreDefaultSchema=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreExtendedProperties=(BOOLEAN)| x | x | | | x | x |
|IgnoreFileAndLogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFilegroupPlacement=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFileSize=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFillFactor=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreIdentitySeed=(BOOLEAN)| | x | | | x | x |
|IgnoreIncrement=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexPadding=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreKeywordCasing=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreLockHintsOnIndexes=(BOOLEAN)| | x | | | x | x |
|IgnoreLoginSids=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreNotForReplication=(BOOLEAN)| | x | | | x | x |
|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')| | x | | | x | x |
|IgnorePartitionSchemes=(BOOLEAN)| | x | | | x | x |
|IgnorePermissions=(BOOLEAN 'True')| x | x | | | x | x |
|IgnoreQuotedIdentifiers=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreRoleMembership=(BOOLEAN)| | x | | | x | x |
|IgnoreRouteLifetime=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreTableOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreTablePartitionOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreUserLoginMappings=(BOOLEAN)| x | | | | | |
|IgnoreUserSettingsObjects=(BOOLEAN)| | x | | | x | x |
|IgnoreWhitespace=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)| | x | | | x | |
|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)| | x | | | x | |
|ImportContributorArguments=(STRING)| | | | x | | |
|ImportContributors=(STRING)| | | | x | | |
|ImportContributorPaths=(STRING)| | | | x | | |
|IncludeCompositeObjects=(BOOLEAN)| | x | | | x | x |
|IncludeTransactionalScripts=(BOOLEAN)| | x | | | x | x |
|LongRunningCommandTimeout=(INT32)| x | x | x | x | x | x |
|NoAlterStatementsToChangeClrTypes=(BOOLEAN)| | x | | | x | x |
|PopulateFilesOnFileGroups=(BOOLEAN 'True')| | x | | | x | x |
|RegisterDataTierApplication=(BOOLEAN)| | x | | | x | x |
|RunDeploymentPlanExecutors=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCollation=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCompatibility=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseOptions=(BOOLEAN 'True')| | x | | | x | x |
|ScriptDeployStateChecks=(BOOLEAN)| | x | | | x | x |
|ScriptFileSize=(BOOLEAN)| | x | | | x | x |
|ScriptNewConstraintValidation=(BOOLEAN 'True')| | x | | | x | x |
|ScriptRefreshModule=(BOOLEAN 'True')| | x | | | x | x |
|Storage=({File&#124;Memory} 'File')| x | x | x | x | x | x |
|TableData=(STRING)| x | | x | | | |
|TargetEngineVersion=(ENUM 'Latest')| | | x | | | |
|TempDirectoryForTableData=(STRING)| x | | x | | | |
|TreatVerificationErrorsAsWarnings=(BOOLEAN)| | x | | | x | x |
|UnmodifiableObjectWarnings=(BOOLEAN 'True')| | x | | | x | x |
|VerifyCollationCompatibility=(BOOLEAN 'True')| | x | | | x | x |
|VerifyDeployment=(BOOLEAN 'True')| | x | | | x | x |
|VerifyExtraction=(BOOLEAN)| x | | | | | |
|VerifyFullTextDocumentTypesSupported=(BOOLEAN)| | | x | | | |


## <a name="next-steps"></a>后续步骤

- 了解有关 [SqlPackage 提取](sqlpackage-extract.md)的详细信息
- 了解有关 [SqlPackage 发布](sqlpackage-publish.md)的详细信息
- 了解有关 [SqlPackage 导出](sqlpackage-export.md)的详细信息
- 了解有关 [SqlPackage 导入](sqlpackage-import.md)的详细信息
