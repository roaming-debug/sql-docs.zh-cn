---
title: 重播跟踪以进行 SQL Server 升级
description: 了解如何使用数据库实验助手 SQL Server 升级重播捕获的跟踪。
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 105f471afc10e03c8d6371aa22cce1f6e74ca8c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065912"
---
# <a name="replay-a-trace-in-database-experimentation-assistant"></a>在数据库实验助手中重播跟踪

在数据库实验助手 (DEA) 中，可以针对升级后的测试环境重播捕获的跟踪文件。 例如，请考虑在 SQL Server 2008 R2 上运行的生产工作负荷。 工作负荷的跟踪文件必须重播两次：一次是在生产环境中运行的同一版本的 SQL Server，并在具有升级目标 SQL Server 版本的环境上第二次运行，如 SQL Server 2016。

> [!NOTE]
> 重播跟踪要求您手动将虚拟机或物理计算机设置为运行 Distributed Replay 跟踪。 有关详细信息，请参阅 [Configure Distributed Replay for 数据库实验助手](database-experimentation-assistant-configure-replay.md)。
>

## <a name="configure-a-trace-replay-for-target-1"></a>为目标1配置跟踪重播

首先，您需要对表示您的现有生产环境的目标1执行跟踪重播。

1. 在 DEA 的左侧导航栏上，选择箭头图标，然后在 " **所有重播** " 页上，选择 " **新重播**"。

    ![在 DEA 中创建重播](./media/database-experimentation-assistant-replay-trace/dea-create-replay.png)

    > [!NOTE]
    > Distributed Replay 控制器计算机要求使用远程连接的用户帐户的权限。

2. 在 " **新重播** " 页上的 " **重放详细信息**" 下，输入或选择以下信息：

    - **重播名称**：输入跟踪重播的名称。
    - **源跟踪格式**：指定源跟踪文件 (Trace 或 XEvents) 的格式。
    - **源文件的完整路径**：指定源跟踪文件的完整路径。 如果使用的是 DReplay，则该文件必须存在于充当 DReplay 控制器的计算机上，并且用户帐户需要访问文件和文件夹。
    - **重播工具**： (DReplay 或内置) 指定重播工具。
    - **控制器计算机名称**：指定充当 Distributed Replay 控制器的计算机的名称。
    - **重播跟踪位置**：指定用于存储与跟踪重播关联的跟踪文件/XEvents 的路径。

        > [!NOTE]
        > 对于 Azure SQL 数据库或 Azure SQL 托管实例，需要提供 Azure blob 存储帐户的 SAS URI。

3. 通过选择 **"是，手动还原数据库 () "** 复选框，验证是否已将数据库还原 () 。

4. 在 " **SQL Server 连接详细信息**" 下，输入或选择以下信息：

    - **服务器类型**：指定 SQL Server (**SqlServer**、 **AzureSqlDb**、 **AzureSqlManagedInstance**) 的类型。
    - **服务器名称**：指定 SQL Server 的服务器名称或 IP 地址。
    - **身份验证类型**：对于身份验证类型，请选择 " **Windows**"。
    - **数据库名称**：输入要在其上启动服务器端跟踪的数据库的名称。 如果未指定数据库，则会在服务器上的所有数据库中捕获跟踪。

5. 根据你的方案，选中或取消选中 " **加密连接** " 和 " **信任服务器证书** " 复选框。

    ![新建重播页](./media/database-experimentation-assistant-replay-trace/dea-new-replay.png)

## <a name="start-the-trace-replay-on-target-1"></a>在目标1上启动跟踪重播

- 输入或选择所需的信息后，请选择 " **启动** " 以启动跟踪重播。

  如果输入的信息有效，Distributed Replay 过程将启动。 否则，具有错误信息的文本框将突出显示为红色。 请确保输入的值正确，然后选择 " **启动**"。

  ![针对目标1的重播进度](./media/database-experimentation-assistant-replay-trace/dea-run-replay-target1.png)

  你可以根据需要监视此过程。 当重播完成运行后，DEA 会将结果存储在文件中指定的位置。

  ![针对目标1的重播完成](./media/database-experimentation-assistant-replay-trace/dea-replay-target1-complete.png)

## <a name="perform-the-trace-replay-against-target-2"></a>针对目标2执行跟踪重播

针对目标1执行跟踪重播后，需要针对第二个目标执行相同操作，该目标表示预期的升级环境。

1. 配置跟踪重播，这次使用与目标2环境关联的详细信息。
2. 在目标2上启动跟踪重播。

   你可以根据需要监视此过程。 当重播完成运行后，DEA 会将结果存储在文件中指定的位置。

## <a name="frequently-asked-questions-about-trace-replay"></a>有关跟踪重播的常见问题

**问：在我的目标服务器上启动重播捕获所需的安全权限是什么？**

- 在 DEA 应用程序中运行跟踪操作的 Windows 用户在运行 SQL Server 的目标计算机上必须具有 sysadmin 权限。 需要这些用户权限才能启动跟踪。
- 运行 SQL Server 运行的目标计算机的服务帐户必须具有对指定跟踪文件路径的写访问权限。
- 运行 Distributed Replay 客户端服务的服务帐户必须具有连接到运行 SQL Server 的目标计算机和执行查询的用户权限。

**问：我是否可以在同一会话中启动多个重播？**

是的，您可以启动多个重播并在同一会话中跟踪它们以完成。

**问：我是否可以并行启动多个重播？**

是，但不是在 **控制器和客户端** 中选择的一组计算机。 控制器和客户端将处于繁忙状态。 在 **控制器和客户端** 下设置一组单独的计算机，以开始并行重播。

**问：重播通常需要多长时间才能完成？**

重播通常使用与源跟踪相同的时间，加上预处理源跟踪所需的时间量。 但是，如果向控制器注册的客户端计算机没有足够的时间来管理从重播中生成的负载，则重播可能需要更长的时间才能完成。 最多可以向控制器注册16台客户端计算机。

**问：目标跟踪文件有多大？**

目标跟踪文件的大小可以是源跟踪大小的5到15倍。 文件大小基于运行的查询数。 例如，查询计划 blob 可能会很大。 如果这些查询的统计信息经常更改，则会捕获更多事件。

**问：为什么需要还原数据库？**

SQL Server 是有状态的关系数据库管理系统。 若要正确运行 A/B 测试，数据库的状态必须始终保持不变。 否则，在重播期间可能会出现不在生产中显示的错误。 若要防止出现这些错误，建议您在源捕获之前进行备份。 同样，必须在运行 SQL Server 的目标计算机上还原备份，以防止重播期间发生错误。

**问：重播页上的 "通过%" 是什么意思？**

**Pass%** 表示只传递了百分比的查询。 您可以诊断是否应出现错误数。 这些错误可能是预期的，也可能是由于数据库丢失了其完整性导致的错误。 如果 " **传递%** " 的值不是预期的值，则可以停止跟踪，并在 SQL 事件探查器中查看跟踪文件，以查看哪些查询未成功。

**问：如何查看在重播期间收集的跟踪事件？**

打开目标跟踪文件，并在 SQL 事件探查器中查看它。 或者，如果你想要对重播捕获进行修改，则所有 SQL Server 脚本都将在 C： \\ Program 文件 (x86) \\ Microsoft Corporation \\ 数据库实验助手 \\ 脚本 \\ StartReplayCapture 中使用。

**问：重播期间 DEA 收集哪些跟踪事件？**

DEA 捕获包含与性能相关的信息的跟踪事件。 捕获配置位于 StartReplayCaptureTrace 脚本中。 这些事件是 SQL Server [sp_trace_setevent (transact-sql) 参考文档](../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)中列出的跟踪事件的典型事件。

## <a name="troubleshoot-trace-replay"></a>跟踪重播疑难解答

**问：为什么无法连接到运行 SQL Server 的计算机？**

- 确认运行 SQL Server 的计算机的名称有效。 若要确认，请尝试通过使用 SQL Server Management Studio (SSMS) 连接到服务器。
- 确认防火墙配置不会阻止与运行 SQL Server 的计算机的连接。
- 确认用户具有所需的用户权限。
- 确认 Distributed Replay 客户端的服务帐户有权访问运行 SQL Server 的计算机。

可在% temp% DEA 中的日志中获取更多详细信息 \\ 。 如果问题仍然存在，请与产品团队联系。

**问：为什么无法连接到 Distributed Replay 控制器？**

- 验证 Distributed Replay 控制器服务是否正在控制器计算机上运行。 若要验证，请使用 Distributed Replay 管理工具 (运行命令 `dreplay.exe status -f 1`) "。
- 如果远程启动重播：
  - 确认运行 DEA 的计算机可以成功地 ping 控制器。 根据 **配置重播环境** 页面上的说明，确认防火墙设置允许连接。 有关详细信息，请参阅文章 [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md)。
  - 请确保 Distributed Replay 控制器的用户可以使用 DCOM 远程启动和远程激活。
  - 请确保 Distributed Replay 控制器的用户可以使用 DCOM 远程访问用户权限。

**问：跟踪文件路径在我的计算机上存在。为什么 Distributed Replay 控制器找不到它？**

Distributed Replay 只能访问本地磁盘资源。 必须先将源跟踪文件复制到 Distributed Replay 控制器计算机，然后才能开始重播。 此外，还必须提供 DEA **新重播** 页上的路径。

UNC 路径与 Distributed Replay 不兼容。 Distributed Replay 路径必须是第一个源跟踪文件（包括扩展名）的本地路径和绝对路径。

**问：为什么无法在新的重播页面上浏览文件？**

由于无法浏览远程计算机上的文件夹，因此浏览文件并不有用。 复制和粘贴绝对路径更为有效。

**问：我使用跟踪开始重播，但 Distributed Replay 未重播任何事件。为什么?**

出现此问题的原因可能是跟踪文件没有可重播事件，或者有关于如何重播事件的信息。 确认提供的跟踪文件路径是否指向源跟踪文件。 源跟踪文件是使用 StartCaptureTrace 脚本中提供的配置创建的。

**问：我在尝试使用 SQL Server 2017 Distributed Replay 控制器来预处理跟踪文件时看到 "出现意外错误！"。为什么?**

此问题在 SQL Server 2017 的 RTM 版本中已知。 有关详细信息，请参阅 [使用 DReplay 功能重播捕获的跟踪 SQL Server 2017 中的意外错误](https://support.microsoft.com/help/4045678/fix-unexpected-error-when-you-use-the-dreplay-feature-to-replay-a)。  
  
此问题已在 2017 SQL Server 的最新累积更新1中解决。 下载 [SQL Server 2017 的累积更新 1](https://support.microsoft.com/help/4038634/cumulative-update-1-for-sql-server-2017)的最新版本。

## <a name="see-also"></a>另请参阅

- 若要创建分析报表来帮助获取有关建议更改的信息，请参阅 [创建报表](database-experimentation-assistant-create-report.md)。
