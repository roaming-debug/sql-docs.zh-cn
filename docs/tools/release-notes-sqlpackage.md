---
title: DacFx 和 SqlPackage 发行说明
description: Microsoft sqlpackage 的发行说明。
ms.custom: tools|sos
ms.date: 02/02/2019
ms.prod: sql
ms.reviewer: alayu; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: pensivebrian
ms.author: broneill
manager: kenvh
ms.openlocfilehash: d78df7ed144c1cb94f486a20bd65ffc9c1c9cafb
ms.sourcegitcommit: d35d0901296580bfceda6e0ab2e14cf2b7e99a0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92496702"
---
# <a name="release-notes-for-sqlpackageexe"></a>SqlPackage.exe 的发行说明

[下载最新版本](sqlpackage-download.md) 

本文列出了 SqlPackage.exe 的已发布版本提供的功能和修补程序。

<!--
TODO:
The Introduction text needs to add clarity regarding the relationship between
'SqlPackage.exe' and 'DacFx' (the Microsoft 'Data-Tier Application Framework').
One added sentence would be sufficient.

Or, if there is no relationship, remove 'DacFx' from the metadata 'title:'.

I discussed this with SStein (SteveStein).
Thanks.  GeneMi (MightyPen in GitHub).  2019-03-27
-->

## <a name="186-sqlpackage"></a>18.6 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2143544)|2020 年 9 月 18 日|18.6|15.0.4897.1|
|macOS .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2143659)|2020 年 9 月 18 日| 18.6|15.0.4897.1|
|Linux .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2143497)|2020 年 9 月 18 日| 18.6|15.0.4897.1|
|Windows .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2143496)|2020 年 9 月 18 日| 18.6|15.0.4897.1|

### <a name="features"></a>功能
| Feature | 详细信息 |
| :------ | :------ |
| 平台 | 已将 sqlpackage .NET Core 版本更新为 .NET Core 3.1 |
| Always Encrypted | 添加了对 SQL Server 2019 的安全 enclave 导入和导出的支持 |
| 部署 | 添加了支持，以在从 Azure SQL 数据库导出时忽略已启用变更数据捕获的表 |
| 部署 | 添加了对 Azure SQL 数据库中 OPTIMIZE_FOR_SEQUENTIAL_KEY 索引选项的支持 |
| 部署 | 添加了对 Azure Synapse Analytics 的标识列的支持 | 
| 帮助 | 在帮助 (?) 中输出 sqlpackage 版本，并支持 version 参数 | 

### <a name="fixes"></a>修复项
| Feature | 详细信息 |
| :------ | :------ | 
| 部署 | 修复了以非 sysadmin 用户身份将 Azure SQL 托管实例作为目标时生成的错误部署脚本  | 
| 部署 | 修复了在运行脚本操作时加载部署的问题 | 
| 帮助 | 当运行时间超过 1 天时，输出 sqlpackage 中的正确运行时间 | 
| 部署 | 修复了在 .NET Core 中部署时的 dacpac 注册问题 | 
| 部署 | 修复了在 .NET Core 中处理 accessToken (at) 参数时的 sqlpackage 问题 | 
| 部署 | 允许将存储过程中的 ALTER TABLE 语句作为非顶级语句 | 
| 部署 | 修复了在具体化视图中验证 Azure Synapse Analytics 时不区分大小写的问题 | 

### <a name="known-issues"></a>已知问题
| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 目前尚不支持 Azure Synapse Analytics 工作负荷管理功能（工作负荷组和工作负荷分类器） | 

## <a name="1851-sqlpackage"></a>18.5.1 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2134206)|2020 年 6 月 24 日|18.5.1|15.0.4826.1|
|macOS .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2134312)|2020 年 6 月 24 日| 18.5.1|15.0.4826.1|
|Linux .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2134311)|2020 年 6 月 24 日| 18.5.1|15.0.4826.1|
|Windows .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2134310)|2020 年 6 月 24 日| 18.5.1|15.0.4826.1|

### <a name="fixes"></a>修复项
| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 修复了 18.5 中引入的倒退，在将 dacpac（具有使用外部登录名的用户）导入或部署到本地时出现“"type" 附近的语法不正确”错误 | 

## <a name="185-sqlpackage"></a>18.5 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2128142)|2020 年 4 月 28 日|18.5|15.0.4769.1|
|macOS .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2128145)|2020 年 4 月 28 日| 18.5|15.0.4769.1|
|Linux .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2128144)|2020 年 4 月 28 日| 18.5|15.0.4769.1|
|Windows .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2128143)|2020 年 4 月 28 日| 18.5|15.0.4769.1|

### <a name="features"></a>功能
| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 现在 SQL Server 2008 及更高版本、Azure SQL 数据库和 Azure Synapse Analytics 支持数据敏感度分类 |
| 部署 | 针对表约束添加 Azure Synapse Analytics 支持 |
| 部署 | 针对有序聚集列存储索引添加 Azure Synapse Analytics 支持 |
| 部署 | 为外部数据源（适用于 Oracle、Teradata、MongoDB/CosmosDB、ODBC、大数据群集）和外部表（适用于 SQL Server 2019 大数据群集）添加支持 |
| 部署 | 添加 SQL Database Edge 实例作为支持的版本 |
| 部署 | 支持格式为“\<server>.\<dnszone>.database.windows.net”的托管实例服务器名称 |
| 部署 | 在 Azure Synapse Analytics 中添加对复制命令的支持 |
| 部署 | 在发布过程中添加部署选项“IgnoreTablePartitionOptions”，以便在 Azure Synapse Analytics 的分区功能发生更改时避免重新创建表 |
| .NET Core | 在 sqlpackage 的 .NET Core 版本中添加对 Microsoft.Data.SqlClient 的支持 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项
| Fix | 详细信息 |
| :-- | :------ |
| 部署 | 修复将 json 路径分析为表达式的问题 |
| 部署 | 修复为 AlterAnyDatabaseScopedConfiguration 和 AlterAnySensitivityClassification 权限生成 GRANT 语句的问题 |
| 部署 | 修复无法识别外部脚本权限的问题 |
| 部署 | 修复内联属性 - 属性的隐式添加不应显示差别，但显式提及应通过脚本显示 |
| 部署 | 解决了更改具体化视图 (MV) 引用的表会导致生成改变视图语句（Azure Synapse Analytics 的 MV 不支持该语句）的问题 |
| 部署 | 修复在将列添加到包含 Azure Synapse Analytics 数据的表时发布失败的问题 |
| 部署 | 修复在为 Azure Synapse Analytics 更改分布列类型（数据丢失方案）时更新脚本应将数据移动到新表的问题 |
| ScriptDom | 修复无法识别在内联索引之后定义的内联约束的 ScriptDom bug |
| ScriptDom | 修复 ScriptDom SYSTEM_TIME 在批处理语句中缺少右括号的问题 |
| Always Encrypted | 修复 #tmpErrors 表在 sqlpackage 重新连接并且临时表已消失时无法删除（因为临时表会在连接终止时消失）的问题 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题
| Feature | 详细信息 |
| :------ | :------ |
| 部署 |  18.5 中引入了一个倒退，在将 dacpac（具有使用外部登录名的用户）导入或部署到本地时出现“"type" 附近的语法不正确”错误。 使用 sqlpackag 18.4 可暂时避开此问题，将在下一个 sqlpackag 版本中进行修复。 | 
| .NET Core | 由于 Microsoft.Data.SqlClient 中的这个[已知问题](https://github.com/dotnet/SqlClient/issues/559)，导入具有敏感度分类的 bacpacs 失败，并显示“重大内部连接错误”消息。 此问题将在下一个 sqlpackage 版本中修复。 |
| &nbsp; | &nbsp; |

## <a name="1841-sqlpackage"></a>18.4.1 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2113703)|2019 年 12 月 13 日|18.4.1|15.0.4630.1|
|macOS .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2113705)|2019 年 12 月 13 日| 18.4.1|15.0.4630.1|
|Linux .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2113331)|2019 年 12 月 13 日| 18.4.1|15.0.4630.1|
|Windows .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2113704)|2019 年 12 月 13 日| 18.4.1|15.0.4630.1|

### <a name="fixes"></a>修复项
| Fix | 详细信息 |
| :-- | :------ |
| ScriptDom |  ScriptDom 分析回归在 18.3.1 中引入，其中“重命名”被错误地视为顶级令牌，导致分析失败。
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题 

| Feature | 详细信息 |
| :------ | :------ |
| 部署 |  在 18.4.1 中引入了一个退行性问题，导致“对象引用未设置为对象的实例”。 作为具有外部登录名的用户部署 dacpac 或导入 bacpac 时出错。 使用 sqlpackag 18.4 可暂时避开此问题，将在下一个 sqlpackag 版本中进行修复。 | 
| &nbsp; | &nbsp; |

## <a name="184-sqlpackage"></a>18.4 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2108813)|2019 年 10 月 29 日|18.4|15.0.4573.2|
|macOS .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2108815)|2019 年 10 月 29 日| 18.4|15.0.4573.2|
|Linux .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2108814)|2019 年 10 月 29 日| 18.4|15.0.4573.2|
|Windows .NET Core |[zip 文件](https://go.microsoft.com/fwlink/?linkid=2109019)|2019 年 10 月 29 日| 18.4|15.0.4573.2|

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 添加对部署到 Azure Synapse Analytics (GA) 的支持。 | 
| 平台 | 适用于 macOS、Linux 和 Windows 的 sqlpackage .NET Core GA。 | 
| 安全性 | 删除 SHA1 代码签名。 |
| 部署 | 添加对新 Azure 数据库版本的支持：GeneralPurpose、BusinessCritical、超大规模 |
| 部署 | 添加对 Azure Active Directory 用户和组的托管实例支持。 |
| 部署 | 支持 .NET Core 上的 sqlpackage 的 /AccessToken 参数。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题 

| Feature | 详细信息 |
| :------ | :------ |
| ScriptDom |  ScriptDom 分析回归在 18.3.1 中引入，其中“重命名”被错误地视为顶级令牌，导致分析失败。 此问题将在下一个 sqlpackage 版本中修复。 | 
| &nbsp; | &nbsp; |

### <a name="known-issues-for-net-core"></a>.NET Core 的已知问题

| Feature | 详细信息 |
| :------ | :------ |
| 导入 |  对于压缩文件大小超过 4 GB 的 .bacpac 文件，你可能需要使用 sqlpackage 的 .NET Core 版本来执行导入。  此行为由 .NET Core 生成 zip 标头文件的方式导致，虽然 zip 标头文件是有效的，但无法由 sqlpackage 的 .NET Full Framework 版本读取。 | 
| 部署 | 不支持参数 /p:Storage=File。 .NET Core 仅支持 Memory。 | 
| Always Encrypted | sqlpackage .NET Core 不支持 Always Encrypted 列。 | 
| 安全性 | sqlpackage .NET Core 不支持多重身份验证的 /ua 参数。 | 
| 部署 | 不支持使用 json 数据序列化的较旧 V2 .dacpac 和 .bacpac 文件。 |
| &nbsp; | &nbsp; |

## <a name="1831-sqlpackage"></a>18.3.1 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2102893)|2019 年 9 月 13 日|18.3.1|15.0.4538.1|
|macOS .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2102894)|2019 年 9 月 13 日| 18.3.1|15.0.4538.1|
|Linux .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2102978)|2019 年 9 月 13 日| 18.3.1|15.0.4538.1|
|Windows .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2102979)|2019 年 9 月 13 日| 18.13.1|15.0.4538.1|

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 添加对部署到 Azure Synapse Analytics（预览版）的支持。 | 
| 部署 | 将 /p:DatabaseLockTimeout=(INT32 '60') 参数添加到 sqlpackage。 | 
| 部署 | 将 /p:LongRunningCommandTimeout=(INT32) 参数添加到 sqlpackage。 |
| 导出/提取 | 将 /p:TempDirectoryForTableData=(STRING) 参数添加到 sqlpackage。 |
| 部署 | 允许从其他位置加载部署参与者。 将从以下位置加载部署参与者：与部署目标 .dacpac 的相同目录、与 sqlpackage 二进制文件相对应的扩展目录，以及添加到 sqlpackage 的 /p:AdditionalDeploymentContributorPaths=(STRING) 参数（可在其中指定其他目录位置）。 |
| 部署 | 添加对 OPTIMIZE_FOR_SEQUENTIAL_KEY 的支持。 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 部署 | 修复了忽略自动索引使其不会在部署时被删除的问题。 | 
| Always Encrypted | 修复了处理 Always Encrypted varchar 列的问题。 | 
| 生成/部署 | 修复了解析 xml 列集的 nodes() 方法的问题。| 
| ScriptDom | 修复了“URL”字符串被解释为顶级令牌的其他情况。 | 
| 图形 | 修复了约束中伪列引用的生成 TSQL 的问题。  | 
| 导出 | 生成满足复杂性要求的随机密码。 | 
| 部署 | 修复了在检索约束时遵循命令超时的问题。 | 
| .NET Core（预览版） | 修复了诊断日志记录到文件的问题。 | 
| .NET Core（预览版） | 使用流式传输来导出表数据，以支持大型表。 | 
| &nbsp; | &nbsp; |

## <a name="182-sqlpackage"></a>18.2 sqlpackage

|平台|下载|发布日期|版本|构建
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2087429)|2019 年 4 月 15 日|18.2|15.0.4384.2|
|macOS .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2087247)|2019 年 4 月 15 日 | 18.2 |15.0.4384.2|
|Linux .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2087431)|2019 年 4 月 15 日 | 18.2 |15.0.4384.2|

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 图形 | 添加了对边缘约束和边缘约束子句的图形表支持。 |
| 部署 | 启用了模型验证规则以支持 SQL Server 2016 及更高版本的索引键的 32 个列。 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 部署 | 由于使用的查询提示不受支持，因此修复了对 SQL Server 2016 RTM 数据库进行的反向工程。 |
| 部署 | 修复了 create filegroup 语句之前出现的 auto close alter 语句的部署顺序。 |
| ScriptDom | 修复了 ScriptDom 分析回归，其中“URL”字符串被解释为顶级令牌。 |
| 部署 | 修复了分析 alter table add index 语句时出现的空引用异常。 | 
| 架构比较 | 修复了始终显示为不同的可为空的持久化计算列的架构比较。|
| &nbsp; | &nbsp; |

## <a name="181-sqlpackage"></a>18.1 sqlpackage

发行日期：&nbsp; 2019 年 2 月 1 日  
内部版本：&nbsp; 15.0.4316.1  
预览版。

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 添加了对 UTF8 排序规则的支持。 |
| 部署 | 对索引视图启用了非聚集列存储索引。 |
| 平台 | 已移动到 .NET Core 2.2。 | 
| 架构比较 | 将内存支持的存储用于在 .NET Core 上进行架构比较。 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 性能 | 修复了性能以使用旧版基数估计器进行反向工程查询。 | 
| 性能 | 修复了生成脚本时出现的重大架构比较性能问题。 | 
| 架构比较 | 修复了架构偏差检测逻辑以忽略特定扩展事件 (xevent) 会话。 |
| 图形 | 修复了图形表的导入顺序。 | 
| 导出 | 修复了导出具有对象权限的外部表的问题。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

此版本包括面向 .NET Core 2.2 的 sqlpackage 跨平台预览版。 sqlpackage 可以在 macOS 和 Linux 上运行。

| 已知问题 | 详细信息 |
| :---------- | :------ |
| 部署 | 对于 .NET Core，不支持生成和部署参与者。 | 
| 部署 | 对于 .NET Core，不支持使用 json 数据序列化的较旧 .dacpac 和 .bacpac 文件。 | 
| 部署 | 对于 .NET Core，由于区分大小写的文件系统出现问题，因此可能无法解析引用的 .dacpacs（例如 master.dacpac）。 | 一种解决方法是大写引用文件的名称（例如 MASTER.BACPAC）。 |
| &nbsp; | &nbsp; |

## <a name="180-sqlpackage"></a>18.0 sqlpackage

发行日期：&nbsp; 2018 年 10 月 24 日  
内部版本：&nbsp; 15.0.4200.1

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 部署 | 添加了对数据库兼容级别 150 的支持。 | 
| 部署 | 添加了对托管实例的支持。 | 
| 性能 | 添加了 MaxParallelism 命令行参数以指定数据库操作的并行度。 | 
| 安全性 | 添加了 AccessToken 命令行参数以在连接到 SQL Server 时指定身份验证令牌。 | 
| 导入 | 添加了对流式传输 BLOB/CLOB 数据类型以进行导入的支持。 | 
| 部署 | 添加了对标量 UDF“INLINE”选项的支持。 | 
| 图形 | 添加了对图形表“MERGE”语法的支持。 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 图形 | 修复了图形表的未解析伪列。 |
| 部署 | 修复了使用内存优化表时创建具有内存优化文件组的数据库的问题。 |
| 部署 | 修复了将扩展属性包括在外部表中的问题。 |
| &nbsp; | &nbsp; |

## <a name="178-sqlpackage"></a>17.8 sqlpackage

发行日期：&nbsp; 2018 年 6 月 22 日  
内部版本：&nbsp; 14.0.4079.2

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 诊断 | 改进了连接失败的错误消息，包括 SqlClient 异常消息。 |
| 部署 | 支持对单分区索引进行索引压缩以便导入/导出。 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 部署 | 修复了 SQL 2017 及更高版本的 XML 列集的反向工程问题。 | 
| 部署 | 修复了对 Azure SQL 数据库忽略数据库兼容性级别 140 脚本编写的问题。 |
| &nbsp; | &nbsp; |

## <a name="1741-sqlpackage"></a>17.4.1 sqlpackage

发行日期：&nbsp; 2018 年 1 月 25 日  
内部版本：&nbsp; 14.0.3917.1

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 导入/导出 | 添加了 ThreadMaxStackSize 命令行参数以分析具有大量嵌套语句的 Transact-SQL。 |
| 部署 | 数据库目录排序规则支持。 | 
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 导入 | 将 Azure SQL 数据库 .bacpac 导入到本地实例时，修复了由于“此版本的 SQL Server 不支持没有密码的数据库主密钥”  而出现的错误。 |
| 图形 | 修复了图形表的未解析伪列错误。 |
| 架构比较 | 修复了用于比较架构的 SQL 身份验证问题。 | 
| &nbsp; | &nbsp; |

## <a name="1740-sqlpackage"></a>17.4.0 sqlpackage

发行日期：&nbsp; 2017 年 12 月 12 日  
内部版本：&nbsp; 14.0.3881.1

### <a name="features"></a>功能

| Feature | 详细信息 |
| :------ | :------ |
| 部署 |  添加了对 SQL 2017+ 和 Azure SQL 数据库上的时态保留策略  的支持。 | 
| 诊断 | 添加了 /DiagnosticsFile:"C:\Temp\sqlpackage.log" 命令行参数以指定用于保存诊断信息的文件路径。 | 
| 诊断 | 添加了 /Diagnostics 命令行参数以将诊断信息记录到控制台。 |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>修复项

| Fix | 详细信息 |
| :-- | :------ |
| 部署 | 遇到无法识别的数据库兼容性级别时不会进行阻止。 相反，将假定最新的 Azure SQL 数据库或本地平台。 |
| &nbsp; | &nbsp; |
