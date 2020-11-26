---
title: 为可用性组配置 HealthCheckTimeout
description: 为 Always On 可用性组配置 HealthCheckTimeout，用于指定 SQL Server 资源 DLL 在报告无响应之前等待的时间。
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
ms.assetid: 3bbeb979-e6fc-4184-ad6e-cca62108de74
author: cawrites
ms.author: chadam
ms.openlocfilehash: 045240334fd40420064f5e44179ec93373f92de3
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96121074"
---
# <a name="configure-healthchecktimeout-property-settings"></a>配置 HealthCheckTimeout 属性设置
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  HealthCheckTimeout 设置用于指定以毫秒表示的一段时间，在这段时间内 SQL Server 资源 DLL 应等待 [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) 存储过程返回的信息，之后才能将 Always On 故障转移群集实例 (FCI) 报告为不响应。 对超时设置所做的更改会立即生效，不需要重新启动 SQL Server 资源。  
  
-   **开始之前：** [限制和局限](#Limits)、[安全性](#Security)  
  
-   **若要配置 HeathCheckTimeout 设置，请使用：** [PowerShell](#PowerShellProcedure)、[故障转移群集管理器](#WSFC)、[Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Limits"></a> 限制和局限  
 此属性的默认值为 30,000 毫秒（30 秒）。 最小值为 15,000 毫秒（15 秒）。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要 ALTER SETTINGS 和 VIEW SERVER STATE 权限。  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
  
##### <a name="to-configure-healthchecktimeout-settings"></a>配置 HealthCheckTimeout 设置  
  
1.  通过 **“以管理员身份运行”** 启动提升的 Windows PowerShell。  
  
2.  导入 **FailoverClusters** 模块以启用群集 cmdlet。  
  
3.  使用 **Get-ClusterResource** cmdlet 查找 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源，然后使用 **Set-ClusterParameter** cmdlet 为故障转移群集实例设置 **HealthCheckTimeout** 属性。  
  
> [!TIP]  
>  每次打开新的 PowerShell 窗口时，都需要导入 **FailoverClusters** 模块。  
  
### <a name="example-powershell"></a>示例 (PowerShell)  
 下面的示例将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源“`SQL Server (INST1)`”上的 HealthCheckTimeout 设置更改为 60000 毫秒。  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter HealthCheckTimeout 60000  
  
```  
  
### <a name="related-content-powershell"></a>相关内容 (PowerShell)  
  
-   [群集和高可用性](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) （故障转移群集和网络负载平衡团队博客）  
  
-   [故障转移群集上的 Windows PowerShell 入门](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [群集资源命令和等效的 Windows PowerShell cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> 使用故障转移群集管理器管理单元  
 **配置 HealthCheckTimeout 设置**  
  
1.  打开故障转移群集管理器管理单元。  
  
2.  展开 **“服务和应用程序”** ，然后选择 FCI。  
  
3.  右键单击“其他资源”  下的“SQL Server 资源”  ，然后从右键菜单中选择“属性”  。 此时将打开 SQL Server 资源 **“属性”** 对话框。  
  
4.  选择 **“属性”** 选项卡，为 **HealthCheckTimeout** 属性输入所需的值，然后单击 **“确定”** 以应用更改。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 使用 [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句，你可以指定 HealthCheckTimeOut 属性值。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例将 HealthCheckTimeout 选项设置为 15,000 毫秒（15 秒）。  
  
```  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
## <a name="see-also"></a>另请参阅  
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
