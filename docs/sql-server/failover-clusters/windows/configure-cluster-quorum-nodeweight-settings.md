---
title: 配置群集仲裁 NodeWeight 设置
description: 说明如何配置 Windows Server 故障转移群集中成员节点的 NodeWeight 设置。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: cb3fd9a6-39a2-4e9c-9157-619bf3db9951
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2b42e55c1d7bcbe16a56411f3f3d80123fb73d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353980"
---
# <a name="configure-cluster-quorum-nodeweight-settings"></a>配置群集仲裁 NodeWeight 设置
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何配置 Windows Server 故障转移群集 (WSFC) 群集中成员节点的 NodeWeight 设置。 在仲裁投票期间，使用 NodeWeight 设置来支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的灾难恢复和多子网方案。  
  
-   **开始之前：** [先决条件](#Prerequisites)、[安全性](#Security)  
  
-   **若要查看仲裁 NodeWeight 设置，请使用：** [使用 PowerShell](#PowerShellProcedure)、[使用 Cluster.exe](#CommandPromptProcedure)  
  
-   [相关内容](#RelatedContent)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 仅在 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 或更高版本中支持此功能。  
  
> [!IMPORTANT]  
>  为了使用 NodeWeight 设置，必须将以下修补程序应用到 WSFC 群集中的所有服务器：  
>   
>  [KB2494036](https://support.microsoft.com/kb/2494036)：该修补程序用于配置在 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] 中没有仲裁投票的群集节点  
  
> [!TIP]  
>  如果未安装此修补程序，本主题中的示例将为 NodeWeight 返回空或 NULL 值。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 用户必须是一个域帐户，该帐户是每个 WSFC 群集节点上本地 Administrators 组的成员。  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
  
##### <a name="to-configure-nodeweight-settings"></a>配置 NodeWeight 设置  
  
1.  通过 **“以管理员身份运行”** 启动提升的 Windows PowerShell。  
  
2.  导入 `FailoverClusters` 模块以启用群集 commandlet。  
  
3.  使用 `Get-ClusterNode` 对象以设置群集中每个节点的 `NodeWeight` 属性。  
  
4.  以可读格式输出群集节点属性。  
  
### <a name="example-powershell"></a>示例 (PowerShell)  
 下面的示例更改 NodeWeight 设置以删除“AlwaysOnSrv1”节点的仲裁投票，然后输出群集中所有节点的设置。  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "AlwaysOnSrv1"  
(Get-ClusterNode $node).NodeWeight = 0  
  
$cluster = (Get-ClusterNode $node).Cluster  
$nodes = Get-ClusterNode -Cluster $cluster  
  
$nodes | Format-Table -property NodeName, State, NodeWeight  
```  
  
##  <a name="using-clusterexe"></a><a name="CommandPromptProcedure"></a> 使用 cluster.exe  
  
> [!NOTE]  
>  在 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] 版本中不推荐使用 cluster.exe 实用工具。  在将来的开发工作中，请将 PowerShell 与故障转移群集结合使用。  在 Windows Server 的下一版本中，将删除 cluster.exe 实用工具。 有关详细信息，请参阅 [Mapping Cluster.exe Commands to Windows PowerShell Cmdlets for Failover Clusters](https://technet.microsoft.com/library/ee619744\(WS.10\).aspx)（为故障转移群集将 cluster.exe 命令映射到 Windows PowerShell Cmdlet）。  
  
##### <a name="to-configure-nodeweight-settings"></a>配置 NodeWeight 设置  
  
1.  通过 **“以管理员身份运行”** 启动提升的命令提示符。  
  
2.  使用 **cluster.exe** 以设置 `NodeWeight` 值。  
  
### <a name="example-clusterexe"></a>示例 (Cluster.exe)  
 下面的示例更改 NodeWeight 值以删除“Cluster001”群集中“AlwaysOnSrv1”节点的仲裁投票。  
  
```ms-dos  
cluster.exe Cluster001 node AlwaysOnSrv1 /prop NodeWeight=0  
```  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [查看故障转移群集的事件和日志](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog 故障转移群集 Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>另请参阅  
 [WSFC 仲裁模式和投票配置 (SQL Server)](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)   
 [查看群集仲裁 NodeWeight 设置](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)   
 [Windows PowerShell 中按任务焦点列出的故障转移群集 Cmdlet](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
