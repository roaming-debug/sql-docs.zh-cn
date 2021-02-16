---
description: Attunity Oracle 更改数据捕获的已知错误和解决方法
title: Attunity Oracle 更改数据捕获的已知错误和解决方法 | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a898b268de4f339d26a6720c53356723ad56b285
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062114"
---
# <a name="known-errors-and-resolutions-with-change-data-capture-for-oracle-by-attunity"></a>Attunity Oracle 更改数据捕获的已知错误和解决方法
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]

本主题列出在 Oracle CDC 设计器配置工具中查看更改数据捕获 (CDC) 实例时遇到的主要问题和已知解决方法。 此工具是从 SQL Server 2012 开始随附的 Attunity Oracle 更改数据捕获的一部分。 

## <a name="bug-fixes"></a>Bug 修复
在花费过多时间进行故障排除前，请务必使用 Attunity Oracle CDC 的最新版本，以免出现以下已知问题：

### <a name="sql-server-2017"></a>SQL Server 2017

**5.0.0.111 版** 包含以下修复：
- Bug 修复 - 添加 Oracle 表时，Oracle CDC 设计器失败并显示“Incorrect syntax near the keyword 'KEY'”（关键字“KEY”附近的语法不正确）错误。 
- 改进 - 改进了对 RAC 的支持，其中包括重启 RAC 节点时的处理改进。 
- Bug 修复 - 由于从 v$log 请求 NEXT_CHANGE#，CDC 无法与 Oracle 10.2 配合使用。 


**5.0.0.93 版** 包含以下修复： 
- 添加 Oracle 表时，Attunity Oracle Microsoft CDC 设计器失败并显示“Incorrect syntax near the keyword 'KEY'”（关键字“KEY”附近的语法不正确）错误。 

 
### <a name="sql-server-2016"></a>SQL Server 2016

**4.0.107 版** 包含以下修复：
- Bug 修复 - 添加 Oracle 表时，Oracle CDC 设计器失败并显示“Incorrect syntax near the keyword 'KEY'”（关键字“KEY”附近的语法不正确）错误。
- 改进 - 改进了对 RAC 的支持，其中包括重启 RAC 节点时的处理改进。
- Bug 修复 - 由于从 v$log 请求 NEXT_CHANGE#，CDC 无法与 Oracle 10.2 配合使用。

**4.0.0.95 版** 包含以下修复： 
- Bug 修复 - 添加 Oracle 表时，Oracle CDC 设计器失败并显示“Incorrect syntax near the keyword 'KEY'”（关键字“KEY”附近的语法不正确）错误。

**4.0.0.88 版** 包含以下修复：
-  从 CDC 添加或删除表时，将删除 Attunity CDC 实例的“高级”选项中添加的属性。 
- 应用添加 __$command_id 列的 SQL 修复后，Attunity CDC 停止工作

### <a name="sql-server-2014"></a>SQL Server 2014 

**2.0.0.114 版** 包含以下修复：
- Bug 修复 - 添加 Oracle 表时，Oracle CDC 设计器失败并显示“Incorrect syntax near the keyword 'KEY'”（关键字“KEY”附近的语法不正确）错误。
- 改进 - 改进了对 RAC 的支持，其中包括重启 RAC 节点时的处理改进。
- Bug 修复 - 由于从 v$log 请求 NEXT_CHANGE#，CDC 无法与 Oracle 10.2 配合使用。

**2.0.0.92 版** 包含以下修复： 
- 从 CDC 添加或删除表时，将删除 Attunity CDC 实例的“高级”选项中添加的属性。 应用添加 __$command_id 列的 SQL 修复后，Attunity CDC 停止工作
- Oracle 表 cdc.table_name 的元数据验证失败。 列 column_name 索引超出范围。  以及下述问题：使用 Attunity Oracle CDC 时，Oracle CDC 服务显示中止状态
    - 已在 _SQL Server 2014 RTM 累积更新 1_ 中修复，如知识库文章 [2894025](https://support.microsoft.com/kb/2894025) 中所述。
- 部分更改会丢失，并且不会复制到 SQL Server 数据库中。 当一个表包含多个字符的大型二进制对象 (CLOB) 并且其中一个 CLOB 的值较大时，会发生此问题。 
    - 已在 _SQL Server 2014 SP1 累积更新 1_ 和 _SQL Server 2014 RTM 累积更新 8_ 中修复，如知识库文章 [3029096](https://support.microsoft.com/kb/3029096) 中所述。 
- 当 Oracle 表具有数据类型为 Long 的列时，Attunity Oracle 更改数据捕获将停止工作。
    - 已在 SQL Server 2014 SP1 的累积更新 5 和 SQL 2014 RTM 的累积更新 12 中修复，如知识库文章 [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d) 中所述 。

### <a name="sql-server-2012"></a>SQL Server 2012

**1.1.0.102 版** 包含以下修复： 
 
- 从 CDC 添加或删除表时，将删除 Attunity CDC 实例的“高级”选项中添加的属性。 应用添加 __$command_id 列的 SQL 修复后，Attunity CDC 停止工作
- Oracle CDC 实例在启动时会挂起，并且不会捕获更改。 Oracle 服务器内存可能会增加，直到内存用完或崩溃。
- [2672759](https://support.microsoft.com/kb/2672759)：使用 Attunity Oracle Microsoft 更改数据捕获服务时的错误消息：“ORA-00600: internal error code”（ORA-00600: 内部错误代码）。 添加源级别跟踪并确认是否出现相同的 ORA-00600 错误。 已通过 Oracle 修补程序下载修复。
- 多个分区
    - 在 Oracle 表中使用超过 10 个分区时，CDC 实例无法捕获该表的所有更改。 当 Oracle 表定义的分区超过 10 个时，仅从最后 10 个分区捕获更改。 已在 _SQL Server 2012 Service Pack 1_ 中修复。 请参阅 [SP1 功能包下载页面](https://www.microsoft.com/download/details.aspx?id=35575)。 
- 更改丢失
    - 事件捕获可能会陷入无限循环并停止捕获新的数据更改（与 Oracle bug 5623813 相关）。 在 Oracle RAC 环境中执行 CDC 实例的停止或恢复操作时，可能跳过/丢失更改。 这意味着 SQL Server 更改数据捕获将丢失重要的行，因此，数据仓库或订阅系统中存在数据丢失。 已在 _SQL Server 2012 Service Pack 1_ 中修复。 请参阅 [SP1 功能包下载页面](https://www.microsoft.com/download/details.aspx?id=35575)
- SQL 中的列为双倍宽度
    - 创建 Oracle CDC 实例时，在针对 SQL Server 运行的脚本中，通过脚本创建的 SQL Server 表中的可变宽度数据类型列的宽度是其原来宽度的两倍。 例如，如果尝试跟踪 Oracle 表中 VARCHAR2(10) 列的更改，则 SQL Server 表中的对应列为部署脚本中的 NVARCHAR(20)。 已在 _SQL Server 2012 SP1 累积更新 2_ 或 _SQL Server 2012 累积更新 5_ 中修复，如知识库文章 [2769673](https://support.microsoft.com/kb/2769673) 中所述。 
- DDL 数据被截断
    - 针对包含非拉丁字符的 Oracle 数据库运行大于 4,000 字节的数据定义语言 (DDL) 语句时，Attunity Oracle CDC 会失败。 此外，还会看到错误消息 `ORA-01406: fetched column value was truncated.`。 已在 _SQL Server 2012 SP1 累积更新 4_ 中修复，如知识库文章 [2839806](https://support.microsoft.com/kb/2839806) 中所述。 
- 最后两列中的更改丢失
    - 已在 _SQL Server 2012 SP1 累积更新 6_ 中修复，如知识库文章 [2874879](https://support.microsoft.com/kb/2874879) 中所述。 
- SQL 事务日志在使用 Oracle CDC 时增长
     - 配置 Oracle 更改数据捕获实例后，接收更改数据的 SQL 数据库将具有镜像表，其中标记了待复制的事务。 出现此行为的原因在于 Oracle CDC 依赖于基础系统存储过程，这些过程类似于 SQL Server CDC 中使用的存储过程。 但是，由于单独使用 Oracle CDC 时不涉及 SQL CDC 复制，没有日志读取器来清除标记为待复制的事务。 因为不必在 SQL Server 中复制事务，所以使用本文稍后介绍的解决方法将事务手动标记为已分发是安全的做法。 可在知识库文章 [2871474](https://support.microsoft.com/kb/2871474) 中找到详细信息。 
- 错误 `ORACDC000T: Error encountered at position to change event - SCN not found - EOF simulated`。 已在 _SQL Server 2012 SP1 累积更新 7_ 中修复，如知识库文章 [2883524](https://support.microsoft.com/kb/2883524) 中所述。 
- Oracle 表 cdc.table_name 的元数据验证失败。 列 column_name 索引超出范围。 已在 _SQL Server 2012 SP1 累积更新 7_ 中修复，如知识库文章 [2883524](https://support.microsoft.com/kb/2883524) 中所述。
- 在 SQL Server 2012 中使用 Attunity Oracle CDC 时，Oracle CDC 服务显示中止状态。 已在 _SQL Server 2012 SP1 累积更新 8_ 中修复，如知识库文章 [2923839](https://support.microsoft.com/kb/2923839)中所述。  
- 部分更改会丢失，并且不会复制到 SQL Server 数据库中。 当一个表包含多个字符的大型二进制对象 (CLOB) 并且其中一个 CLOB 的值较大时，会发生此问题。 已在 _SQL Server 2012 SP1 累积更新 8_ 中修复，如知识库文章 [2923839](https://support.microsoft.com/kb/2923839)中所述。   
- 当 Oracle 表具有数据类型为 Long 的列时，Attunity Oracle 更改数据捕获将停止工作。 已在 SQL Server 2012 SP3 累积更新 2 和 SQL 2012 SP2 累积更新 11 中修复，如知识库文章 [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d) 中所述 。 

## <a name="collect-detailed-logs"></a>收集详细日志 

本部分说明如何从 CDC 实例收集错误和事件。 

### <a name="management-console"></a>管理控制台

当 CDC 实例在左窗格中突出显示时，你会在 Oracle 更改数据捕获设计器管理控制台中记录的“状态”消息中看到错误。 

### <a name="query-trace-table"></a>查询跟踪表

可在 SQL Server 的 CDC 数据库中查询跟踪表，以查看记录的错误。 

### <a name="save-output-from-basic-logging"></a>保存基本日志记录的输出 

若要捕获诊断，请在 Oracle 更改数据捕获管理控制台的“状态”选项卡上选择“收集诊断”。 

![显示 Oracle 更改数据捕获管理控制台中“状态”选项卡的屏幕截图，其中突出显示了“收集诊断”选项。](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

选择开始时间和日志文件的位置。 然后选择“创建”以启动诊断收集。 

![“为 testTA 收集诊断”对话框的屏幕截图。](media/known-issues-resolutions-with-cdc-for-oracle-attunity/start-diagnostics.png)

### <a name="detailed-errors"></a>详细错误

可提高实例收集的跟踪级别，并重复该方案以收集更详细的日志记录。 为此，请在“操作”下选择“属性”，然后在“高级”选项卡的“高级设置”网格中添加新属性   。将属性的名称设置为 `trace`，然后将值设置为 `SOURCE`。 

![显示“操作”下的“属性”选项的屏幕截图。](media/known-issues-resolutions-with-cdc-for-oracle-attunity/properties.png)

重现该错误，然后选择“收集诊断”选项以收集日志。 

![Oracle 更改数据捕获管理控制台中“状态”选项卡的另一个屏幕截图，其中突出显示了“收集诊断”选项。](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

## <a name="ora-00942-table-of-view-does-not-exist"></a>ORA-00942 表视图不存在 

这是 CDC 实例的“状态”消息字段中显示的常见错误。 该实例进行了多次重试，因此状态图标会暂时变为绿色，但随后会失败，并显示红色感叹号和“意外”状态。 

![显示 CDC 实例的“状态”消息字段中所示的常见错误的屏幕截图。](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-error.png)

```
"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC508E:Oracle method OCIStmtExecute failed with error: ORA-00942: table or view does not exist ","source","",""

"ERROR","computername","RUNNING","IDLE",
"ORACDC518E:Failed to verify archive log mode.","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC517E:Oracle Call Interface (OCI) method failed: ORA-00942: table or view does not exist .","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC414E:The Engine component failed with return code 1.","engine","",""
```

从 CDC 实例连接到 Oracle 服务器的 Oracle 帐户无权查看系统日志视图时，则会出现此错误。 

### <a name="resolution"></a>解决方法

若要解决此错误，请在 Oracle 数据库系统中授予当前配置的用户适当的权限，或者更改用于从 CDC 实例连接到 Oracle 服务器的帐户。 

安装程序文件文件夹 `C:\Program Files\Change Data Capture for Oracle by Attunity\Attunity.SqlServer.XdbCdcDesigner.chm` 中包含的帮助文件详细列出了所有必需的权限。  有关完整列表，请参阅 .chm 文件中标题为“连接到 Oracle 源数据库”的页面。

可通过在左窗格中选择 CDCInstance 并在“CDC 设计器”窗口最右侧的“操作”窗格中选择“属性”按钮来设置用户帐户。 可从属性对话框页面更改 Oracle 日志挖掘身份验证帐户。

![显示“testTA 属性”对话框的“Oracle”选项卡的屏幕截图。](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-connection.png)


  
## <a name="see-also"></a>另请参阅  
 [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [处理变更数据 (SQL Server)](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [管理和监视变更数据捕获 (SQL Server)](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
