---
title: 将 MSDeploy 用于 dbSqlPackage 提供程序
description: 了解过时的 MSDeploy 提供程序 DbSqlPackage。 查看参数、示例，以及备用 SQL Server 和 Azure SQL 数据库发布工具。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 213b91ab-03e9-431a-80f0-17eed8335abe
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: e9ceb447b078f7219d2fadec2dee4274a74190e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066822"
---
# <a name="using-msdeploy-with-dbsqlpackage-provider"></a>将 MSDeploy 用于 dbSqlPackage 提供程序

DbSqlPackage 是一个 MSDeploy 提供程序，可使你与 SQL Server/Azure SQL 数据库交互。 DbSqlPackage 支持以下操作：  
  
-   **提取**：从活动的 SQL Server 或 Azure SQL 数据库创建数据库快照 (.dacpac) 文件。  
  
-   **发布**：增量更新数据库架构以便匹配源 .dacpac 文件的架构。  
  
-   **DeployReport**：创建将由发布操作完成的更改的 XML 报表。  
  
-   **脚本**：创建等效于由发布操作执行的脚本的 Transact\-SQL 脚本。  
  
有关 DACFx 的详细信息，请参阅 [https://msdn.microsoft.com/library/microsoft.sqlserver.dac.aspx](/dotnet/api/microsoft.sqlserver.dac) 或 [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)（DACFx 命令行工具）上的 DACFx 托管 API 文档。  
  
> [!IMPORTANT]  
> dbSqlPackage 提供程序功能将从 Visual Studio 的下一个主要版本中删除。 有关如何使用 Web Deploy 进行数据库发布的信息，请参阅[用于增量数据库发布的 dbDacFx 提供程序](https://www.iis.net/learn/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing)。  
  
## <a name="command-line-syntax"></a>命令行语法  
将 MSDeploy 与 dbSqlPackage 提供程序一起使用时，采用以下格式的命令行：  
  
```  
  
MSDeploy -verb: MSDeploy-verb -source:dbSqlPackage="Input"[,dbSqlPackage-source-parameters] -dest:dpSqlPackage="Input"[,dbSqlPackage-target-parameters]  
```  
  
## <a name="ms-deploy-verbs"></a>MS-Deploy 谓词  
在 MS-Deploy 命令行上使用 –verb 开关指定 MS-Deploy 谓词。 dbSqlPackage 提供程序支持以下 MSDeploy 谓词：  
  
|谓词|说明|  
|--------|---------------|  
|dump|提供 .dacpac 文件中包含的源数据库的信息（包括名称、版本号和说明）。 在命令行上使用以下格式指定源数据库：<br /><br />msdeploy -verb:dump -source:dbSqlPackage=".dacpac-file-path"|  
|sync|在命令行上使用以下格式指定 dbSqlPackage 操作：<br /><br />msdeploy -verb:sync -source:dbSqlPackage="input" *[,DbSqlPackage-source-parameters] -*dest:dbSqlPackage="input" [,DbSqlPackage-destination-parameters]<br /><br />请参阅以下各节以了解同步谓词的有效源和目标参数。|  
  
## <a name="dbsqlpackage-source"></a>dbSqlPackage 源  
dbSqlPackage 提供程序接受是有效 SQL Server/SQL Azure 连接字符串或是 .dacpac 文件磁盘路径的输入。  指定提供程序输入源的语法如下：  
  
|输入|默认|说明|  
|---------|-----------|---------------|  
|**-source:dbSqlPackage=** {*input*}|**空值**|input 是有效的 SQL Server 或 SQL Azure 连接字符串，或磁盘上的 .dacpac 文件的路径。<br /><br />注意：使用连接字符串作为输入源时，支持的唯一连接字符串属性是 InitialCatalog、DataSource、UserID、Password、IntegratedSecurity、Encrypt、TrustServerCertificate 和 ConnectionTimeout。|  
  
如果输入源是到 SQL Server/Azure SQL 数据库的连接字符串，dbSqlPackage 将从 SQL Server/Azure SQL 数据库提取 .dacpac 文件形式的数据库快照。  
  
源参数包括：  
  
|参数|默认|说明|  
|-------------|-----------|---------------|  
|**Profile**:{ *string*}|空值|指定 DAC 发布配置文件的文件路径。 该配置文件定义在生成结果 dacpac 时要使用的属性和变量的集合。 发布配置文件会被传递至目标，且在执行 Publish、Script 或 DeployReport 操作之一时被用作默认选项。|  
|**DacApplicationName**={ *string* }|数据库名称|定义要在 DACPAC 元数据中存储的应用程序名称。 默认字符串为数据库名称。|  
|**DacMajorVersion** ={*integer*}|**1**|定义要在 DACPAC 元数据中存储的主版本。|  
|**DacMinorVersion**={*integer*}|**0**|定义要在 DACPAC 元数据中存储的次版本。|  
|**DacApplicationDescription**={ *string* }|空值|定义要存储在 DACPAC 元数据中的应用程序说明。|  
|**ExtractApplicationScopedObjectsOnly={True &#124; False}**|**True**|如果为 True，则只从源中提取应用程序范围的对象。 如果为 False，则同时提取应用程序范围的对象和非应用程序范围的对象。|  
|**ExtractReferencedServerScopedElements={True &#124; False}**|**True**|如果为 True，则提取源数据库对象所引用的登录对象、服务器审核对象和凭据对象。|  
|**ExtractIgnorePermissions={True &#124; False}**|**False**|如果为 True，则忽略提取所有已提取对象的权限；如果为 False，则不这样做。|  
|**ExtractStorage={File&#124;Memory}**|**File**|指定在提取过程中使用的架构模型的后备存储的类型。|  
|**ExtractIgnoreExtendedProperties={True&#124;False}**|**False**|指定是否应忽略扩展属性。|  
|**VerifyExtraction = {True&#124;False}**|**False**|指定是否应验证提取的 dacpac。|  
  
## <a name="dbsqlpackage-destination"></a>DbSqlPackage 目标  
dbSqlPackage 提供程序接受是有效 SQL Server/SQL Azure 连接字符串或是 .dacpac 文件磁盘路径的输入作为目标输入。  指定提供程序目标的语法如下：  
  
|输入|默认|说明|  
|---------|-----------|---------------|  
|-**dest:dbSqlPackage**={*input*}|空值|input 是有效的 SQL Server 或 SQL Azure 连接字符串，或磁盘上的 .dacpac 文件的完整或部分路径。 如果 input 是文件路径，则不可指定其他参数。|  
  
以下目标参数可用于所有 dbSqlPackage 操作：  
  
|属性|默认|说明|  
|------------|-----------|---------------|  
|**Action={Publish&#124;DeployReport&#124;Script}**|空值|可选参数，它们指定要在目标处执行的操作。|  
|**AllowDropBlockingAssemblies ={True &#124; False}**|**False**|指定 SqlClr 发布是否删除作为部署计划的一部分的阻塞程序集。 默认情况下，如果必须删除引用程序集，则任何阻塞或引用程序集将阻止程序集更新。|  
|**AllowIncompatiblePlatform={True &#124; False}**|**False**|指定发布操作是否应继续进行，而不管可能存在的不兼容 SQL Server 平台。|  
|**BackupDatabaseBeforeChanges={True &#124; False}**|**False**|在部署任何更改之前，备份数据库。|  
|**BlockOnPossibleDataLoss={ True &#124; False}**|**True**|指定在发布操作可能导致数据丢失的情况下，是否终止发布集。|  
|**BlockWhenDriftDetected={ True &#124; False}**|**True**|指定是否阻止更新其架构与其注册不再匹配或已取消注册的数据库。|  
|**CommentOutSetVarDeclarations= {True &#124; False}**|**False**|指定是否在生成的发布脚本中注释掉 SETVAR 变量声明。 如果你计划在发布时使用 SQLCMD.exe 等工具指定命令行上的值，则可以选择这样做。|  
|**CompareUsingTargetCollation={ True &#124; False}**|**False**|此设置指示在部署过程中如何处理数据库的排序规则；默认情况下，如果目标数据库的排序规则与源所指定的排序规则不匹配，就将更新目标数据库的排序规则。  设置此选项后，应使用目标数据库（或服务器）的排序规则。|  
|**CreateNewDatabase={ True &#124; False}**|**False**|指定当您发布到数据库时，是否应更新目标数据库或是否应删除并重新创建目标数据库。|  
|**DeployDatabaseInSingleUserMode={ True &#124; False}**|**False**|如果为 True，则数据库在部署前将设置为单用户模式。|  
|**DisableAndReenableDdlTriggers={True &#124; False}**|**True**|指定在发布过程开始时是否禁用数据定义语言 (DDL) 触发器，以及在发布操作结束时是否重新启用此类触发器。|  
|**DoNotAlterChangeDataCaptureObjects={ True &#124; False}**|**True**|如果为 True，则不更改变更数据捕获对象。|  
|**DoNotAlterReplicatedObjects=( True &#124; False}**|**True**|指定是否在验证过程中标识重复的对象。|  
|**DropConstraintsNotInSource= {True &#124; False}**|**True**|指定当您发布到数据库时，发布操作是否从目标数据库中删除数据库快照 (.dacpac) 中不存在的约束。|  
|**DropDmlTriggersNotInSource= {True &#124; False}**|**True**|指定当您发布到数据库时，发布操作是否从目标数据库中删除数据库快照 (.dacpac) 中不存在的数据操作语言 (DML) 触发器。|  
|**DropExtendedPropertiesNotInSource= {True &#124; False}**|**True**|指定当您发布到数据库时，发布操作是否从目标数据库中删除数据库快照 (.dacpac) 中不存在的扩展属性。|  
|**DropIndexesNotInSource= {True &#124; False}**|**True**|指定当您发布到数据库时，发布操作是否从目标数据库中删除数据库快照 (.dacpac) 中不存在的索引。|  
|**DropObjectsNotInSource= {True &#124; False}**|**False**|指定当你发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的对象。|  
|**DropPermissionsNotInSource= {True &#124; False}**|**False**|指定当您发布到数据库时，发布操作是否从目标数据库中删除数据库快照 (.dacpac) 中不存在的权限。|  
|**DropRoleMembersNotInSource= {True &#124; False}**|**False**|指定当您发布到数据库时，发布操作是否从目标数据库中删除数据库快照 (.dacpac) 中不存在的角色成员。|  
|**GenerateSmartDefaults={True &#124; False}**|**False**|指定当 SqlPackage.exe 使用一个不允许 Null 值的列更新一个包含数据的表时，是否自动提供默认值。|  
|**IgnoreAnsiNulls= {True &#124; False}**|**False**|指定当你发布到数据库时，是忽略还是更新 ANSI NULLS 设置之间的差异。|  
|**IgnoreAuthorizer= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新授权者之间的差异。|  
|**IgnoreColumnCollation= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新列排序规则之间的差异。|  
|**IgnoreComments= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新注释顺序之间的差异。|  
|**IgnoreCryptographicProviderFile= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新加密提供程序的文件路径之间的差异。|  
|**IgnoreDdlTriggerOrder= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新数据定义语言 (DDL) 触发器顺序之间的差异。|  
|**IgnoreDdlTriggerState={True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新 DDL 触发器的已启用状态或已禁用状态之间的差异。|  
|**IgnoreDefaultSchema={True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新默认架构之间的差异。|  
|**IgnoreDmlTriggerOrder= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新 DML 触发器顺序之间的差异。|  
|**IgnoreDmlTriggerState= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新 DML 触发器的已启用状态或已禁用状态之间的差异。|  
|**IgnoreExtendedProperties= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新扩展属性之间的差异。|  
|**IgnoreFileAndLogFilePath={True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新文件和日志文件的路径之间的差异。|  
|**IgnoreFilegroupPlacement= {True &#124; False}**|**True**|指定当你发布到数据库时，是忽略还是更新 FILEGROUP 的位置之间的差异。|  
|**IgnoreFileSize= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新文件大小之间的差异。|  
|**IgnoreFillFactor= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新填充因子之间的差异。|  
  
|属性|默认|说明|  
|------------|-----------|---------------|  
|**IgnoreFullTextCatalogFilePath= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新全文索引文件的路径之间的差异。|  
|**IgnoreIdentitySeed= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新标识列的种子之间的差异。|  
|**IgnoreIncrement= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新标识列的增量之间的差异。|  
|**IgnoreIndexOptions ={True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新索引选项之间的差异。|  
|**IgnoreIndexPadding= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新索引填充之间的差异。|  
|**IgnoreKeywordCasing= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新关键字大小写之间的差异。|  
|**IgnoreLockHintsOnIndexes= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新索引上的锁提示之间的差异。|  
|**IgnoreLoginSids= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新安全标识符 (SID) 之间的差异。|  
|**IgnoreNotForReplication= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新 not-for-replication 设置之间的差异。|  
|**IgnoreObjectPlacementOnPartitionScheme= {True &#124; False}**|**True**|指定当发布到数据库时，是忽略还是更新对象在分区方案上的位置之间的差异。|  
|**IgnorePartitionSchemes= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新分区方案和函数之间的差异。|  
|**IgnorePermissions= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新权限之间的差异。|  
|**IgnoreQuotedIdentifiers= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新带引号的标识符设置之间的差异。|  
|**IgnoreRoleMembership={True &#124; False}**|**False**|指定当您发布到数据库时，应忽略还是更新登录的角色成员身份之间的差异。|  
|**IgnoreRouteLifetime= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新登录的角色成员身份之间的差异。|  
|**IgnoreSemicolonBetweenState= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新 Transact-SQL 语句之间的分号的差异。|  
|**IgnoreTableOptions= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新表选项之间的差异。|  
|**IgnoreUserSettingsObjects= {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新用户设置选项之间的差异。|  
|**IgnoreWhitespace= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新空白中的差异。|  
|**IgnoreWithNocheckOnCheckConstraints={True &#124; False}**|**False**|指定当你发布到数据库时，是忽略还是更新用于 CHECK 约束的 WITH NOCHECK 子句的值之间的差异。|  
|**IgnoreWithNocheckOnForeignKeys={True &#124; False}**|**False**|指定当你发布到数据库时，是忽略还是更新用于外键的 WITH NOCHECK 子句的值之间的差异。|  
|**IncludeCompositeObjects= {True &#124; False}**|**False**|指定是否将所有复合元素作为单个发布操作的一部分包含。|  
|**IncludeTransactionalScripts={True &#124; False}**|**False**|指定当您发布到数据库时，是否在任何可能的位置使用事务性语句。|  
|**NoAlterStatementsToChangeClrTypes={True &#124; False}**|**False**|指定发布操作在存在差异时应始终删除并重新创建程序集，而不是发出 ALTER ASSEMBLY 语句。|  
|**PopulateFilesOnFilegroups= {True &#124; False}**|**True**|指定当你在目标数据库中创建一个新的 FileGroup 时，是否也创建一个新文件。|  
|**RegisterDataTierApplication={True &#124; False}**|**False**|指定是否向数据库服务器注册该架构。|  
|**ScriptDatabaseCollation {True &#124; False}**|**False**|指定当您发布到数据库时，是忽略还是更新数据库排序规则之间的差异。|  
|**ScriptDatabaseCompatibility= {True &#124; False}**|**True**|指定当您发布到数据库时，是忽略还是更新数据库兼容性之间的差异。|  
|**ScriptDatabaseOptions= {True &#124; False}**|**True**|指定当您发布到数据库时，是设置还是更新目标数据库属性。|  
|**ScriptFileSize={True &#124; False}**|**False**|控制在向文件组添加文件时是否指定大小。|  
|**ScriptNewConstraintValidation= {True &#124; False}**|**True**|指定在发布结束时是否将所有约束作为一个集合进行验证，从而避免 CHECK 约束或外键约束在发布操作过程中导致数据错误。 如果此选项为 False，则将发布约束而不检查相应的数据。|  
|**ScriptDeployStateChecks={True &#124; False}**|**False**|指定是否在发布脚本中生成语句以验证数据库名称和服务器名称是否与数据库项目中指定的名称匹配。|  
|**ScriptRefreshModule= {True &#124; False}**|**True**|指定是否在发布脚本的结尾处包含刷新语句。|  
|**Storage={File&#124;Memory}**|**内存**|指定在生成数据库模型时如何存储元素。 出于性能原因，默认值为 Memory。 对于非常大的数据库，需要文件备份的存储。|  
|**TreatVerificationErrorsAsWarnings= {True &#124; False}**|**False**|指定是否将发布验证过程中出现的错误视为警告。 在对目标数据库执行生成的部署计划之前，会先对该计划执行检查。 计划验证将检测仅有目标的对象（如索引）丢失等问题，必须解决这些问题以进行更改。 验证还检测以下情况：依赖项（如表或视图）因对复合项目的引用而存在，但未存在于目标数据库中。 您可选择将验证错误视为警告以获取问题的完整列表，而不是允许发布操作在出现第一个错误时停止。|  
|**UnmodifiableObjectWarnings= {True &#124; False}**|**True**|指定在无法修改的对象中发现差异（例如，如果文件的大小或路径存在差异）时是否生成警告。|  
|**VerifyCollationCompatibility={True &#124; False}**|**True**|指定是否验证排序规则兼容性。|  
|**VerifyDeployment={True &#124; False}**|**True**|指定在出现可能阻止成功发布的问题的情况下，是否在发布前执行将停止发布操作的检查。 例如，如果您在发布过程中收到错误，则发布操作可能会停止，因为数据库项目中不存在目标数据库上的外键。|  
  
> [!NOTE]  
> 指定的任何目标参数将覆盖源发布配置文件中指定的目标参数。  
  
> [!NOTE]  
> 必须在发布配置文件源参数中指定 SQLCMD 变量和值，因为不能将它们指定为目标参数。  
  
以下目标参数仅限用于 DeployReport 和 Script 操作：  
  
|参数|默认|说明|  
|-------------|-----------|---------------|  
|**OutputPath**={ *string* }|空值|可选参数，它通知 dbSqlPackage 在 string 指定的磁盘位置创建 DeployReport XML 输出文件或 Script SQL 输出文件。 此操作会覆盖当前驻留在 string 指定的位置的任何脚本。|  
  
> [!NOTE]  
> 如果没有为 DeployReport 或 Script 操作提供 OutputPath 参数，则会以消息的形式返回输出结果。  
  
## <a name="examples"></a>示例  
下面是使用 dbSqlPackage 的 Extract 操作的示例语法：  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source connection string>",<source parameter> -dest:dbSqlPackage="<target dacpac file path>"  
```  
  
下面是使用 dbSqlPackage 的 Publish 操作的示例语法：  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Publish,<destination parameters>  
```  
  
下面是使用 dbSqlPackage 的 DeployReport 操作的示例语法：  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=DeployReport,OutputPath="<path to output XML file>",<destination parameters>  
```  
  
下面是使用 dbSqlPackage 的  操作的示例语法：  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Script,OutputPath="<path to output sql script>",<destination parameters>  
```  
