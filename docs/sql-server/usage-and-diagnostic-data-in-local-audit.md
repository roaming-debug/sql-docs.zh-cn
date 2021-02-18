---
title: 使用情况和诊断数据收集的本地审核
description: 了解由 SQL Server 用于收集使用情况和诊断数据并将其发送给 Microsoft 的本地审核。
ms.custom: seo-lt-2019
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Local Audit
ms.assetid: a0665916-7789-4f94-9086-879275802cf3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6333f571ea271420b4044f7e3cf562d2f1edcc15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353544"
---
# <a name="local-audit-for-sql-server-usage-and-diagnostic-data-collection-ceip"></a>SQL Server 使用情况和诊断数据收集的本地审核 (CEIP)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

## <a name="introduction"></a>介绍

Microsoft SQL Server 包含了一些支持 Internet 的功能，可以收集和发送关于计算机或设备的信息。 这被称为“标准计算机信息”  。 [SQL Server 使用情况和诊断数据收集](usage-and-diagnostic-data-configuration-for-sql-server.md)的本地审核组件将服务收集的数据（表示将发送给 Microsoft 的数据（日志））写入指定文件夹。 本地审核的用途是，便于客户出于合规性、监管或隐私验证原因，查看 Microsoft 使用此功能收集的所有数据。  

对于 SQL Server 2016 CU2 和 CU3，本地审核可在实例级别针对 SQL Server 数据库引擎和 Analysis Services (SSAS) 进行配置。 对于 SQL Server 2016 CU4 和 2016 SP1 及更高版本，SQL Server Integration Services (SSIS) 也启用了本地审核。 在安装过程中安装的其他 SQL Server 组件，以及在安装后下载或安装的 SQL Server 工具没有使用情况和诊断数据收集的本地审核功能。

## <a name="remarks"></a>备注

 - 不支持删除或禁用 SQL CEIP 服务。 
 - 不支持从群集组中删除 SQL CEIP 资源。 

若要选择退出数据收集，请参阅[启用或禁用本地审核](#turning-local-audit-on-or-off)

## <a name="prerequisites"></a>先决条件 

以下是在每个 SQL Server 实例上启用本地审核的先决条件： 

1. 实例修补为 SQL Server 2016 RTM CU2 或更高版本。 对于 Integration Services，实例被修补为 SQL 2016 RTM CU4、SQL 2016 SP1 及更高版本。

1. 用户必须是系统管理员，或是具有添加和修改注册表项、创建文件夹、管理文件夹安全性以及停止/启动 Windows 服务的访问权限的角色。  

## <a name="pre-configuration-steps-prior-to-turning-on-local-audit"></a>启用本地审核前的预配置步骤

启用本地审核前，系统管理员需要：

1. 了解 SQL Server 实例名称和 SQL Server CEIP 服务登录帐户。 

1. 为本地审核文件配置新文件夹。

1. 向 SQL Server CEIP 服务登录帐户授予权限。

1. 创建注册表项设置以配置本地审核目标目录。 


### <a name="get-the-sql-server-ceip-service-logon-account"></a>获取 SQL Server CEIP 服务登录帐户 

若要获取 SQL Server CEIP 服务登录帐户，请按以下步骤操作
 
1. 启动“服务”控制台  。 若要执行此操作，请选择键盘上的“Windows 徽标键+R”以打开“运行”对话框。 然后在文本字段中键入 services.msc 并选择“确定”，以启动“服务”控制台。  

2. 导航到相应的服务。 例如，对于数据库引擎，找到 SQL Server CEIP 服务 (Your-Instance-Name)   。 对于 Analysis Services，找到 SQL Server Analysis Services CEIP (Your-Instance-Name)   。 对于 Integration Services，找到 SQL Server Integration Services CEIP 服务  。

3. 右键单击服务，然后选择“属性”  。 

4. 选择“登录”选项卡  。登录帐户会在“此帐户”  中列出。 

### <a name="configure-a-new-folder-for-the-local-audit-files"></a>为本地审核文件配置新文件夹。    

新建文件夹（本地审核目录），以供本地审核将日志写入其中。 例如，数据库引擎默认实例的本地审核目录的完整路径是：*C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* . 
 
  >[!NOTE] 
  >在 SQL Server 安装路径之外配置本地审核目录路径，以免审核功能和修补导致 SQL Server 出现潜在问题。

|设计决策|建议|  
|-----------------|----------|  
|空间可用性 |对于大约具有 10 个数据库的中等工作负荷，计划每个实例每个数据库使用大约 2 MB 磁盘空间。|  
|单独的目录 | 为每个实例创建一个目录。 例如，对名为 `MSSQLSERVER` 的 SQL Server 实例使用 C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\。 这简化了文件管理。
|单独的文件夹 |对每个服务使用特定文件夹。 例如对于给定实例名称，对数据库引擎使用一个文件夹。 如果有 Analysis Services 的实例使用相同实例名称，则为 Analysis Services 创建单独的文件夹。 如果将数据库引擎实例和 Analysis Services 实例配置到相同的文件夹，会导致本地审核将这两个实例中的所有数据都写入同一日志文件。| 
|向 SQL Server CEIP 服务登录帐户授予权限|向 SQL Server CEIP 服务登录帐户授予“列出文件夹内容”、“读取”和“写入”访问权限|


### <a name="grant-permissions-to-the-sql-server-ceip-service-logon-account"></a>向 SQL Server CEIP 服务登录帐户授予权限
  
1. 在“文件资源管理器”中，导航到新文件夹所在的位置。

1. 右键单击新文件夹，然后选择“属性”。 

1. 在“安全性”选项卡上，选择“编辑”以管理权限。

1. 选择“添加”，并键入 SQL Server CEIP 服务的凭据。 例如，`NT Service\SQLTELEMETRY`。

1. 选择“检查名称”以验证所提供的名称，然后选择“确定”。

1. 在“权限”对话框中，选择 SQL Server CEIP 服务登录帐户，并依次选择“列出文件夹内容”、“读取”和“写入”。

1. 选择“确定”以立即应用权限更改。 
  
### <a name="create-a-registry-key-setting-to-configure-local-audit-target-directory"></a>创建注册表项设置以配置本地审核目标目录

1. 启动 regedit。

1. 导航到相应的 CPE 路径：

   | 版本 | 数据库引擎 - 注册表项 |
   | :------ | :----------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL13.Your-Instance-Name\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL14.Your-Instance-Name\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL15.Your-Instance-Name\\CPE |
   | &nbsp; | &nbsp; |

   | 版本 | Analysis Services - 注册表项 |
   | :------ | :------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS13.Your-Instance-Name\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS14.Your-Instance-Name\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS15.Your-Instance-Name\\CPE |  
   | &nbsp; | &nbsp; |

   | 版本 | Integration Services - 注册表项 |
   | :------ | :---------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\130 |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\140 |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\150 |
   | &nbsp; | &nbsp; |

1. 右键单击 CPE 路径并选择“新建”。 选择“字符串值”。

1. 对新的注册表项 `UserRequestedLocalAuditDirectory` 进行命名。 
 
## <a name="turning-local-audit-on-or-off"></a>启用或禁用本地审核

完成预配置步骤后，便能启用本地审核。 为此，请使用系统管理员帐户，或有权将注册表项修改为启用或禁用本地审核（具体步骤如下）的类似角色。 

1. 启动 **regedit**。  

1. 导航到相应的 CPE [路径](#create-a-registry-key-setting-to-configure-local-audit-target-directory)。 

1. 右键单击“UserRequestedLocalAuditDirectory”，然后选择“修改”。 

1. 若要启用本地审核，请键入本地审核路径（例如，“C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\”）。
 
    若要禁用本地审核，请清空“UserRequestedLocalAuditDirectory”中的值。

1. 关闭 **regedit**。 

如果服务已在运行，SQL Server CEIP 应该会立即识别出本地审核设置。 若要启动 SQL Server CEIP 服务，系统管理员或具有启动或停止 Windows 服务的访问权限的人员可以执行以下步骤： 

1. 启动“服务”控制台。 若要执行此操作，请选择键盘上的“Windows 徽标键+R”以打开“运行”对话框。 然后在文本字段中键入 services.msc 并选择“确定”，以启动“服务”控制台。  

1. 导航到相应的服务。 

    - 对于数据库引擎，使用 SQL Server CEIP 服务 (Your-Instance-Name)****。     
    - 对于 Analysis Services，使用 SQL Server Analysis Services CEIP (Your-Instance-Name)****。
    - 对于 Integration Services， 
        - 对于 SQL 2016，使用 SQL Server Integration Services CEIP 服务 13.0。
        - 对于 SQL 2017，使用 SQL Server Integration Services CEIP 服务 14.0。
    - 对于 SQL 2019，使用 SQL Server Integration Services CEIP 服务 15.0。

1. 右键单击服务，然后选择“重新启动”。 

1. 验证服务的状态是否为“正在运行”。 

本地审核每天生成一个日志文件。 日志文件会采用 `<YYYY-MM-DD>.json` 的形式。 例如 2016-07-12.json。 如果指定目录中已有当天的文件，本地审核会向此文件追加信息。 否则，它会为该天创建新文件。 

  >[!NOTE]
  > 启用本地审核后，首次写入日志文件可能最多需要 5 分钟才能完成。 

## <a name="maintenance"></a>维护 

1. 若要限制本地审核写入的文件所占用的磁盘空间，请设置策略或定期作业来清理本地审核目录，以删除不需要的旧文件。  

2. 保护本地审核目录路径，只有相应人员才能访问它。 请注意，日志文件包含[如何配置 SQL Server 2016 以向 Microsoft 发送反馈](https://support.microsoft.com/kb/3153756)中概述的信息。 针对此文件的访问权限会阻止组织的大多数成员读取它。  

## <a name="data-dictionary-of-local-audit-output-data-structure"></a>本地审核输出数据结构的数据字典 

- 本地审核日志文件采用 JSON 格式，其中包含一组对象（行），表示在 emitTime 发送回 Microsoft 的数据点。
- 每行都遵循由 **schemaVersion** 标识的特定架构。
- 每行都是标识为 **sessionID** 的 SQLCEIP 服务会话的输出。
- 这些行按 **sequence** 标识的顺序发出。
- 每个数据点行都包含 queryIdentifier 的输出，可以是与某种类型的跟踪（标识为 traceName）相关的 T-SQL 查询、XE 会话或消息 。
- **queryIdentifiers** 会进行分组并随同及 **querySetVersion** 一起进行版本管理。
- data 包含对应查询执行（花费时间为 queryTimeInTicks）的输出 。
- T-SQL 查询的 **queryIdentifiers** 具有查询中存储的 T-SQL 查询定义。

| 逻辑本地审核信息层次结构 | 相关列 |
| ------ | -------|
| 标头 | emitTime、schemaVersion 
| 计算机 | operatingSystem 
| 实例 | instanceUniqueID、correlationID、clientVersion 
| 会话 | sessionID、traceName 
| 查询 | sequence、querySetVersion、queryIdentifier、query、queryTimeInTicks 
| 数据 |  data 

### <a name="namevalue-pairs-definition-and-examples"></a>名称/值对定义和示例 

下面各列表示本地审核文件输出的顺序。 采用 SHA 256 的单向哈希常常是下面一些列的匿名值。  

| 名称 | 说明 | 示例值
|-------|--------| ----------|
|instanceUniqueID| 匿名实例标识符 | 888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD 
|schemaVersion| SQLCEIP 的架构版本 |  3 
|emitTime |数据点发出时间 (UTC) | 2016-09-08T17:20:22.1124269Z 
|sessionID | 用于处理 SQLCEIP 服务的会话标识符 | 89decf9a-ad11-485c-94a7-fefb3a02ed86 
|correlationId | 其他标识符的占位符 | 0 
|sequence | 在会话中发送的数据点的序列号 | 15 
|clientVersion | SQL Server 实例版本 | 13.0.2161.3 ((SQL16_RTM_QFE-CU).160907-1223) 
|operatingSystem | 安装 SQL Server 实例的操作系统版本 | Microsoft Windows Server 2012 R2 Datacenter 
|querySetVersion | 一组查询定义的版本 | 1.0.0.0 
|traceName | 跟踪的类别：（SQLServerXeQueries、SQLServerPeriodicQueries、SQLServerOneSettingsException） | SQLServerPeriodicQueries 
|queryIdentifier | 查询的标识符 | SQLServerProperties.002 
|data   | 以 T-SQL 查询、XE 会话或应用程序输出的形式，对 queryIdentifier 收集的信息的输出 |  [{"Collation": "SQL_Latin1_General_CP1_CI_AS","SqlFTinstalled": "0" "SqlIntSec": "1","IsSingleUser": "0","SqlFilestreamMode": "0","SqlPbInstalled": "0","SqlPbNodeRole": "","SqlVersionMajor": "13","SqlVersionMinor": "0","SqlVersionBuild": "2161","ProductBuildType": "","ProductLevel": "RTM","ProductUpdateLevel": "CU2","ProductUpdateReference": "KB3182270","ProductRevision": "3","SQLEditionId": "-1534726760","IsClustered": "0","IsHadrEnabled": "0","SqlAdvAInstalled": "0","PacketReceived": "1210","Version": "Microsoft SQL Server 2016 (RTM-CU2) (KB3182270) - 13.0.2161.3 (X64) \n\tSep  7 2016 14:24:16 \n\tCopyright (c) Microsoft Corporation\n\tStandard Edition (64-bit) on Windows Server 2012 R2 Datacenter 6.3 \u003cX64\u003e (Build 9600: ) (Hypervisor)\n"}],
|查询| 如果适用，是与生成数据的 queryIdentifier 相关的 T-SQL 查询定义。        此组件不会由 SQL Server CEIP 服务上传。 它包含在本地审核中，仅供客户参考。| SELECT\n      SERVERPROPERTY(\u0027Collation\u0027) AS [Collation],\n      SERVERPROPERTY(\u0027IsFullTextInstalled\u0027) AS [SqlFTinstalled],\n      SERVERPROPERTY(\u0027IsIntegratedSecurityOnly\u0027) AS [SqlIntSec],\n      SERVERPROPERTY(\u0027IsSingleUser\u0027) AS [IsSingleUser],\n      SERVERPROPERTY (\u0027FileStreamEffectiveLevel\u0027) AS [SqlFilestreamMode],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbInstalled],\n      SERVERPROPERTY(\u0027PolyBaseRole\u0027) AS [SqlPbNodeRole],\n      SERVERPROPERTY(\u0027ProductMajorVersion\u0027) AS [SqlVersionMajor],\n      SERVERPROPERTY(\u0027ProductMinorVersion\u0027) AS [SqlVersionMinor],\n      SERVERPROPERTY(\u0027ProductBuild\u0027) AS [SqlVersionBuild],\n      SERVERPROPERTY(\u0027ProductBuildType\u0027) AS ProductBuildType,\n      SERVERPROPERTY(\u0027ProductLevel\u0027) AS ProductLevel,\n      SERVERPROPERTY(\u0027ProductUpdateLevel\u0027) AS ProductUpdateLevel,\n      SERVERPROPERTY(\u0027ProductUpdateReference\u0027) AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)),CHARINDEX(\u0027.\u0027, REVERSE(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY(\u0027EditionID\u0027) AS SQLEditionId,\n      SERVERPROPERTY(\u0027IsClustered\u0027) AS IsClustered,\n      SERVERPROPERTY(\u0027IsHadrEnabled\u0027) AS IsHadrEnabled,\n      SERVERPROPERTY(\u0027IsAdvancedAnalyticsInstalled\u0027) AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version
|queryTimeInTicks | 具有以下跟踪类别的查询执行所需的持续时间：（SQLServerXeQueries、SQLServerPeriodicQueries） |  0 
 
### <a name="trace-categories"></a>跟踪类别 
我们当前收集以下跟踪类别： 

- **SQLServerXeQueries**：包含通过扩展事件会话收集的数据点。
- **SQLServerPeriodicQueries**：包含通过在 SQL Server 实例中执行的定期查询收集的数据点。
- **SQLServerPerDBPeriodicQueries**：包含通过在 SQL Server 实例中对最多 30 个数据块执行的定期查询收集的数据点。
- **SQLServerOneSettingsException**：包含与更新架构和/或查询集相关的异常消息。
- **DigitalProductID**：包含用于聚合 SQL Server 实例的匿名 (SHA-256) 哈希数字产品 ID 的数据点。 

### <a name="local-audit-file-examples"></a>本地审核文件示例



下面摘录了本地审核的 JSON 文件输出。

```JSON
[
  {
    "instanceUniqueId": "888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:27:59.7031518Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 18,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "SQLServerProperties.002",
    "data": [
      {
        "Collation": "SQL_Latin1_General_CP1_CI_AS",
        "SqlFTinstalled": "0",
        "SqlIntSec": "1",
        "IsSingleUser": "0",
        "SqlFilestreamMode": "2",
        "SqlPbInstalled": "1",
        "SqlPbNodeRole": "Head",
        "SqlVersionMajor": "14",
        "SqlVersionMinor": "0",
        "SqlVersionBuild": "3025",
        "ProductBuildType": "",
        "ProductLevel": "RTM",
        "ProductUpdateLevel": "CU6",
        "ProductUpdateReference": "KB4101464",
        "ProductRevision": "34",
        "SQLEditionId": "1872460670",
        "IsClustered": "0",
        "IsHadrEnabled": "0",
        "SqlAdvAInstalled": "1",
        "PacketReceived": "422",
        "Version": "Microsoft SQL Server 2017 (RTM-CU6) (KB4101464) - 14.0.3025.34 (X64) \n\tApr  9 2018 18:00:41 \n\tCopyright (C) 2017 Microsoft Corporation\n\tEnterprise Edition: Core-based Licensing (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 16299: )\n"
      }
    ],
    "query": "SELECT\n      SERVERPROPERTY('Collation') AS [Collation],\n      SERVERPROPERTY('IsFullTextInstalled') AS [SqlFTinstalled],\n      SERVERPROPERTY('IsIntegratedSecurityOnly') AS [SqlIntSec],\n      SERVERPROPERTY('IsSingleUser') AS [IsSingleUser],\n      SERVERPROPERTY ('FileStreamEffectiveLevel') AS [SqlFilestreamMode],\n      SERVERPROPERTY('IsPolyBaseInstalled') AS [SqlPbInstalled],\n      SERVERPROPERTY('PolyBaseRole') AS [SqlPbNodeRole],\n      SERVERPROPERTY('ProductMajorVersion') AS [SqlVersionMajor],\n      SERVERPROPERTY('ProductMinorVersion') AS [SqlVersionMinor],\n      SERVERPROPERTY('ProductBuild') AS [SqlVersionBuild],\n      SERVERPROPERTY('ProductBuildType') AS ProductBuildType,\n      SERVERPROPERTY('ProductLevel') AS ProductLevel,\n      SERVERPROPERTY('ProductUpdateLevel') AS ProductUpdateLevel,\n      SERVERPROPERTY('ProductUpdateReference') AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)),CHARINDEX('.', REVERSE(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY('EditionID') AS SQLEditionId,\n      SERVERPROPERTY('IsClustered') AS IsClustered,\n      SERVERPROPERTY('IsHadrEnabled') AS IsHadrEnabled,\n      SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version",
    "queryTimeInTicks": 0
  },
  {
    "instanceUniqueId": "8884F770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:28:00.9025999Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 23,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "OsSysInfo.003",
    "data": [
      {
        "LogicalCPUCount": "8",
        "HyperthreadRatio": "8",
        "PhysicalMemoryMB": "32710.902343",
        "SQLServerStartTime": "05/04/2018 08:22:30",
        "AffinityTypeDesc": "AUTO",
        "VirtualMachineType": "0",
        "SocketCount": "1",
        "CoresPerSocket": "4",
        "NumaNodeCount": "1",
        "ContainerType": "0",
        "ContainerDescription": "NONE"
      }
    ],
    "query": "SELECT\n      cpu_count AS LogicalCPUCount,\n      hyperthread_ratio AS HyperthreadRatio,\n      physical_memory_kb/1024.0 AS PhysicalMemoryMB,\n      sqlserver_start_time AS SQLServerStartTime,\n      affinity_type_desc AS AffinityTypeDesc,\n      virtual_machine_type AS VirtualMachineType,\n      socket_count as SocketCount,\n      cores_per_socket as CoresPerSocket,\n      numa_node_count as NumaNodeCount,\n      container_type as ContainerType,\n      container_type_desc as ContainerDescription\n      FROM sys.dm_os_sys_info WITH(nolock)",
    "queryTimeInTicks": 0
  }
]
```
## <a name="frequently-asked-questions"></a>常见问题

**DBA 如何读取本地审核日志文件？**
这些日志文件以 JSON 格式写入。 每行都是一个 JSON 对象，表示上传到 Microsoft 的一段使用情况/诊断数据。 字段名称应该浅显易懂。

**如果 DBA 禁用使用情况和诊断数据收集，会怎么样？**
不会写入任何本地审核文件。

**如果防火墙后面没有 Internet 连接/计算机，则会发生什么情况？**
SQL Server 使用情况和诊断数据不会发送给 Microsoft。 如果配置正确，则它仍会尝试写入本地审核日志。

**DBA 如何禁用本地审核？**
删除 UserRequestedLocalAuditDirectory 注册表项。

**谁可以读取本地审核日志文件？**
组织中有权访问本地审核目录的任何人。

**DBA 如何管理写入指定目录的日志文件？**
DBA 需要自行管理该目录中文件的清理，以避免占用过多磁盘空间。

**是否存在可以用于读取此 JSON 输出的客户端或工具？**
可以使用记事本、Visual Studio 或选择的任何 JSON 读取器来读取输出。
或者，可以按如下所示在 SQL Server 实例中读取 JSON 文件并分析数据。 有关如何在 SQL Server 中读取 JSON 文件的更多详细信息，请访问 [使用 OPENROWSET (BULK) 和 OPENJSON (Transact-SQL) 将 JSON 文件导入 SQL Server](/archive/blogs/sqlserverstorageengine/bulk-importing-json-files-into-sql-server)。

```Transact-SQL
DECLARE @JSONFile AS VARCHAR(MAX)

-- Read the JSON file into variable 
SELECT @JSONFile = BulkColumn 
FROM OPENROWSET (BULK 'C:\SQLCEIPAudit\MSSQLSERVER\2016-09-08.json', SINGLE_CLOB) MyFile 

-- Check if the JSON file has been read properly and if it's in a JSON format
SELECT 
    @JSONFile LocalAuditOutput, 
    ISJSON(@JSONFile) IsFileInJSONFormat

-- Get the query identifier, query and the data (output of the query)   
SELECT 
    sequence,
    queryIdentifier,
    query,
    data
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON)
-- Get specific details about the output of "DatabaseProperties.001" query  
SELECT 
    QueryIdentifier,
    DatabaseID,
    CompatibilityLevel,
    IsQueryStoreOn
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON) 
    CROSS APPLY OPENJSON(data) 
        WITH (   DatabaseID varchar(128) '$.database_id'
                ,CompatibilityLevel varchar(128) '$.compatibility_level'
                ,IsQueryStoreOn varchar(128) '$.QS'
             )
WHERE queryIdentifier = 'DatabaseProperties.001'
```

## <a name="see-also"></a>另请参阅
[SSMS 使用情况和诊断数据收集的本地审核](../ssms/sql-server-management-studio-telemetry-ssms.md)