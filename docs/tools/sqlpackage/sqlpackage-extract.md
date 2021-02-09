---
title: SqlPackage 提取
description: 了解如何通过 SqlPackage.exe 提取自动执行数据库开发任务。 查看示例和可用参数、属性和 SQLCMD 变量。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: c4a9947520ef3914a2ccb34aba5ffaacc1bc6bb2
ms.sourcegitcommit: 5ceafd29b8f22edb800cec150f0ccddea43313e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983640"
---
# <a name="sqlpackage-extract-parameters-and-properties"></a>SqlPackage 提取的参数和属性
SqlPackage.exe 的 Extract 操作会在 DACPAC 文件 (.dacpac) 中创建连接的数据库的架构。 默认情况下，.dacpac 文件中不包含数据。 若要包括数据，请使用[导出操作](sqlpackage-export.md)或使用 Extract 属性 ExtractAllTableData/TableData 。 

## <a name="command-line-syntax"></a>命令行语法

SqlPackage.exe 使用在命令行上指定的参数、属性和 SQLCMD 变量启动指定的操作  。  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> 提取具有密码凭证（例如 SQL 身份验证用户）的数据库时，密码将替换为具有合适复杂性的不同密码。 SqlPackage 或 DacFx 用户应在 dacpac 发布后更改密码。

## <a name="parameters-for-the-extract-action"></a>提取操作的参数

|参数|缩写|值|说明|
|---|---|---|---|
|**/Action:**|**/a**|Extract|指定要执行的操作。 |
|**/AccessToken:**|**/at**|{string}| 指定要在连接到目标数据库时使用的基于令牌的身份验证访问令牌。 |
|**/Diagnostics:**|**/d**|{True&#124;False}|指定诊断日志记录是否输出到控制台。 默认为 False。 |
|**/DiagnosticsFile:**|**/df**|{string}|指定一个用于存储诊断日志的文件。 |
|**/MaxParallelism:**|**/mp**|{int}| 指定针对数据库运行的并发操作的并行度。 默认值为 8。 |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|指定 sqlpackage.exe 是否应覆盖现有文件。 指定 false 会导致 sqlpackage.exe 在遇到现有文件时中断操作。 默认值为 True。 |
|**/Properties:**|**/p**|{PropertyName}={Value}|为特定于操作的属性指定名称值对；{PropertyName}={Value}。 请参考特定操作的帮助以便查看该操作的属性名称。 示例：sqlpackage.exe /Action:Extract /?。 |
|**/Quiet:**|**/q**|{True&#124;False}|指定是否隐藏详细反馈。 默认为 False。 |
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

## <a name="properties-specific-to-the-extract-action"></a>特定于 Extract 操作的属性

|properties|值|说明|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|指定针对 SQL Server 执行查询时的命令超时（以秒为单位）。|
|**/p:**|DacApplicationDescription=(STRING)|定义要存储在 DACPAC 元数据中的应用程序说明。|
|**/p:**|DacApplicationName=(STRING)|定义要存储在 DACPAC 元数据中的应用程序名称。 默认值为数据库名称。|
|**/p:**|DacMajorVersion=(INT32 '1')|定义要在 DACPAC 元数据中存储的主版本。|
|**/p:**|DacMinorVersion=(INT32 '0')|定义要在 DACPAC 元数据中存储的次版本。|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| 指定针对 SQLServer 执行查询时的数据库锁超时（以秒为单位）。 使用 -1 表示无限期等待。|
|**/p:**|ExtractAllTableData=(BOOLEAN)|指示是否提取所有用户表中的数据。 如果为“true”，则提取所有用户表中的数据，并且不能指定单个用户表来提取数据。 如果设置为“false”，则指定要从中提取数据的一个或多个用户表。|
|**/p:**|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')|如果为 true，则只为指定的源提取应用程序范围的对象。 如果为 false，则为指定的源提取所有对象。|
|**/p:**|ExtractReferencedServerScopedElements=(BOOLEAN 'True')|如果为 true，则提取源数据库对象所引用的登录对象、服务器审核对象和凭据对象。|
|**/p:**|ExtractUsageProperties=(BOOLEAN)|指定了使用情况属性，例如表行数和索引大小，是否将从数据库中提取。|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|指定是否应忽略扩展属性。|
|**/p:**|IgnorePermissions=(BOOLEAN 'True')|指定是否应忽略权限。|
|**/p:**|IgnoreUserLoginMappings=(BOOLEAN)|指定是否忽略用户和登录名之间的关系。|
|**/p:**|LongRunningCommandTimeout=(INT32)| 指定针对 SQL Server 执行查询时的长时间运行命令超时（以秒为单位）。 使用 0 表示无限期等待。|
|**/p:**|Storage=({File&#124;Memory} 'File')|指定在提取过程中使用的架构模型的后备存储的类型。|
|**/p:**|TableData=(STRING)|指示将从中提取数据的表。 请按以下格式指定表名，不一定要使用括号来括住名称部分：schema_name.table_identifier。 可以多次指定此选项。|
|**/p:**| TempDirectoryForTableData=(STRING)|指定用于在将表数据写入包文件前缓冲表数据的临时目录。|
|**/p:**|VerifyExtraction=(BOOLEAN)|指定是否应验证提取的 dacpac。|

## <a name="next-steps"></a>后续步骤

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息