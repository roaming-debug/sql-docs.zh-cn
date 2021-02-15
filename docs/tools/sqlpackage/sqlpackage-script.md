---
title: SqlPackage 脚本
description: 了解如何通过 SqlPackage.exe 脚本自动执行数据库开发任务。 查看示例和可用参数、属性和 SQLCMD 变量。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/4/2020
ms.openlocfilehash: ffd92afe2a5e57b4c039ead2dd5fee6e2a23be0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060922"
---
# <a name="sqlpackage-script-parameters-and-properties"></a>SqlPackage 脚本的参数和属性
SqlPackage.exe 脚本操作会创建 Transact-SQL 增量更新脚本，该脚本可更新目标数据库的架构以匹配源数据库的架构。 

## <a name="command-line-syntax"></a>命令行语法

SqlPackage.exe 使用在命令行上指定的参数、属性和 SQLCMD 变量启动指定的操作  。  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-script-action"></a>脚本操作的参数

|参数|缩写|值|说明|
|---|---|---|---|
|**/Action:**|**/a**|Script|指定要执行的操作。 |
|**/AccessToken:**|**/at**|{string}| 指定要在连接到目标数据库时使用的基于令牌的身份验证访问令牌。 |
|**/DeployScriptPath:**|**/dsp**|{string}|指定用于输出部署脚本的可选文件路径。 对于 Azure 部署，如果有用于创建或修改 master 数据库的 TSQL 命令，脚本便会写入相同路径，不同之处在于使用“Filename_Master.sql”作为输出文件名。 |
|**/DeployReportPath:**|**/drp**|{string}|指定用于输出部署报告 xml 文件的可选文件路径。 |
|**/Diagnostics:**|**/d**|{True&#124;False}|指定诊断日志记录是否输出到控制台。 默认为 False。 |
|**/DiagnosticsFile:**|**/df**|{string}|指定一个用于存储诊断日志的文件。 |
|**/MaxParallelism:**|**/mp**|{int}| 指定针对数据库运行的并发操作的并行度。 默认值为 8。 |
|**/OutputPath:**|**/op**|{string}|指定生成输出文件的文件路径。 |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|指定 sqlpackage.exe 是否应覆盖现有文件。 指定 false 会导致 sqlpackage.exe 在遇到现有文件时中断操作。 默认值为 True。 |
|**/Profile:**|**/pr**|{string}|指定 DAC 发布配置文件的文件路径。 该配置文件定义在生成输出时要使用的属性和变量的集合。|
|**/Properties:**|**/p**|{PropertyName}={Value}|为特定于操作的属性指定名称值对；{PropertyName}={Value}。 请参考特定操作的帮助以便查看该操作的属性名称。 示例：sqlpackage.exe /Action:Script /?。|
|**/Quiet:**|**/q**|{True&#124;False}|指定是否隐藏详细反馈。 默认为 False。|
|**/SourceConnectionString:**|**/scs**|{string}|指定源数据库的有效 SQL Server/Azure 连接字符串。 如果指定了此参数，则应该独立于所有其他源参数来使用此参数。 |
|**/SourceDatabaseName:**|**/sdn**|{string}|定义源数据库的名称。 |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|指定是否应该将 SQL 加密用于源数据库连接。 |
|**/SourceFile:**|**/sf**|{string}|指定要用作操作源的源文件。 如果使用此参数，则其他源参数应无效。 |
|**/SourcePassword:**|**/sp**|{string}|对于 SQL Server 身份验证方案，定义要用于访问源数据库的密码。 |
|**/SourceServerName:**|**/ssn**|{string}|定义承载源数据库的服务器的名称。 |
|**/SourceTimeout:**|**/st**|{int}|指定建立与源数据库的连接的超时时间（以秒为单位）。 |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|指定是否使用 TLS 对源数据库连接进行加密，并绕过证书链来验证信任。 |
|**/SourceUser:**|**/su**|{string}|对于 SQL Server 身份验证方案，定义要用于访问源数据库的 SQL Server 用户。 |
|**/TargetConnectionString:**|**/tcs**|{string}|指定目标数据库的有效 SQL Server/Azure 连接字符串。 如果指定了此参数，则应该独立于所有其他目标参数来使用此参数。 |
|**/TargetDatabaseName:**|**/tdn**|{string}|指定作为 sqlpackage.exe 操作目标的数据库的替代名称。 |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|指定是否应将 SQL 加密用于目标数据库连接。 |
|**/TargetFile:**|**/tf**|{string}| 指定要用作操作（而不是数据库）目标的目标文件（即 .dacpac 文件）。 如果使用此参数，则其他目标参数应无效。 对于仅支持数据库目标的操作，此参数应该无效。|
|**/TargetPassword:**|**/tp**|{string}|对于 SQL Server 身份验证方案，定义要用于访问目标数据库的密码。 |
|**/TargetServerName:**|**/tsn**|{string}|定义承载目标数据库的服务器的名称。 |
|**/TargetTimeout:**|**/tt**|{int}|指定建立与目标数据库的连接的超时时间（以秒为单位）。 对于 Azure AD，建议此值大于或等于 30 秒。|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|指定是否使用 TLS 对目标数据库连接进行加密，并绕过证书链来验证信任。 |
|**/TargetUser:**|**/tu**|{string}|对于 SQL Server 身份验证方案，定义要用于访问目标数据库的 SQL Server 用户。 |
|**/TenantId:**|**/tid**|{string}|表示 Azure AD 租户 ID 或域名。 此选项是支持来宾用户或已导入的 Azure AD 用户以及 outlook.com、hotmail.com 或 live.com 等 Microsoft 帐户的必需选项。 如果省略此参数，将使用 Azure AD 的默认租户 ID（假定经过身份验证的用户是此 AD 的本机用户）。 但是，在这种情况下，不支持在此 Azure AD 中托管的任何来宾或已导入的用户和/或 Microsoft 帐户，并且操作将失败。 <br/> 有关 Active Directory 通用身份验证的详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 的通用身份验证（对 MFA 的 SSMS 支持）](/azure/sql-database/sql-database-ssms-mfa-authentication)。|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|指定是否应使用通用身份验证。 如果设置为 True，则激活交互式身份验证协议以支持 MFA。 此选项还可用于在不使用 MFA 时进行 Azure AD 身份验证，方法是使用需要用户输入用户名和密码或集成身份验证（Windows 凭据）的交互式协议。 如果将 /UniversalAuthentication 设置为 True，则不能在 SourceConnectionString (/scs) 中指定 Azure AD 身份验证。 如果将 /UniversalAuthentication 设置为 False，则必须在 SourceConnectionString (/scs) 中指定 Azure AD 身份验证。 <br/> 有关 Active Directory 通用身份验证的详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 的通用身份验证（对 MFA 的 SSMS 支持）](/azure/sql-database/sql-database-ssms-mfa-authentication)。|
|**/Variables:**|**/v**|{PropertyName}={Value}|为特定于操作的变量指定名称值对；{VariableName}={Value}。 该 DACPAC 文件包含有效 SQLCMD 变量的列表。 如果没有为每个变量都提供值，则会发生错误。 |

## <a name="properties-specific-to-the-script-action"></a>特定于 Script 操作的属性

|properties|值|说明|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|为部署参与者指定其他部署参与者参数。 这应该是用分号分隔的值列表。
|**/p:**|AdditionalDeploymentContributors=(STRING)|指定应在部署 dacpac 时运行的其他部署参与者。 这应该是以分号分隔的完全限定的生成参与者名称或 ID 列表。
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| 指定用于加载其他部署参与者的路径。 这应该是用分号分隔的值列表。 | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|SqlClr 部署使用此属性以导致阻塞程序集作为部署计划的组成部分被删除。 默认情况下，如果需要删除引用程序集，则任何阻塞或引用程序集将阻止程序集更新。
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|指定是否尝试操作，而不管存在的不兼容 SQL Server 平台。
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|如果此属性设置为 true，则不阻止具有行级别安全性的表上的数据运动。 默认值为 false。
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|在部署任何更改之前，备份数据库。
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')| 指定在产生的架构更改可能会导致数据丢失（包括由于数据精度降低，或由于需要强制转换操作的数据类型更改）的情况下，将在架构验证步骤期间终止操作。 默认值 (`True`) 会导致操作终止，而无论目标数据库是否包含数据。  如果目标中存在无法转换为新列类型的数据，则将 `False` 设为 BlockOnPossibleDataLoss 的值来执行操作仍可能会在执行部署计划期间失败。 |
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|指定是否阻止更新其架构与其注册不再匹配或已取消注册的数据库。
|**/p:**|CommandTimeout=(INT32 '60')|指定针对 SQL Server 执行查询时的命令超时（以秒为单位）。
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|指定是否应在生成的发布脚本中注释掉 SETVAR 变量的声明。 如果您打算在发布时使用 SQLCMD.EXE 等工具指定命令行上的值，则可以选择这样做。
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|此设置指示在部署过程中如何处理数据库的排序规则；默认情况下，如果目标数据库的排序规则与源所指定的排序规则不匹配，就将更新目标数据库的排序规则。 设置此选项后，应使用目标数据库（或服务器）的排序规则。|
|**/p:**|CreateNewDatabase=(BOOLEAN)|指定当您发布到数据库时，是否应更新目标数据库或是否应删除并重新创建目标数据库。
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|定义 Azure SQL 数据库的版本。|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| 指定针对 SQLServer 执行查询时的数据库锁超时（以秒为单位）。 使用 -1 表示无限期等待。|
|**/p:**|DatabaseMaximumSize=(INT32)|定义 Azure SQL 数据库的大小上限（以 GB 为单位）。
|**/p:**|DatabaseServiceObjective=(STRING)|定义 Azure SQL 数据库的性能级别，如“P0”或“S1”。
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|如果为 true，则数据库在部署前将设置为单用户模式。
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| 指定在发布过程开始时是否禁用数据定义语言 (DDL) 触发器，以及在发布操作结束时是否重新启用此类触发器。|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|如果为 true，则不更改变更数据捕获对象。
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|指定是否在验证过程中标识重复的对象。
|**/p:**|DoNotDropObjectType=(STRING)|当 DropObjectsNotInSource 为 True 时不应删除的对象类型。 有效的对象类型名称为 Aggregates、ApplicationRoles、Assemblies、AsymmetricKeys、BrokerPriorities、Certificates、ColumnEncryptionKeys、ColumnMasterKeys、Contracts、DatabaseRoles、DatabaseTriggers、Defaults、ExtendedProperties、ExternalDataSources、ExternalFileFormats、ExternalTables、Filegroups、FileTables、FullTextCatalogs、FullTextStoplists、MessageTypes、PartitionFunctions、PartitionSchemes、Permissions、Queues、RemoteServiceBindings、RoleMembership、Rules、ScalarValuedFunctions、SearchPropertyLists、SecurityPolicies、Sequences、Services、Signatures、StoredProcedures、SymmetricKeys、Synonyms、Tables、TableValuedFunctions、UserDefinedDataTypes、UserDefinedTableTypes、ClrUserDefinedTypes、Users、Views、XmlSchemaCollections、Audits、Credentials、CryptographicProviders、DatabaseAuditSpecifications、DatabaseScopedCredentials、Endpoints、ErrorMessages、EventNotifications、EventSessions、LinkedServerLogins、LinkedServers、Logins、Routes、ServerAuditSpecifications、ServerRoleMembership、ServerRoles、ServerTriggers。
|**/p:**|DoNotDropObjectTypes=(STRING)|当 DropObjectsNotInSource 为 True 时不应删除的以分号分隔的对象类型列表。 有效的对象类型名称为 Aggregates、ApplicationRoles、Assemblies、AsymmetricKeys、BrokerPriorities、Certificates、ColumnEncryptionKeys、ColumnMasterKeys、Contracts、DatabaseRoles、DatabaseTriggers、Defaults、ExtendedProperties、ExternalDataSources、ExternalFileFormats、ExternalTables、Filegroups、FileTables、FullTextCatalogs、FullTextStoplists、MessageTypes、PartitionFunctions、PartitionSchemes、Permissions、Queues、RemoteServiceBindings、RoleMembership、Rules、ScalarValuedFunctions、SearchPropertyLists、SecurityPolicies、Sequences、Services、Signatures、StoredProcedures、SymmetricKeys、Synonyms、Tables、TableValuedFunctions、UserDefinedDataTypes、UserDefinedTableTypes、ClrUserDefinedTypes、Users、Views、XmlSchemaCollections、Audits、Credentials、CryptographicProviders、DatabaseAuditSpecifications、DatabaseScopedCredentials、Endpoints、ErrorMessages、EventNotifications、EventSessions、LinkedServerLogins、LinkedServers、Logins、Routes、ServerAuditSpecifications、ServerRoleMembership、ServerRoles、ServerTriggers。
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|指定当发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的约束。|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|指定当发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的 DML 触发器。|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|指定在发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的扩展属性。|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|指定当发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的索引。|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|指定当发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的对象。 此值优先于 DropExtendedProperties。|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|指定当您更新到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的权限。|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|指定当您更新到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中未定义的角色成员。|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|指定当发布到数据库时，是否将从目标数据库中删除数据库快照 (.dacpac) 文件中不存在的统计信息。|
|**/p:**|ExcludeObjectType=(STRING)|应在部署期间忽略的对象类型。 有效的对象类型名称为 Aggregates、ApplicationRoles、Assemblies、AsymmetricKeys、BrokerPriorities、Certificates、ColumnEncryptionKeys、ColumnMasterKeys、Contracts、DatabaseRoles、DatabaseTriggers、Defaults、ExtendedProperties、ExternalDataSources、ExternalFileFormats、ExternalTables、Filegroups、FileTables、FullTextCatalogs、FullTextStoplists、MessageTypes、PartitionFunctions、PartitionSchemes、Permissions、Queues、RemoteServiceBindings、RoleMembership、Rules、ScalarValuedFunctions、SearchPropertyLists、SecurityPolicies、Sequences、Services、Signatures、StoredProcedures、SymmetricKeys、Synonyms、Tables、TableValuedFunctions、UserDefinedDataTypes、UserDefinedTableTypes、ClrUserDefinedTypes、Users、Views、XmlSchemaCollections、Audits、Credentials、CryptographicProviders、DatabaseAuditSpecifications、DatabaseScopedCredentials、Endpoints、ErrorMessages、EventNotifications、EventSessions、LinkedServerLogins、LinkedServers、Logins、Routes、ServerAuditSpecifications、ServerRoleMembership、ServerRoles、ServerTriggers。
|**/p:**|ExcludeObjectTypes=(STRING)|在部署期间应忽略的以分号分隔的对象类型列表。 有效的对象类型名称为 Aggregates、ApplicationRoles、Assemblies、AsymmetricKeys、BrokerPriorities、Certificates、ColumnEncryptionKeys、ColumnMasterKeys、Contracts、DatabaseRoles、DatabaseTriggers、Defaults、ExtendedProperties、ExternalDataSources、ExternalFileFormats、ExternalTables、Filegroups、FileTables、FullTextCatalogs、FullTextStoplists、MessageTypes、PartitionFunctions、PartitionSchemes、Permissions、Queues、RemoteServiceBindings、RoleMembership、Rules、ScalarValuedFunctions、SearchPropertyLists、SecurityPolicies、Sequences、Services、Signatures、StoredProcedures、SymmetricKeys、Synonyms、Tables、TableValuedFunctions、UserDefinedDataTypes、UserDefinedTableTypes、ClrUserDefinedTypes、Users、Views、XmlSchemaCollections、Audits、Credentials、CryptographicProviders、DatabaseAuditSpecifications、DatabaseScopedCredentials、Endpoints、ErrorMessages、EventNotifications、EventSessions、LinkedServerLogins、LinkedServers、Logins、Routes、ServerAuditSpecifications、ServerRoleMembership、ServerRoles、ServerTriggers。
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|在使用一个不允许 Null 值的列更新一个包含数据的表时，自动提供默认值。
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新 ANSI NULLS 设置方面的差异。
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新授权者方面的差异。
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新列排序规则方面的差异。
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|指定在发布到数据库时是否应忽略或更新表列顺序中的差异。|
|**/p:**|IgnoreComments=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新注释方面的差异。
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新加密提供程序的文件路径方面的差异。
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|指定在发布到数据库或服务器时，是应忽略还是更新数据定义语言 (DDL) 触发器中的顺序差异。|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新数据定义语言 (DDL) 触发器的启用状态或禁用状态方面的差异。|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新默认架构方面的差异。|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|指定在发布到数据库时，是应忽略还是更新数据操作语言 (DML) 触发器中的顺序差异。|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新 DML 触发器的启用状态或禁用状态方面的差异。|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新扩展属性方面的差异。|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新文件和日志文件路径方面的差异。|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新 FILEGROUP 中的对象位置方面的差异。|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略文件的大小差异，还是应发出警告。|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|指定在进行发布时，是应忽略索引存储的填充因子方面的差异，还是应发出警告。|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略全文目录文件路径方面的差异，还是应发出警告。|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新标识列种子方面的差异。|
|**/p:**|IgnoreIncrement=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新标识列增量方面的差异。|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新索引选项方面的差异。|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新索引填充方面的差异。|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新关键字的大小写方面的差异。|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|指定当您发布到数据库时，应忽略还是更新索引上的锁提示之间的差异。|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')| 指定在发布到数据库时，是应忽略还是应更新安全标识号 (SID) 方面的差异。|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新不用于复制设置。|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新对象在分区方案中的位置。|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新分区方案和分区函数方面的差异。|
|**/p:**|IgnorePermissions=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新权限方面的差异。|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|指定在发布到数据库时，是应忽略还是应更新带引号的标识符设置方面的差异。|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|指定当您发布到数据库时，应忽略还是更新登录的角色成员身份之间的差异。|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|指定在发布到数据库时，应忽略还是应更新 SQL Server 保留路由表中的路由的时间量方面的差异。|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|指定在发布到数据库时，是将忽略还是将更新 T-SQL 语句之间的分号差异。|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|指定在发布到数据库时，是将忽略还是将更新表选项方面的差异。|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|指定在发布到数据库时，是将忽略还是将更新表分区选项方面的差异。  此选项仅适用于 Azure Synapse Analytics 数据仓库数据库。|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|指定在发布到数据库时，是将忽略还是将更新用户设置对象方面的差异。|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|指定在发布到数据库时，是将忽略还是将更新空白方面的差异。|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|指定在进行发布时，是将忽略还是将更新 CHECK 约束的 WITH NOCHECK 子句值方面的差异。|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|指定在发布到数据库时，是将忽略还是将更新外键的 WITH NOCHECK 子句值方面的差异。|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|将所有复合元素包含为单个发布操作的一部分。|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|指定在发布到数据库时，是否应在可能的位置使用事务性语句。|
|**/p:**|LongRunningCommandTimeout=(INT32)| 指定针对 SQL Server 执行查询时的长时间运行命令超时（以秒为单位）。 使用 0 表示无限期等待。|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|指定发布操作在存在差异时应始终删除并重新创建程序集，而不是发出 ALTER ASSEMBLY 语句。|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|指定当在目标数据库中创建新 FileGroup 时，是否也创建新文件。|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|指定是否向数据库服务器注册该架构。|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|指定是否应在执行其他操作时运行 DeploymentPlanExecutor 参与者。|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新数据库排序规则方面的差异。|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|指定在发布到数据库时，是应忽略还是应更新数据库兼容性方面的差异。|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|指定是否应作为发布操作的一部分来设置或更新目标数据库属性。|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|指定是否在发布脚本中生成声明，以验证数据库名称和服务器名称是否与数据库项目中指定的名称匹配。|
|**/p:**|ScriptFileSize=(BOOLEAN)|控制在向文件组添加文件时是否指定大小。|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|在发布结束时将所有约束作为一个集合进行验证，从而避免 CHECK 约束或外键约束在发布过程中导致数据错误。 如果设置为“False”，则将发布约束而不检查对应的数据。|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|在发布脚本的末尾包括刷新语句。|
|**/p:**|Storage=({File&#124;Memory})|指定在生成数据库模型时如何存储元素。 出于性能原因，默认值为 InMemory。 对于大型数据库，需要备份了文件的存储。|
|**/p:**|TreatVerificationErrorsAsWarnings=(BOOLEAN)|指定是否应将发布验证过程中遇到的错误视为警告。 在对目标数据库执行生成的部署计划之前，会先对该计划执行检查。 计划验证将检测仅有目标的对象（如索引）丢失等问题，必须解决这些问题以进行更改。 验证还检测以下情况：依赖项（如表或视图）因对复合项目的引用而存在，但未存在于目标数据库中。 可以选择执行此操作以获取所有问题的完整列表，而不是在出现第一个错误时停止发布操作。|
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|指定在无法修改的对象中发现差异（例如，如果文件的大小或路径存在差异）时是否生成警告。|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|指定是否验证排序规则兼容性。
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|指定在出现可能阻止成功发布的问题的情况下，是否应在发布前执行将停止发布操作的检查。 例如，如果具有在数据库项目中不存在的目标数据库上的外键并且在发布时导致错误，则发布操作可能会停止。|

## <a name="next-steps"></a>后续步骤

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息