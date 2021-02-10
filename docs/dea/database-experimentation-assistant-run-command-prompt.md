---
title: 在命令提示符下运行数据库实验助手
description: 了解如何在数据库实验助手 (DEA) 中捕获跟踪，然后从命令提示符中分析结果。
ms.custom: seo-lt-2019
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 0e52cf31a94f2fd63f92305b80f7f7ae049c2809
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066442"
---
# <a name="run-database-experimentation-assistant-at-a-command-prompt"></a>在命令提示符下运行数据库实验助手

本文介绍如何在数据库实验助手 (DEA) 捕获跟踪，然后在命令提示符下分析结果。

   > [!NOTE]
   > 若要了解有关每个 DEA 操作的详细信息，请尝试运行以下命令：
   >
   > `Deacmd.exe -o <operation> --help`
   >
   > 需要操作名称;有效的操作为 **分析**、 **StartCapture** 和 **StopCapture**。

## <a name="start-a-new-workload-capture-by-using-the-dea-command"></a>使用 DEA 命令启动新的工作负荷捕获

若要启动新的工作负荷捕获，请在命令提示符下运行以下命令：

`Deacmd.exe -o StartCapture -n <Trace FileName> -x <Trace Format> -h <SQLServerInstance> -f <database name> -e <Encrypt Connection> -m <Authetication Mode> -u <user name> -p <password> -l <Location of Output Folder> -d <duration>`

**示例**

`Deacmd.exe -o StartCapture -n sql2008capture -x 0 -h localhost -f adventureworks -e --trust -m 0 -l c:\test  -d 60`

**其他选项**

使用命令启动新的工作负荷捕获时 `Deacmd.exe` ，可以使用以下附加选项：

| 选项| 说明 |  
| --- | --- |
| -n, --name | 请求跟踪文件名 |
| -x，--format | 请求trace 的格式 (Trace = 0，XEvents = 1)  |
| -d、--duration | 请求捕获的最大持续时间（分钟） |
| -l, --location | 请求用于在主计算机上存储跟踪/xevent 文件的输出文件夹的位置 |
| -t，--type |  (默认值： 0) 类型/版本的 Sql Server (SqlServer = 0、AzureSQLDB = 1、Azure SQL 托管实例 = 2)  |
| -h、--host | 请求要开始捕获 SQL Server 的主机名和/或实例名 |
| -e，--加密 |  (默认值： True) 加密与 SQL Server 实例的连接。 默认值为 true |
| --信任 |  (默认值：在连接到 SQL Server 实例时) 信任服务器证书。 默认值为 false |
| -f，--databasename |  (默认值：要筛选跟踪的数据库 ) 名称，如果未指定，将在所有数据库上启动捕获 |
| -m、--authmode |  (默认值： 0) 身份验证模式， (Windows = 0，Sql Authentication = 1)  |
| -u，--username | 用于连接到 SQL Server 的用户名 |
| -p，--password | 用于连接到 SQL Server 的密码 |

## <a name="replay-a-workload-capture"></a>重播工作负荷捕获

**使用 Distributed Replay**

如果要使用 Distributed Replay，请执行以下步骤。

1. 登录到 Distributed Replay 控制器计算机。
2. 若要将使用 DEA 命令捕获的工作负荷跟踪转换为 IRF 文件，请在命令提示符下运行以下命令：

    `DReplay preprocess -m "dreplaycontroller" -i "Path to first trace file" -d "<Folder path on controller>\IrfFolder"`

3. 使用 StartReplayCaptureTrace 在运行 SQL Server 的目标计算机上启动跟踪捕获。

    a.  在 SQL Server Management Studio (SSMS) ，打开 <Dea_InstallPath \> \Scripts\StartReplayCaptureTrace.sql。

    b.  运行， `Set @durationInMins=0` 以便跟踪捕获不会在指定的时间后自动停止。

    c.  若要设置每个跟踪文件的最大文件大小，请运行 `Set @maxfilesize` 。 建议的大小为 200 (以 MB) 为单位。

    d.  编辑 `@Tracefile` 以为跟踪文件设置唯一名称。

    e.  `@dbname`如果只能在特定数据库上捕获工作负荷，请编辑以指定数据库名称。 默认情况下，将捕获整个服务器上的工作负荷。

4. 若要针对目标 SQL Server 实例重播 IRF 文件，请在命令提示符下运行以下命令：

    `DReplay replay -m "dreplaycontroller" -d "<Folder Path on Dreplay Controller>\IrfFolder" -o -s "SQL2016Target" -w "dreplaychild1,dreplaychild2,dreplaycild3,dreplaychild4"`

    a.  若要监视状态，请在命令提示符下运行 `DReplay status -f 1` 。

    b.  若要停止重播，例如，如果在命令提示符处看到 pass% 低于预期，请运行 `DReplay cancel` 。

5. 停止目标 SQL Server 实例上的跟踪捕获。
6. 在 SSMS 中打开 `<Dea_InstallPath>\Scripts\StopCaptureTrace.sql` 。
7. 编辑 `@Tracefile` 此项可匹配运行 SQL Server 的目标计算机上的跟踪文件路径。
8. 针对运行 SQL Server 的目标计算机运行脚本。

**使用内置重播**

如果使用的是内置重播，则无需设置 Distributed Replay。 通过命令行使用内置重播的能力正在进行，但在这种情况下，可以使用 GUI 通过内置重播运行重播。

## <a name="analyze-traces-using-the-dea-command"></a>使用 DEA 命令分析跟踪

若要启动新的跟踪分析，请在命令提示符下运行以下命令：

`Deacmd.exe -o analysis -a <Target1 trace filepath> -b <Target2 trace filepath> -r reportname -h <SQLserverInstance> -e <encryptconnection> -u <username>`

**示例**

`Deacmd.exe -o analysis -a C:\Trace\SQL2008Source\Trace.trc -b C:\ Trace\SQL2014Trace\Trace.trc -r upgrade20082014 -h localhost -e`

若要查看这些跟踪文件的分析报告，需要使用 GUI 来查看图表和组织的指标。  不过，分析数据库会写入指定的 SQL Server 实例，因此您还可以直接查询生成的分析表。

**其他选项**

使用 DEA 命令分析跟踪时，可以使用以下附加选项：

| 选项| 说明 |  
| --- | --- |
| -a，--traceA | 请求实例的事件文件的文件路径。 示例 C:\traces\Sql2008trace.trc。  如果有一批文件，请选择第一个文件，DEA 将自动检查滚动更新文件。 如果文件在 blob 中，则提供要在其中存储事件文件的文件夹路径。  示例 C:\traces\ |
| -b，--traceB | 请求B 实例的事件文件的文件路径。 示例 C:\traces\Sql2014trace.trc。 如果有一批文件，请选择第一个文件，DEA 将自动检查滚动更新文件。 如果文件在 blob 中，则提供要在其中存储事件文件的文件夹路径。  示例 C:\traces\ |
| -r，--ReportName | 请求当前分析的名称。 将使用此名称标识生成的分析报告。 |
| -t，--type |  (默认值： 0) 类型/版本的 Sql Server (SqlServer = 0、AzureSQLDB = 1、Azure SQL 托管实例 = 2)  |
| -h、--host | 请求SQL Server 主机名和/或实例名 |
| -e，--加密 |  (默认值： True) 加密与 SQL Server 实例的连接。 默认值为 true |
| --信任 |  (默认值：在连接到 SQL Server 实例时) 信任服务器证书。 默认值为 false |
| -m、--authmode |  (默认值： 0) 身份验证模式， (Windows = 0，Sql Authentication = 1)  |
| -u，--username | 用于连接到 SQL Server 的用户名 |
| --p | 用于连接到 SQL Server 的密码 |
| --ab |  (默认值： False) 跟踪 A 的存储位置在 blob 中。 如果使用，还必须指定--阿尔 (跟踪 Blob Url)  |
| --bb |  (默认值： False) 跟踪 B 的存储位置在 blob 中。 如果使用，还必须指定--bbu (Trace B Blob Url)  |
| --阿尔 | 使用 SAS 密钥的实例的 Blob URL |
| --bbu | 带有 SAS 密钥的 B 实例的 Blob URL |

## <a name="see-also"></a>另请参阅

- 有关使用 DEA 的详细信息，请参阅 [数据库实验助手概述](database-experimentation-assistant-overview.md)。
