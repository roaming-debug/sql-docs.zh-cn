---
title: 捕获 SQL Server 升级的跟踪
description: 使用数据库实验助手 (DEA) 来创建跟踪文件，其中包含已捕获服务器事件的日志。
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
ms.openlocfilehash: e335170c97f18039767fab8bf0b8400ce9f9b45d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643757"
---
# <a name="capture-a-trace-in-database-experimentation-assistant"></a>在数据库实验助手中捕获跟踪

您可以使用数据库实验助手 (DEA) 来创建跟踪文件，其中包含已捕获服务器事件的日志。 捕获的服务器事件是在特定时间段内发生在特定服务器上的事件。 每个服务器必须运行一次跟踪捕获。

在开始跟踪捕获之前，请确保备份所有目标数据库。

SQL Server 中的查询缓存可能会影响评估结果。 建议在服务应用程序中重新启动 SQL Server 服务 (MSSQLSERVER) ，以提高评估结果的一致性。

## <a name="configure-a-trace-capture"></a>配置跟踪捕获

1. 在 DEA 的左侧导航栏上，选择 "照相机" 图标，然后在 " **所有捕获** " 页上选择 " **新建捕获**"。

    ![在 DEA 中创建捕获](./media/database-experimentation-assistant-capture-trace/dea-initiate-capture.png)

2. 在 " **新建捕获** " 页上的 " **捕获详细信息**" 下，输入或选择以下信息：

    - **捕获名称**：输入捕获的跟踪文件的名称。
    - **Format**：指定捕获 (Trace 或 XEvents) 的格式。
    - **持续时间**：选择想要跟踪捕获运行的时间长度 (以分钟) 。
    - **捕获位置**：选择跟踪文件的目标路径。

        > [!NOTE]
        > 跟踪文件的文件路径必须位于运行 SQL Server 的计算机上。 如果没有为特定帐户设置 SQL Server 服务，则该服务可能需要对要写入的跟踪文件的指定文件夹具有写入权限。

3. 通过选择 **"是，我已手动创建备份 ...** " 来验证是否已执行备份 .。。 复选框。

4. 在 " **捕获详细信息**" 下，输入或选择以下信息：

    - **服务器类型**：指定 SQL Server (**SqlServer**、 **AzureSqlDb**、 **AzureSqlManagedInstance**) 的类型。
    - **服务器名称**：指定 SQL Server 的服务器名称或 IP 地址。
    - **身份验证类型**：对于身份验证类型，请选择 " **Windows**"。
    - **数据库名称**：输入要在其上启动数据库跟踪的数据库的名称。 如果未指定数据库，则会在服务器上的所有数据库中捕获跟踪。

5. 根据你的方案，选中或取消选中 " **加密连接** " 和 " **信任服务器证书** " 复选框。

    ![新建捕获页面](./media/database-experimentation-assistant-capture-trace/dea-new-capture.png)

## <a name="start-the-trace-capture"></a>启动跟踪捕获

1. 输入或选择所需的信息后，请选择 " **启动** " 以启动跟踪捕获。

    如果输入的信息有效，则跟踪捕获过程将开始。 否则，包含无效项的文本框将以红色突出显示。 如果遇到错误，请更正任何必要的条目，然后选择 "重新 **启动** "。

    跟踪捕获运行时，在 " **捕获详细信息**" 下，将显示跟踪捕获进程的状态和进度。

    ![监视捕获进度](./media/database-experimentation-assistant-capture-trace/dea-capture-running.png)

2. 跟踪捕获运行完毕后，新的跟踪 () 文件将保存在初始配置过程中特定的 **捕获位置** 。

    ![已完成跟踪捕获](./media/database-experimentation-assistant-capture-trace/dea-capture-complete.png)

    跟踪文件包含 SQL Server 数据库的活动的跟踪结果。 trace.dat.trc 文件旨在提供有关 SQL Server 检测和报告的错误的详细信息。

## <a name="frequently-asked-questions-about-trace-capture"></a>有关跟踪捕获的常见问题

下面是 DEA 中有关跟踪捕获的一些常见问题解答。

**问：在生产数据库上运行跟踪捕获时捕获哪些事件？**

下表列出了 DEA 为跟踪收集的事件和相应的列数据：
  
|事件名称|文本数据 (1) |二进制数据 (2) |数据库 ID (3) |主机名 (8) |应用程序名称 (10) |登录名 (11) |SPID (12) |开始时间 (14) |结束时间 (15) |数据库名称 (35) |事件序列 (51) |IsSystem (60) |  
|---|---|---|---|---|---|---|---|---|---|---|---|---|  
|**RPC：已完成 (10)**||*|*|*|*|*|*|*|*|*|*|*|  
|**RPC：正在启动 (11)**||*|*|*|*|*|*|*||*|*|*|  
|**RPC 输出参数 (100)**|*||*|*|*|*|*|*||*|*|*|  
|**SQL： BatchCompleted (12)**|*||*|*|*|*|*|*|*|*|*|*|  
|**SQL： BatchStarting (13)**|*||*|*|*|*|*|*||*|*|*|  
|**审核登录 (14)**|*|*|*|*|*|*|*|*||*|*|*|  
|**审核注销 (15)**|*||*|*|*|*|*|*|*|*|*|*|  
|**Existingconnection 事件类 (17)**|*|*|*|*|*|*|*|*||*|*|*|  
|**Cursoropen 事件类 (53)**|*||*|*|*|*|*|*||*|*|*|  
|**Cursorprepare 事件类 (70)**|*||*|*|*|*|*|*||*|*|*|  
|**准备 SQL (71)**|||*|*|*|*|*|*||*|*|*|  
|**Exec 预定义 SQL (72)**|||*|*|*|*|*|*||*|*|*|  
|**Cursorexecute 事件类 (74)**|*||*|*|*|*|*|*||*|*|*|  
|**Cursorunprepare 事件类 (77)**|*||*|*|*|*|*|*||*|*|*|  
|**Cursorclose 事件类 (78)**|*||*|*|*|*|*|*||*|*|*|  

**问：在运行跟踪捕获时，生产服务器上是否会产生性能影响？**

是的，跟踪收集过程中性能的影响最小。 在我们的测试中，我们发现了3% 的内存压力。

**问：捕获生产工作负荷的跟踪需要哪种权限？**

- 在 DEA 应用程序中运行跟踪操作的 Windows 用户必须对运行 SQL Server 的计算机具有 sysadmin 权限。
- 在运行 SQL Server 的计算机上使用的服务帐户必须具有对指定跟踪文件路径的写访问权限。

**问：我能否捕获整个服务器的跟踪，还是只捕获单个数据库的跟踪？**

您可以使用 DEA 来捕获服务器中的所有数据库或单个数据库的跟踪。

**问：我的生产环境中配置了链接服务器。这些查询是否显示在跟踪中？**

如果要为整个服务器运行跟踪捕获，则跟踪将捕获所有查询，包括链接服务器查询。 若要为整个服务器运行跟踪捕获，请将 "**数据库名称** **" 框留空。**

**问：生产工作负荷跟踪的建议最小时间是多少？**

建议选择最能准确表示工作负荷的时间。 这样，分析就会在工作负荷中的所有查询上运行。

**问：在开始跟踪捕获之前，数据库备份的重要程度如何？**

在开始跟踪捕获之前，请确保备份所有目标数据库。 将重播目标1和目标2中捕获的跟踪。 如果数据库状态不相同，则试验的结果不对称。

**问：我是否可以收集 XEvents 而不是跟踪，能否重播 XEvents？**

是。 DEA 支持 XEvents。 下载最新版本的 DEA 并试用。

## <a name="troubleshoot-trace-captures"></a>跟踪捕获疑难解答

如果在运行跟踪捕获时出现错误，请确认：

- 运行 SQL Server 的计算机的名称有效。 若要确认，请尝试通过使用 SQL Server Management Studio (SSMS) 连接到运行 SQL Server 的计算机。
- 防火墙配置不会阻止与运行 SQL Server 的计算机的连接。
- 用户具有 [重播常见问题解答](./database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay)中列出的权限。
- 跟踪名称不遵循标准翻转约定 (捕获 \_ 1) 。 改为尝试跟踪名称（如 Capture \_ 1a 或 Capture1）。

下面是你可能会看到的一些可能的错误以及解决这些错误的解决方案：

|可能的错误|解决方案|  
|---|---|  
|无法在目标 SQL Server 上启动跟踪，请检查您是否具有所需的权限，以及 SQL Server 帐户是否有权访问指定的跟踪文件路径 Sql 错误代码 (53) |运行 DEA 工具的用户必须有权访问运行 SQL Server 的计算机。 必须为用户分配 sysadmin 角色。|  
|无法在目标 SQL Server 上启动跟踪，请检查您是否具有所需的权限，以及 SQL Server 帐户是否有权访问指定的跟踪文件路径 Sql 错误代码 (19062) |指定的跟踪路径可能不存在，或者文件夹对于运行 SQL Server 服务的帐户不具有写入权限 (例如，网络服务) 。 路径必须存在，并且必须具有启动跟踪所需的权限。|  
|当前正在目标服务器上运行的 DEA 跟踪。|目标服务器上已运行活动跟踪。 当服务器范围内的跟踪已在运行时，无法启动新跟踪。|  
|无法打开请求的数据库来捕获跟踪。 此错误可能是由数据库名称不正确引起的。|指定的数据库不存在，或当前用户无法访问该数据库。 使用正确的数据库名称。|  

如果看到标记为 " *Sql 错误代码*" 的任何其他错误，请参阅 [数据库引擎错误](../relational-databases/errors-events/database-engine-events-and-errors.md) 以了解详细说明。

## <a name="see-also"></a>另请参阅

- 若要了解如何在重播捕获的跟踪之前配置 SQL Server 中的 Distributed Replay 工具，请参阅 [为数据库实验助手配置 Distributed Replay](database-experimentation-assistant-configure-replay.md)。