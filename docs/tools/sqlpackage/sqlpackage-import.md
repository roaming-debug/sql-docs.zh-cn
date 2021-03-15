---
title: SqlPackage 导入
description: 了解如何通过 SqlPackage.exe 导入自动执行数据库开发任务。 查看示例和可用参数、属性和 SQLCMD 变量。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 3/10/2021
ms.openlocfilehash: 94ddccc0789f01f3d7d6ac6675ec9c54405972a1
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622623"
---
# <a name="sqlpackage-import-parameters-and-properties"></a>SqlPackage 导入参数和属性
SqlPackage.exe 导入操作将架构和表数据从 BACPAC 包（.bacpac 文件）导入 SQL Server 或 Azure SQL 数据库中的新数据库或空白数据库。 对现有数据库执行导入操作时，目标数据库无法包含任何用户定义的架构对象。  

## <a name="command-line-syntax"></a>命令行语法

SqlPackage.exe 使用在命令行上指定的参数、属性和 SQLCMD 变量启动指定的操作  。  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-import-action"></a>导入操作的参数

|参数|缩写|值|说明|
|---|---|---|---|
|**/Action:**|**/a**|导入|指定要执行的操作。 |
|**/AccessToken:**|**/at**|{string}| 指定要在连接到目标数据库时使用的基于令牌的身份验证访问令牌。 |
|**/Diagnostics:**|**/d**|{True&#124;False}|指定诊断日志记录是否输出到控制台。 默认为 False。 |
|**/DiagnosticsFile:**|**/df**|{string}|指定一个用于存储诊断日志的文件。 |
|**/MaxParallelism:**|**/mp**|{int}| 指定针对数据库运行的并发操作的并行度。 默认值为 8。 |
|**/Properties:**|**/p**|{PropertyName}={Value}|为特定于操作的属性指定名称值对；{PropertyName}={Value}。 请参考特定操作的帮助以便查看该操作的属性名称。 示例：sqlpackage.exe /Action:Import /?。|
|**/Quiet:**|**/q**|{True&#124;False}|指定是否隐藏详细反馈。 默认为 False。|
|**/SourceFile:**|**/sf**|{string}|指定要用作操作源的源文件。 如果使用此参数，则其他源参数应无效。 |
|**/TargetConnectionString:**|**/tcs**|{string}|指定目标数据库的有效 SQL Server/Azure 连接字符串。 如果指定了此参数，则应该独立于所有其他目标参数来使用此参数。 |
|**/TargetDatabaseName:**|**/tdn**|{string}|指定作为 sqlpackage.exe 操作目标的数据库的替代名称。 |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|指定是否应将 SQL 加密用于目标数据库连接。 |
|**/TargetPassword:**|**/tp**|{string}|对于 SQL Server 身份验证方案，定义要用于访问目标数据库的密码。 |
|**/TargetServerName:**|**/tsn**|{string}|定义承载目标数据库的服务器的名称。 |
|**/TargetTimeout:**|**/tt**|{int}|指定建立与目标数据库的连接的超时时间（以秒为单位）。 对于 Azure AD，建议此值大于或等于 30 秒。|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|指定是否使用 TLS 对目标数据库连接进行加密，并绕过证书链来验证信任。 |
|**/TargetUser:**|**/tu**|{string}|对于 SQL Server 身份验证方案，定义要用于访问目标数据库的 SQL Server 用户。 |
|**/TenantId:**|**/tid**|{string}|表示 Azure AD 租户 ID 或域名。 此选项是支持来宾用户或已导入的 Azure AD 用户以及 outlook.com、hotmail.com 或 live.com 等 Microsoft 帐户的必需选项。 如果省略此参数，将使用 Azure AD 的默认租户 ID（假定经过身份验证的用户是此 AD 的本机用户）。 但是，在这种情况下，不支持在此 Azure AD 中托管的任何来宾或已导入的用户和/或 Microsoft 帐户，并且操作将失败。 <br/> 有关 Active Directory 通用身份验证的详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 的通用身份验证（对 MFA 的 SSMS 支持）](/azure/sql-database/sql-database-ssms-mfa-authentication)。|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|指定是否应使用通用身份验证。 如果设置为 True，则激活交互式身份验证协议以支持 MFA。 此选项还可用于在不使用 MFA 时进行 Azure AD 身份验证，方法是使用需要用户输入用户名和密码或集成身份验证（Windows 凭据）的交互式协议。 如果将 /UniversalAuthentication 设置为 True，则不能在 SourceConnectionString (/scs) 中指定 Azure AD 身份验证。 如果将 /UniversalAuthentication 设置为 False，则必须在 SourceConnectionString (/scs) 中指定 Azure AD 身份验证。 <br/> 有关 Active Directory 通用身份验证的详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 的通用身份验证（对 MFA 的 SSMS 支持）](/azure/sql-database/sql-database-ssms-mfa-authentication)。|

## <a name="properties-specific-to-the-import-action"></a>特定于导入操作的属性

|属性|“值”|说明|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|指定针对 SQL Server 执行查询时的命令超时（以秒为单位）。|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|定义 Azure SQL 数据库的版本。|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| 指定针对 SQLServer 执行查询时的数据库锁超时（以秒为单位）。 使用 -1 表示无限期等待。|
|**/p:**|DatabaseMaximumSize=(INT32)|定义 Azure SQL 数据库的大小上限（以 GB 为单位）。|
|**/p:**|DatabaseServiceObjective=(STRING)|定义 Azure SQL 数据库的性能级别，如“P0”或“S1”。|
|**/p:**|DisableIndexesForDataPhase=(BOOLEAN TRUE)|在将数据导入 SQL Server 之前禁用索引。|
|**/p:**|ImportContributorArguments=(STRING)|为部署参与者指定部署参与者参数。 这应该是用分号分隔的值列表。|
|**/p:**|ImportContributors=(STRING)|指定应在导入 dacpac 时运行的部署参与者。 这应该是以分号分隔的完全限定的生成参与者名称或 ID 列表。|
|**/p:**|ImportContributorPaths=(STRING)|指定用于加载其他部署参与者的路径。 这应该是用分号分隔的值列表。 |
|**/p:**|LongRunningCommandTimeout=(INT32)| 指定针对 SQL Server 执行查询时的长时间运行命令超时（以秒为单位）。 使用 0 表示无限期等待。|
|**/p:**|RebuildIndexesOfflineForDataPhase=(BOOLEAN FALSE)|在将数据导入 SQL Server 后脱机重新生成索引。|
|**/p:**|Storage=({File&#124;Memory})|指定在生成数据库模型时如何存储元素。 出于性能原因，默认值为 InMemory。 对于大型数据库，需要备份了文件的存储。|
  

## <a name="next-steps"></a>后续步骤

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息