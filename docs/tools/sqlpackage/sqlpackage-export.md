---
title: SqlPackage 导出
description: 了解如何通过 SqlPackage.exe 导出自动执行数据库开发任务。 查看示例和可用参数、属性和 SQLCMD 变量。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: c0c3b1462fe165678e3826585f5ce82d5945de56
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577792"
---
# <a name="sqlpackage-export-parameters-and-properties"></a>SqlPackage 导出的参数和属性
SqlPackage.exe 导出操作将活动数据库从 SQL Server 或 Azure SQL 导出到 BACPAC 包（.bacpac 文件）。 默认情况下，所有表的数据将包含在 .bacpac 文件中。 你可以选择仅指定要为其导出数据的表的子集。 即使针对导出指定表的子集，对导出操作的验证可确保 Azure SQL 数据库对完整目标数据库的兼容性。 

## <a name="command-line-syntax"></a>命令行语法

SqlPackage.exe 使用在命令行上指定的参数、属性和 SQLCMD 变量启动指定的操作  。  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-export-action"></a>导出操作的参数

|参数|缩写|值|说明|
|---|---|---|---|
|**/Action:**|**/a**|导出|指定要执行的操作。 |
|**/AccessToken:**|**/at**|{string}| 指定要在连接到目标数据库时使用的基于令牌的身份验证访问令牌。 |
|**/Diagnostics:**|**/d**|{True&#124;False}|指定诊断日志记录是否输出到控制台。 默认为 False。 |
|**/DiagnosticsFile:**|**/df**|{string}|指定一个用于存储诊断日志的文件。 |
|**/MaxParallelism:**|**/mp**|{int}| 指定针对数据库运行的并发操作的并行度。 默认值为 8。 |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|指定 sqlpackage.exe 是否应覆盖现有文件。 指定 false 会导致 sqlpackage.exe 在遇到现有文件时中断操作。 默认值为 True。 |
|**/Properties:**|**/p**|{PropertyName}={Value}|为特定于操作的属性指定名称值对；{PropertyName}={Value}。 请参考特定操作的帮助以便查看该操作的属性名称。 示例：sqlpackage.exe /Action:Export /?。|
|**/Quiet:**|**/q**|{True&#124;False}|指定是否隐藏详细反馈。 默认为 False。|
|**/SourceConnectionString:**|**/scs**|{string}|指定源数据库的有效 SQL Server/Azure 连接字符串。 如果指定了此参数，则应该独立于所有其他源参数来使用此参数。 |
|**/SourceDatabaseName:**|**/sdn**|{string}|定义源数据库的名称。 |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|指定是否应该将 SQL 加密用于源数据库连接。 |
|**/SourcePassword:**|**/sp**|{string}|对于 SQL Server 身份验证方案，定义要用于访问源数据库的密码。 |
|**/SourceServerName:**|**/ssn**|{string}|定义承载源数据库的服务器的名称。 |
|**/SourceTimeout:**|**/st**|{int}|指定建立与源数据库的连接的超时时间（以秒为单位）。 |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|指定是否使用 TLS 对源数据库连接进行加密，并绕过证书链来验证信任。 |
|**/SourceUser:**|**/su**|{string}|对于 SQL Server 身份验证方案，定义要用于访问源数据库的 SQL Server 用户。 |
|**/TargetFile:**|**/tf**|{string}| 指定要用作操作（而不是数据库）目标的目标文件（即 .dacpac 文件）。 如果使用此参数，则其他目标参数应无效。 对于仅支持数据库目标的操作，此参数应该无效。|
|**/TenantId:**|**/tid**|{string}|表示 Azure AD 租户 ID 或域名。 此选项是支持来宾用户或已导入的 Azure AD 用户以及 outlook.com、hotmail.com 或 live.com 等 Microsoft 帐户的必需选项。 如果省略此参数，将使用 Azure AD 的默认租户 ID（假定经过身份验证的用户是此 AD 的本机用户）。 但是，在这种情况下，不支持在此 Azure AD 中托管的任何来宾或已导入的用户和/或 Microsoft 帐户，并且操作将失败。 <br/> 有关 Active Directory 通用身份验证的详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 的通用身份验证（对 MFA 的 SSMS 支持）](/azure/sql-database/sql-database-ssms-mfa-authentication)。|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|指定是否应使用通用身份验证。 如果设置为 True，则激活交互式身份验证协议以支持 MFA。 此选项还可用于在不使用 MFA 时进行 Azure AD 身份验证，方法是使用需要用户输入用户名和密码或集成身份验证（Windows 凭据）的交互式协议。 如果将 /UniversalAuthentication 设置为 True，则不能在 SourceConnectionString (/scs) 中指定 Azure AD 身份验证。 如果将 /UniversalAuthentication 设置为 False，则必须在 SourceConnectionString (/scs) 中指定 Azure AD 身份验证。 <br/> 有关 Active Directory 通用身份验证的详细信息，请参阅 [SQL 数据库和 Azure Synapse Analytics 的通用身份验证（对 MFA 的 SSMS 支持）](/azure/sql-database/sql-database-ssms-mfa-authentication)。|

## <a name="properties-specific-to-the-export-action"></a>特定于 Export 操作的属性

|properties|值|说明|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|指定针对 SQL Server 执行查询时的命令超时（以秒为单位）。|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| 指定针对 SQLServer 执行查询时的数据库锁超时（以秒为单位）。 使用 -1 表示无限期等待。|
|**/p:**|LongRunningCommandTimeout=(INT32)| 指定针对 SQL Server 执行查询时的长时间运行命令超时（以秒为单位）。 使用 0 表示无限期等待。|
|**/p:**|Storage=({File&#124;Memory} 'File')|指定在提取过程中使用的架构模型的后备存储的类型。|
|**/p:**|TableData=(STRING)|指示将从中提取数据的表。 请按以下格式指定表名，不一定要使用括号来括住名称部分：schema_name.table_identifier。 可以多次指定此选项。|
|**/p:**|TempDirectoryForTableData=(STRING)|指定用于在将表数据写入包文件前缓冲表数据的临时目录。|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|指定应使用的目标引擎版本。 这会影响是否允许具有 V12 功能的 Azure SQL 数据库服务器支持的对象，如生成的 bacpac 中的内存优化表。|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|指定是否验证适用于 Microsoft Azure SQL Database v12 的受支持全文文档类型。|

## <a name="next-steps"></a>后续步骤

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息