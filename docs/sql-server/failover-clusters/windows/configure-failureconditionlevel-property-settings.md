---
title: 配置 FailureConditionLevel 属性设置
description: 使用 FailureConditionLevel 属性可以设置 AlwaysOn 故障转移群集实例 (FCI) 进行故障转移或重启的条件。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 513dd179-9a46-46da-9fdd-7632cf6d0816
author: cawrites
ms.author: chadam
ms.openlocfilehash: ecb034767917eac38fa2a161fb749ca8d438b995
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642797"
---
# <a name="configure-failureconditionlevel-property-settings"></a>配置 FailureConditionLevel 属性设置
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  使用 FailureConditionLevel 属性可以设置 AlwaysOn 故障转移群集实例 (FCI) 进行故障转移或重启的条件。 对此属性的更改会立即应用，而无需重新启动 Windows Server 故障转移群集 (WSFC) 服务或 FCI 资源。  
  
-   **开始之前：** [FailureConditionLevel 属性设置](#Restrictions)，[安全性](#Security)  
  
-   **若要配置 FailureConditionLevel 属性设置，请使用** [PowerShell](#PowerShellProcedure)、[故障转移群集管理器](#WSFC)、[Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="failureconditionlevel-property-settings"></a><a name="Restrictions"></a> FailureConditionLevel 属性设置  
 针对递增的级别设置故障条件。 对于级别 1-5，每个级别除了自己的条件外，还包括之前级别的所有条件。 这意味着，每个级别越大，故障转移或重新启动的几率不断加大。  有关详细信息，请参阅 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md) 主题的“确定故障”一节。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要 ALTER SETTINGS 和 VIEW SERVER STATE 权限。  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
  
##### <a name="to-configure-failureconditionlevel-settings"></a>配置 FailureConditionLevel 设置  
  
1.  通过 **“以管理员身份运行”** 启动提升的 Windows PowerShell。  
  
2.  导入 **FailoverClusters** 模块以启用群集 cmdlet。  
  
3.  使用 **Get-ClusterResource** cmdlet 查找 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源，然后使用 **Set-ClusterParameter** cmdlet 为故障转移群集实例设置 **FailureConditionLevel** 属性。  
  
> [!TIP]  
>  每次打开新的 PowerShell 窗口时，都需要导入 **FailoverClusters** 模块。  
  
### <a name="example-powershell"></a>示例 (PowerShell)  
 下面的示例将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源“`SQL Server (INST1)`”上的 FailureConditionLevel 设置更改为在出现严重服务器错误时执行故障转移或重新启动。  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter FailureConditionLevel 3  
  
```  
  
### <a name="related-content-powershell"></a>相关内容 (PowerShell)  
  
-   [群集和高可用性](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) （故障转移群集和网络负载平衡团队博客）  
  
-   [故障转移群集上的 Windows PowerShell 入门](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [群集资源命令和等效的 Windows PowerShell cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> 使用故障转移群集管理器管理单元  
 **配置 FailureConditionLevel 属性设置：**  
  
1.  打开故障转移群集管理器管理单元。  
  
2.  展开 **“服务和应用程序”** ，然后选择 FCI。  
  
3.  右键单击“其他资源”下的“SQL Server 资源”，然后从菜单中选择“属性”。 此时将打开 SQL Server 资源 **“属性”** 对话框。  
  
4.  选择 **“属性”** 选项卡，为 **FaliureConditionLevel** 属性输入所需的值，然后单击 **“确定”** 以应用更改。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **配置 FailureConditionLevel 属性设置：**  
  
 使用 [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句，可指定 FailureConditionLevel 属性值。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例将 FailureConditionLevel 属性设置为 0，表示对于任何故障条件将不自动触发故障转移或重新启动。  
  
```  
ALTER SERVER CONFIGURATION SET FAILOVER CLUSTER PROPERTY FailureConditionLevel = 0;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_server_diagnostics (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)   
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
