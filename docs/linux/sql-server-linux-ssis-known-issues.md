---
title: 适用于 Linux 上 SSIS 的限制和已知问题
description: 本文介绍适用于 Linux 计算机上 SQL Server Integration Services (SSIS) 的限制和已知问题
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 06/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ce07fb02a81b4e44375d80f643713b9a0b62d80d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838990"
---
# <a name="limitations-and-known-issues-for-ssis-on-linux"></a>适用于 Linux 上 SSIS 的限制和已知问题

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍适用于 Linux 上 SQL Server Integration Services (SSIS) 的限制和已知问题。

## <a name="general-limitations-and-known-issues"></a>一般限制和已知问题

Linux 上的此版 SSIS 不支持以下功能：
  - SSIS 目录数据库
  - SQL 代理计划的包执行
  - Windows 身份验证
  - 第三方组件
  - 变更数据捕获 (CDC)
  - SSIS Scale Out
  - 适用于 SSIS 的 Azure 功能包
  - Hadoop 和 HDFS 支持
  - Microsoft Connector for SAP BW

有关 Linux 上 SSIS 的其他限制和已知问题，请参阅[发行说明](sql-server-linux-release-notes.md#ssis)。

## <a name="supported-and-unsupported-components"></a><a name="components"></a> 支持和不支持的组件

Linux 支持以下内置 Integration Services 组件。 其中一些在 Linux 平台上有局限性。 Linux 不支持未在此处列出的内置组件。

## <a name="supported-control-flow-tasks"></a>支持的控制流任务
- 大容量插入任务
- 数据流任务
- 数据事件探查任务
- 执行 SQL 任务
- 执行 T-SQL 语句任务
- 表达式任务
- FTP 任务
- Web 服务任务
- XML 任务

## <a name="control-flow-tasks-supported-with-limitations"></a>受支持但有局限性的控制流任务

| 任务 | 限制 |
|------------|---|
| 执行进程任务 | 仅支持进程内模式。 |
| 文件系统任务 | 不支持“移动目录”和“设置文件属性”操作。 |
| 脚本任务 | 仅支持标准 .NET Framework API。 |
| 发送邮件任务 | 仅支持匿名用户模式。 |
| 传输数据库任务 | 不支持 UNC 路径。 |
| | |

## <a name="supported-and-unsupported-maintenance-plan-tasks"></a>支持和不支持的维护计划任务

在 SQL Server 维护计划中，通常可以使用各种 SSIS 任务。

Linux 不支持以下维护计划任务：
- 通知操作员
- 执行 SQL Server 代理作业

Linux 支持以下维护计划任务：
- 检查数据库完整性
- 收缩数据库
- 重新组织索引
- 重新生成索引
- 更新统计信息
- 清除历史记录
- 备份数据库
- T-SQL 语句

## <a name="supported-control-flow-containers"></a>支持的控制流容器
- 序列容器
- For 循环容器
- Foreach 循环容器

## <a name="supported-data-flow-sources-and-destinations"></a>支持的数据流源和目标
- 原始文件源和目标
- XML 源

## <a name="data-flow-sources-and-destinations-supported-with-limitations"></a>受支持但有局限性的数据流源和目标

| 组件 | 限制 |
|------------|---|
| ADO.NET 源和目标 | 仅支持 SQLClient 数据提供程序。 |
| 平面文件源和目标 | 1.仅支持应用默认路径映射规则的 Windows 样式文件路径。 例如，`D:\home\ssis\travel.csv` 变为 `/home/ssis/travel.csv`。 2. 不支持在 Red Hat 7 上装载的 Azure 文件共享。 |
| OData 源 | 仅支持基本身份验证。 |
| ODBC 源和目标 | 支持 Linux 上的 64 位 Unicode ODBC 驱动程序。 取决于 Linux 上的 UnixODBC 驱动程序管理器。 |
| OLE DB 源和目标 | 仅支持 SQL Server Native Client 11.0 和 Microsoft OLE DB Provider for SQL Server。 |
| | |

## <a name="supported-data-flow-transformations"></a>支持的数据流转换
- 聚合
- 审核
- 平衡的数据分发服务器
- 字符映射
- 有条件拆分
- 复制列
- 数据转换
- 派生列
- 导出列
- 模糊分组
- 模糊查找
- 导入列
- 查找
- 合并
- 合并联接
- 多播
- 透视
- 行计数
- 渐变维度
- 排序
- 字词查找
- Union All
- 逆透视

## <a name="data-flow-transformations-supported-with-limitations"></a>受支持但有局限性的数据流转换

| 组件 | 限制 |
|------------|---|
| OLE DB 命令转换 | 与 OLE DB 源和目标的限制相同。 |
| 脚本组件 | 仅支持标准 .NET Framework API。 |
| | |

## <a name="supported-and-unsupported-log-providers"></a>支持和不支持的日志提供程序
Linux 支持除 Windows 事件日志提供程序以外的所有内置 SSIS 日志提供程序。

SQL Server 日志提供程序仅支持 SQL 身份验证；它不支持 Windows 身份验证。

用于文本文件、XML 文件和 SQL Server Profiler 的 SSIS 日志提供程序将其输出写入指定的文件。 以下注意事项适用于文件路径：
-   如果未提供路径，则日志提供程序写入主机的当前目录。 如果当前用户没有写入主机当前目录的权限，则日志提供程序会引发错误。
-   不能在文件路径中使用环境变量。 如果指定环境变量，指定的文本将显示在文件路径中。 例如，如果指定 `%TMP%/log.txt`，则日志提供程序会将文本 `/%TMP%/log.txt` 追加到当前主机目录。

## <a name="related-content-about-ssis-on-linux"></a>有关 Linux 上的 SSIS 的相关内容
-   [使用 SSIS 在 Linux 上提取、转换和加载数据](sql-server-linux-migrate-ssis.md)
-   [在 Linux 上安装 SQL Server Integration Services (SSIS)](sql-server-linux-setup-ssis.md)
-   [使用 ssis-conf 在 Linux 上配置 SQL Server Integration Services](sql-server-linux-configure-ssis.md)
-   [使用 cron 在 Linux 上计划 SQL Server Integration Services 包执行](sql-server-linux-schedule-ssis-packages.md)
