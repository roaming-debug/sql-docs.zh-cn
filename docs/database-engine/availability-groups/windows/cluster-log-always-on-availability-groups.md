---
title: 为可用性组生成和分析 CLUSTER.LOG
description: 详细了解如何为 Always On 可用性组生成和分析群集日志。
ms.custom: seo-lt-2019
ms.date: 06/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 01a9e3c1-2a5f-4b98-a424-0ffc15d312cf
author: rothja
ms.author: jroth
ms.openlocfilehash: a03d616a289228695a46f4553e4123ceb9a4810e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643053"
---
# <a name="generate-and-analyze-the-clusterlog-for-an-always-on-availability-group"></a>为 Always On 可用性组生成和分析 CLUSTER.LOG
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  作为故障转移群集资源，SQL Server、Windows Server 故障转移群集服务 (WSFC) 群集和 SQL Server 资源 DLL (hadrres.dll) 之间存在外部交互，SQL Server 中无法对其进行监视。 WSFC 日志（即 CLUSTER.LOG）可以诊断 WSFC 群集或 SQL Server 资源 DLL 中的问题。 
  
## <a name="generate-cluster-log"></a>生成群集日志  
 可以通过两种方式来生成群集日志：  
  
1.  在命令提示符处使用 `cluster /log /g` 命令。 此命令会将群集日志生成到每个 WSFC 节点上的 \windows\cluster\reports 目录。 此方法的优点是，可以使用 `/level` 选项在生成的日志中指定详细信息级别。 其缺点在于，不能为生成的群集日志指定目标目录。 有关详细信息，请参阅 [How to create the cluster.log in Windows Server 2008 Failover Clustering](https://techcommunity.microsoft.com/t5/failover-clustering/how-to-create-the-cluster-log-in-windows-server-2008-failover/ba-p/371283)（如何在 Windows Server 2008 故障转移群集中创建 cluster.log）。  
  
2.  使用 [Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10)) PowerShell cmdlet。 此方法的优点是，可以从所有节点将群集日志生成到运行 cmdlet 的节点上的一个目标目录。 其缺点在于，不能在生成的日志中指定详细信息级别。  
  
 以下 PowerShell 命令在过去 15 分钟内从所有群集节点生成群集日志，并将其放在当前目录中。 在具有管理权限的 PowerShell 窗口中运行命令。  
  
```powershell  
Import-Module FailoverClusters   
Get-ClusterLog -TimeSpan 15 -Destination .  
```  
  
## <a name="always-on-log-verbosity"></a>Always On 日志详细级别  
 可以增加可用性组的 CLUSTER.LOG 中日志的详细级别。 若要修改详细级别，请执行下列步骤：  
  
1.  从“开始”菜单中打开“故障转移群集管理器” 。  
  
2.  展开群集和“服务和应用程序”节点，然后单击可用性组名称。  
  
3.  在“详细信息”窗格中，右键单击可用性组资源，然后单击“属性”。  
  
4.  单击“属性”选项卡。  
  
5.  修改“VerboseLogging”属性。 默认情况下，VerboseLogging 设置为 `0`，它会报告信息、警告和错误。 可以将“VerboseLogging”从 `0` 设置为 `2`。  
  
6.  单击“确定”。  
  
7.  再次右键单击可用性组资源，然后单击“使此资源脱机”。  
  
8.  再次右键单击可用性组资源，然后单击“将此资源联机”。  
  
## <a name="availability-group-resource-events"></a>可用性组资源事件  
 下表显示了可在 CLUSTER.LOG 中看到的不同种类的事件，这些事件与可用性组资源有关。 有关 WSFC 中资源宿主子系统 (RHS) 和资源控制监视器 (RCM) 的详细信息，请参阅 [Resource Hosting Subsystem (RHS) In Windows Server 2008 Failover Clusters](/archive/blogs/askcore/resource-hosting-subsystem-rhs-in-windows-server-2008-failover-clusters)（Windows Server 2008 故障转移群集中的资源宿主子系统 (RHS)）。  
  
|标识符|源|CLUSTER.LOG 中的示例|  
|----------------|------------|------------------------------|  
|带有 `[RES]` 和 `[hadrag]` 前缀的消息|hadrres.dll（Always On 资源 DLL）|00002cc4.00001264::2011/08/05-13:47:42.543 INFO  [RES] SQL Server 可用性组 \<ag>：`[hadrag]` 脱机请求。<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.558 ERR   [RES] SQL 可用性组 \<ag>：`[hadrag]` 租用线程已终止<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.605 INFO  [RES] SQL Server 可用性组 \<ag>：`[hadrag]` 释放 SQL 语句<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.902 INFO  [RES] SQL Server 可用性组 \<ag>：`[hadrag]` 从 SQL Server 断开连接|  
|带有 `[RHS]` 前缀的消息|RHS.EXE（资源宿主子系统，hadrres.dll 的主机进程）|00000c40.00000a34::2011/08/10-18:42:29.498 INFO  [RHS] Resource ag 已脱机。 RHS 即将向 RCM 报告资源状态。|  
|带有 `[RCM]` 前缀的消息|资源控制监视器（群集服务）|000011d0.00000f80::2011/08/05-13:47:42.480 INFO  [RCM] rcm::RcmGroup::Move：首先使组“ag”脱机...<br /><br /> 000011d0.00000f80::2011/08/05-13:47:42.496 INFO  [RCM] TransitionToState(ag) Online-->OfflineCallIssued.|  
|RcmApi/ClusAPI|API 调用，通常意味着 SQL Server 正在请求操作|000011d0.00000f80::2011/08/05-13:47:42.465 INFO  [RCM] rcm::RcmApi::MoveGroup: (ag, 2)|  
  
## <a name="debug-always-on-resource-dll-in-isolation"></a>隔离调试 Always On 资源 DLL  
 调试的最佳做法是将群集配置为与其他资源 DLL 隔离开来运行 Always On 资源 DLL (hadrres.dll)。 默认情况下，WSFC 集群在 rhs.exe 的单个实例中运行所有资源 DLL。 这会导致群集中的所有资源都共享相同的 rhs.exe 实例。 如果尝试使用调试器对 hadrres.dll 进行调试，则在断点处暂停可能导致共享 rhs.exe 实例的其他资源也暂停。 此外，在同一群集中运行多个可用性组时，如果在断点处暂停以调试一个可用性组，则相同的配置可能导致所有可用性组都暂停。  
  
 若要将可用性组与其他群集资源 DLL（包括其他可用性组）隔离，请执行以下操作以在单独的 rhs.exe 进程内运行 hadrres.dll：  
  
1.  打开注册表编辑器并导航到以下项：HKEY_LOCAL_MACHINE\Cluster\Resources。 此项包含所有资源的关键值，每个关键值具有不同的 GUID。  
  
2.  查找包含与可用性组名称相匹配的“名称”值的资源关键字。  
  
3.  将“SeparateMonitor”值更改为“1” 。  
  
4.  在 WSFC 集群中重启可用性组的集群服务。  
  
