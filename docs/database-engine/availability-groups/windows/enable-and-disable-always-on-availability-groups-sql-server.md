---
title: 启用或禁用可用性组功能
description: 使用 Transact-SQL (T-SQL)、PowerShell 或 SQL Server Management Studio 启用或禁用 AlwaysOn 可用性组功能的步骤。
ms.custom: seodec18
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], disabling
- Availability Groups [SQL Server], enabling
ms.assetid: 7c326958-5ae9-4761-9c57-905972276a8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 749c98e454556f14283057d33dd5cf5f127c761c
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765604"
---
# <a name="enable-or-disable-always-on-availability-group-feature"></a>启用或禁用 AlwaysOn 可用性组功能
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 是服务器实例使用可用性组的先决条件。 在创建和配置任何可用性组之前，必须在将承载一个或多个可用性组的可用性副本的每个 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例上启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能。  
  
> [!IMPORTANT]  
>  如果您删除后重新创建了 WSFC 群集，则必须在其原始 WSFC 群集上承载可用性副本的每个 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例上都禁用然后重新启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能。  
  
  
##  <a name="prerequisites-for-enabling-always-on-availability-groups"></a><a name="Prerequisites"></a> 启用 AlwaysOn 可用性组的先决条件  
  
-   该服务器实例必须驻留在 Windows Server 故障转移群集 (WSFC) 节点上。  
  
-   该服务器实例必须正在运行支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]的 SQL Server 版本。 有关详细信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
-   一次仅在一个服务器实例上启用 AlwaysOn 可用性组。 在启用 AlwaysOn 可用性组之后，一直等待直到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务已重启，然后才继续在另一个服务器实例上进行操作。  
  
 有关用于创建和配置可用性组的其他先决条件的详细信息，请参阅[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
## <a name="permissions"></a><a name="Permissions"></a> 权限  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例上启用 AlwaysOn 可用性组时，服务器实例具有对 WSFC 群集的完全控制权限。  

 要求本地计算机上 **Administrator** 组中的成员身份以及对 WSFC 群集的完全控制。 使用 PowerShell 启用 AlwaysOn 时，使用“以管理员身份运行”选项打开命令提示符窗口。  
  
 要求 Active Directory 创建对象和管理对象权限。  
  
##  <a name="determine-whether-always-on-availability-groups-is-enabled"></a><a name="IsEnabled"></a> 确定是否已启用 AlwaysOn 可用性组  
  
-   [SQL Server Management Studio](#SSMS1Procedure)  
  
-   [Transact-SQL](#Tsql1Procedure)  
  
-   [PowerShell](#PowerShell1Procedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMS1Procedure"></a> 使用 SQL Server Management Studio  
 **确定是否已启用 AlwaysOn 可用性组**  
  
1.  在“对象资源管理器”中，右键单击服务器实例，再单击“属性”。  
  
2.  在 **“服务器属性”** 对话框中，单击 **“常规”** 页。 **“启用 HADR”** 属性显示以下值之一：  
  
    -   **True**（如果启用了 AlwaysOn 可用性组）  
  
    -   **False**（如果禁用了 AlwaysOn 可用性组）。  
  
###  <a name="using-transact-sql"></a><a name="Tsql1Procedure"></a> 使用 Transact-SQL  
 **确定是否已启用 AlwaysOn 可用性组**  
  
1.  使用以下 [SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md) 语句：  
  
    ```  
    SELECT SERVERPROPERTY ('IsHadrEnabled');  
    ```  
  
     **IsHadrEnabled** 服务器属性的设置指示是否为 AlwaysOn 可用性组启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例，如下所示：  
  
    -   如果 **IsHadrEnabled** = 1，将启用 AlwaysOn 可用性组。  
  
    -   如果 **IsHadrEnabled** = 0，将禁用 AlwaysOn 可用性组。  
  
    > [!NOTE]  
    >  有关 **IsHadrEnabled** 服务器属性的详细信息，请参阅 [SERVERPROPERTY (Transact-SQL)](../../../t-sql/functions/serverproperty-transact-sql.md)的 SQL Server 版本。  
  
###  <a name="using-powershell"></a><a name="PowerShell1Procedure"></a> 使用 PowerShell  
 **确定是否已启用 AlwaysOn 可用性组**  
  
1.  将默认值 (**cd**) 设置为要确定是否启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的服务器实例。  
  
2.  请输入以下 PowerShell **Get-Item** 命令：  
  
    ```  
    PS SQLSERVER:\SQL\NODE1\DEFAULT> get-item . | select IsHadrEnabled  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="enable-always-on-availability-groups"></a><a name="EnableAOAG"></a> 启用 AlwaysOn 可用性组  
 **启用 AlwaysOn，请使用：**  
  
-   [SQL Server 配置管理器](#SQLCM2Procedure)  
  
-   [PowerShell](#PScmd2Procedure)  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM2Procedure"></a> 使用 SQL Server 配置管理器  
 **启用 AlwaysOn 可用性组**  
  
1.  连接到承载要启用 AlwaysOn 可用性组的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的 Windows Server 故障转移群集 (WSFC) 节点。  
  
2.  在“开始”  菜单上，依次指向“所有程序” 、 [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]、“配置工具” ，然后单击“SQL Server 配置管理器” 。  
  
3.  在“SQL Server 配置管理器”中，单击“SQL Server 服务”，右键单击 SQL Server (\<**_instance name_**>)，其中 \<**_instance name_**> 是要启用 Always On 可用性组的本地服务器实例的名称，然后单击“属性”    。  
  
4.  选择“AlwaysOn 高可用性”选项卡。  
  
5.  验证 **Windows 故障转移群集名称** 字段包含本地故障转移群集的名称。 如果此字段为空，则此服务器实例当前不支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。 原因包括本地计算机不是群集节点、WSFC 群集已关闭或此版本的 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 不支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。  
  
6.  选中“启用 AlwaysOn 可用性组”复选框，然后单击“确定” 。  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器保存您的更改。 然后，必须手动重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务。 这使您可以选择最适合您的业务要求的重新启动时间。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务重启后，AlwaysOn 将启用，而且 **IsHadrEnabled** 服务器属性将设置为 1。  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd2Procedure"></a> 使用 SQL Server PowerShell  
 **启用 AlwaysOn**  
  
1.  将目录 (**cd**) 更改为你要为 AlwaysOn 可用性组启用的服务器实例。  
  
2.  使用 Enable-SqlAlwaysOn cmdlet 启用 AlwaysOn 可用性组。  
  
     若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
    > [!NOTE]  
    >  有关如何控制 Enable-SqlAlwaysOn cmdlet 是否重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务的信息，请参阅本主题后面的 [Cmdlet 何时重新启动 SQL Server 服务？](#WhenCmdletRestartsSQL)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
####  <a name="example-enable-sqlalwayson"></a><a name="ExmplEnable-SqlHadrServic"></a> 示例：Enable-SqlAlwaysOn  
 以下 PowerShell 命令在 SQL Server 实例上启用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] （*计算机*\\*实例*）。  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
```  
  
##  <a name="disable-always-on-availability-groups"></a><a name="DisableAOAG"></a> 禁用 AlwaysOn 可用性组  
  
-   **禁用 AlwaysOn 之前：**  
  
     [建议](#Recommendations)  
  
-   **禁用 AlwaysOn，请使用：**  
  
    -   [SQL Server 配置管理器](#SQLCM3Procedure)  
  
    -   [PowerShell](#PScmd3Procedure)  
  
-   **跟进：** [在禁用 AlwaysOn 之后](#FollowUp)  
  
> [!IMPORTANT]  
>  一次只能在一个服务器实例上禁用 AlwaysOn。 在禁用 AlwaysOn 可用性组之后，一直等待直到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务已重启，然后才继续在另一个服务器实例上操作。  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
 在服务器实例上禁用 AlwaysOn 之前，我们建议你执行以下操作：  
  
1.  如果该服务器实例当前正在承载您要保留的可用性组的主副本，我们建议您尽可能手动将该可用性组故障转移到一个同步的辅助副本。 有关详细信息，请参阅[执行可用性组的计划手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)。  
  
2.  删除所有本地辅助副本。 有关详细信息，请参阅[从可用性组中删除次要副本 (SQL Server)](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)。  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM3Procedure"></a> 使用 SQL Server 配置管理器  
 **禁用 AlwaysOn**  
  
1.  连接到承载要禁用 AlwaysOn 可用性组的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的 Windows Server 故障转移群集 (WSFC) 节点。  
  
2.  在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]、 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
3.  在“SQL Server 配置管理器”中，单击“SQL Server 服务”，右键单击 SQL Server (\<**_instance name_**>)，其中 \<**_instance name_**> 是要禁用 Always On 可用性组的本地服务器实例的名称，然后单击“属性”    。  
  
4.  在“Always On 高可用性”选项卡上，取消选中“启用 Always On 可用性组”复选框，然后单击“确定”  。  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器保存您的更改并重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务重启时，将禁用 AlwaysOn 且 **IsHadrEnabled** 服务器属性将设置为 0，以指示已禁用 AlwaysOn 可用性组。  
  
5.  建议阅读本主题后面的[跟进：在禁用 AlwaysOn 之后](#FollowUp)部分中的信息。  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd3Procedure"></a> 使用 SQL Server PowerShell  
 **禁用 AlwaysOn**  
  
1.  将目录 (cd) 更改为你要为 AlwaysOn 可用性组禁用的当前启用的服务器实例。  
  
2.  使用 Disable-SqlAlwaysOn cmdlet 来启用 AlwaysOn 可用性组。  
  
     例如，以下命令在 SQL Server 的实例上禁用 AlwaysOn 可用性组 (*Computer*\\*Instance*)。  此命令需要重新启动实例，并将提示您确认此重新启动。  
  
    ```  
    Disable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
    ```  
  
    > [!IMPORTANT]  
    >  有关如何控制 Disable-SqlAlwaysOn cmdlet 是否重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务的信息，请参阅本主题后面的 [Cmdlet 何时重新启动 SQL Server 服务？](#WhenCmdletRestartsSQL)。  
  
     若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="follow-up-after-disabling-always-on"></a><a name="FollowUp"></a> 跟进：在禁用 AlwaysOn 之后  
 禁用 AlwaysOn 可用性组后，必须重启 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 SQL 配置管理器将自动重新启动该服务器实例。 但是，如果使用了 Disable-SqlAlwaysOn cmdlet，则需要手动重新启动该服务器实例 。 有关详细信息，请参阅 [sqlservr Application](../../../tools/sqlservr-application.md)。  
  
 在重新启动的服务器实例上：  
  
-   可用性数据库在 SQL Server 启动时不启动，因此无法访问它们。  
  
-   唯一支持的 AlwaysOn [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句是 [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md)。 不支持 CREATE AVAILABILITY GROUP、ALTER AVAILABILITY GROUP 和 ALTER DATABASE 的 SET HADR 选项。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 元数据和 WSFC 中的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 配置数据不受禁用 AlwaysOn 可用性组的影响。  
  
 如果你在为承载一个或多个可用性组的可用性副本的每个服务器实例上都永久禁用 AlwaysOn 可用性组，则我们建议你完成以下步骤：  
  
1.  如果你在禁用 AlwaysOn 前未删除本地可用性副本，则删除服务器实例正为其承载可用性副本的每个可用性组。 有关删除可用性组的信息，请参阅[删除可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)。  
  
2.  若要删除留在原地的元数据，请删除服务器实例上作为原始 WSFC 一部分的受影响的可用性组。  
  
3.  所有连接仍可以访问任何主数据库，但是主数据库和辅助数据库之间的数据同步将停止。  
  
4.  辅助数据库将进入 RESTORING 状态。 您可以删除这些数据库，或者可通过使用 RESTORE WITH RECOVERY 还原它们。 但是，还原的数据库不再参与可用性组数据同步。  
  
##  <a name="when-does-a-cmdlet-restart-the-sql-server-service"></a><a name="WhenCmdletRestartsSQL"></a> Cmdlet 何时重新启动 SQL Server 服务？  
 在当前正在运行的服务器实例上，使用 Enable-SqlAlwaysOn 或 Disable-SqlAlwaysOn 更改当前 AlwaysOn 设置可能导致 SQL Server 服务重新启动 。 重新启动行为取决于以下条件：  
  
|指定了 -NoServiceRestart 参数|指定了 -Force 参数|重新启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务？|  
|--------------------------------------------|---------------------------------|---------------------------------------------------------|  
|否|否|默认情况。 但是 cmdlet 提示您以下信息：<br /><br /> **若要完成此操作，必须重新启动服务器实例“<instance_name>”的 SQL Server 服务。是否继续？**<br /><br /> **[Y] 是 [N] 否 [S] 挂起 [?] 帮助（默认值为“Y”）：**<br /><br /> 如果指定 **N** 或 **S**，则不重新启动该服务。|  
|否|是|重新启动服务。|  
|是|否|不重新启动服务。|  
|是|是|不重新启动服务。|  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [SERVERPROPERTY (Transact-SQL)](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
