---
title: 可用性组向导：“指定副本”页
description: 介绍 SQL Server Management Studio (SSMS) 中“新建可用性组向导”的“指定副本”页面上提供的选项。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.listeners.f1
- sql13.swb.addreplicawizard.specifyreplicas.f1
- sql13.swb.newagwizard.specifyreplicas.f1
ms.assetid: 2d90fc12-a67b-4bd0-b0ab-899b73017196
author: cawrites
ms.author: chadam
ms.openlocfilehash: 404d2afc78765adebb191c49fb58f6d390516a6c
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583862"
---
# <a name="specify-replicas-page-new-availability-group-wizard-add-replica-wizard"></a>“指定副本”页（新建可用性组向导：添加副本向导）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍 **“指定副本”** 页的选项。 本页适用于 **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]** 的 **[!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]** 。 使用 **“指定副本”** 页可以指定和配置一个或多个要添加到可用性组的可用性副本。 此页包含四个选项卡，下表将逐一介绍。 单击表中的选项卡名称可转到本主题后面的相应部分。  
  
|选项卡|简要说明|  
|---------|-----------------------|  
|[副本](#ReplicasTab)|使用此选项卡可以指定将承载或当前承载辅助副本的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 请注意，您当前连接的服务器实例必须承载主副本。<br /><br /> 请首先在 **“副本”** 选项卡上完成对所有副本的指定，再开始其他选项卡。<br/><br/> 请注意，如果群集类型为“NONE”，“自动故障转移”将处于禁用状态 。 可用性组不在群集中时，SQL Server 仅支持手动故障转移。 <br/><br/> 群集类型为 EXTERNAL 时，故障转移模式为“外部”。 <br/><br/> 如果计划添加副本，则所有新副本都必须托管在与现有副本相同的操作系统类型上。 <br/><br/>添加副本时，如果主要副本在 WSFC 上，则次要副本必须位于同一群集。|
|[端点](#EndpointsTab)|使用此选项卡可以验证任何现有数据库镜像端点，此外，如果在其服务帐户使用 Windows 身份验证的服务器实例上缺少该端点，则会自动创建该端点。|  
|[备份首选项](#BackupPreferencesTab)|使用此选项卡可以整体为可用性组指定您的备份首选项，并为各个可用性副本指定备份优先级。|  
|[侦听器](#Listener)|使用此选项卡（如果可用）可以创建可用性组侦听器。 默认情况下不创建侦听器。<br /><br /> 仅当您正在运行 [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]时，此选项卡才可用。<br/><br/>群集类型为 EXTERNAL 或 NONE 时，DHCP 将处于禁用状态。 |  
  
##  <a name="replicas-tab"></a><a name="ReplicasTab"></a> “副本”选项卡  
 **服务器实例**  
 显示将承载可用性副本的服务器实例的名称。  
  
 如果“可用性副本”网格未列出计划用于托管次要副本的服务器实例，请单击“添加副本”按钮 。 如果在混合 IT 环境中配置可用性组（请参阅 [Azure 虚拟机中 SQL Server 的高可用性和灾难恢复](/previous-versions/azure/jj870962(v=azure.100))），则可单击“添加 Azure 副本”按钮以在 Azure 中创建具有次要副本的虚拟机。  
  
 **初始角色**  
 指示新副本将执行的初始角色：主要或辅助密钥 。  
  
 **自动故障转移(最多 3 个)**  
 仅当您希望此可用性副本成为自动故障转移伙伴时，才选中此复选框。 要配置自动故障转移，您必须为初始主副本和一个辅助副本选中此选项。 这两个副本将同时使用同步提交可用性模式。 只有三个副本才能支持自动故障转移。  
  
 有关同步提交可用性模式的信息，请参阅[可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。 有关自动故障转移的信息，请参阅[故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)。  
  
 **同步提交(最多 3 个)**  
 如果你为副本选择了 **自动故障转移（最多 3 个）** ，则同时选择了 **同步提交（最多 3 个）** 。 如果该复选框为空白，则仅当您希望该副本仅对计划的手动故障转移使用同步提交模式时，才选中此复选框。 只有三个副本才能使用同步提交模式。  
  
 如果您希望此副本使用异步提交可用性模式，则将此复选框保留为空。 此副本将仅支持强制手动故障转移（可能造成数据丢失）。 有关异步提交可用性模式的信息，请参阅[可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。 有关计划的手动故障转移和强制手动故障转移的信息，请参阅[故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)。  
  
 **可读取的辅助角色**  
 从“可读取辅助角色”下拉列表中选择一个值，如下所示：  
  
 **是**  
 不允许与此副本的辅助数据库的直接连接。 它们不可用于读访问。 这是默认设置。  
  
 **仅限读意向**  
 仅允许与此副本的辅助数据库的直接只读连接。 辅助数据库全都可用于读访问。  
  
 **是**  
 允许与此副本的辅助数据库的所有连接，但仅限读访问。 辅助数据库全都可用于读访问。  
  
 **“添加副本”**  
 单击此选项可将辅助副本添加到可用性组。  
  
 **添加 Azure 副本**  
 单击此项以创建在可用性组中运行辅助副本的 Azure 虚拟机。 此选项仅适用于包含本地副本的混合 IT 中的可用性组。 有关详细信息，请参阅 [Azure 虚拟机中 SQL Server 的高可用性和灾难恢复](/previous-versions/azure/jj870962(v=azure.100))。  
  
 **删除副本**  
 单击可从可用性组中删除选择的辅助副本。  
  
##  <a name="endpoints-tab"></a><a name="EndpointsTab"></a> “端点”选项卡  
 对于将承载可用性副本的每个服务器实例， **“端点”** 选项卡将显示现有数据库镜像端点的实际值（如果有）或者要使用 Windows 身份验证的潜在新端点的建议值。 对于现有端点和潜在端点，“端点”值网格将显示以下信息：  
  
 **服务器名称**  
 显示将承载可用性副本的服务器实例的名称。  
  
 **端点 URL**  
 显示数据库镜像端点的实际或建议的 URL。 对于建议的新端点，可以更改此值。 有关这些 URL 格式的信息，请参阅[在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)。  
  
 **端口号**  
 显示此端点的实际或建议的端口号。 对于建议的新端点，可以更改此值。  
  
 **端点名称**  
 显示此端点的实际或建议的名称。 对于建议的新端点，可以更改此值。  
  
 **对数据进行加密**  
 指示是否对通过此端点发送的数据进行加密。 对于建议的新端点，可以更改此设置。  
  
 **SQL Server 服务帐户**  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的用户名。  
  
 为使服务器实例可以使用采用 Windows 身份验证的端点，其 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户必须是域帐户。  
  
 此要求将确定您的下一配置步骤，如下所示：  
  
-   如果每个服务器实例都基于域服务帐户运行，也就是说，如果 **“SQL Server 服务帐户”** 列显示每个服务器实例的域服务帐户，则单击 **“下一步”** 。  
  
-   如果任何服务器实例基于非域服务帐户运行，则您需要首先对您的服务器实例进行手动更改，然后才能在向导中继续执行。 在此情况下，单击 **“下一步”** 将会显示一个警告对话框；您应该单击 **“否”** ，从而返回到 **“端点”** 选项卡。在 **“指定副本”** 页离开向导时，对 **“SQL Server 服务帐户”** 列显示非域服务帐户的每个服务器实例进行以下更改之一：  
  
    -   使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户更改为某一域帐户。 有关详细信息，请参阅[为 SQL Server 更改服务启动帐户（SQL Server 配置管理器）](../../../database-engine/configure-windows/scm-services-change-the-service-startup-account.md)  
  
    -   使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 PowerShell 手动创建使用证书的数据库镜像端点。 有关详细信息，请参阅 [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md) 或 [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)。  
  
     如果在配置端点时保持 **“指定可用性副本”** 页打开，则返回 **“端点”** 选项卡，然后单击 **“刷新”** 以更新 **“端点值”** 网格。  
  
##  <a name="backup-preferences-tab"></a><a name="BackupPreferencesTab"></a> “备份首选项”选项卡  
 若要指定应发生备份的位置，请选择下列选项之一。  
  
 **优先辅助**  
 指定备份应在辅助副本上发生，但在主副本是唯一联机的副本时除外。 在该情况下，备份应在主副本上发生。 这是默认选项。  
  
 **仅辅助**  
 指定备份应该永远不会在主副本上执行。 如果主副本是唯一的联机副本，则备份应不会发生。  
  
 **主要节点**  
 指定备份应该始终在主副本上发生。 如果您需要在对辅助副本运行备份时不支持的备份功能，例如创建差异备份，此选项将很有用。  
  
 **任何副本**  
 指定您希望在选择要执行备份的副本时备份作业将忽略可用性副本的角色。 请注意，备份作业可能评估其他因素，例如每个可用性副本的备份优先级及其操作状态和已连接状态。  
  
> [!IMPORTANT]  
>  没有实施备份首选项设置。 对此首选项的解释依赖于您为给定可用性组中的数据库撰写作业脚本的逻辑（如果有）。 有关详细信息，请参阅[活动次要副本：次要副本备份（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)。  
  
### <a name="replica-backup-priorities-grid"></a>“副本备份优先级”网格  
 使用 **“副本备份优先级”** 网格可为可用性组的每个副本指定备份优先级。 该网格包含以下各列：  
  
 **服务器实例**  
 显示承载可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。  
  
 **备份优先级(最低 = 1，最高 = 100)**  
 分配对此副本执行备份的优先级（相对于同一可用性组中的其他副本）。 默认值为 50。 您可以选择 0..100 范围内的任何整数。 1 表示最低优先级，100 表示最高优先级。 如果你将“备份优先级”设置为 1，则仅在当前没有具有更高优先级的可用性副本时，才选择对此可用性副本执行备份。  
  
 **排除副本**  
 防止选择此可用性副本来执行备份。 例如，这对于您永远不希望备份故障转移到的远程可用性副本十分有用。  
  
##  <a name="listener-tab"></a><a name="Listener"></a> “侦听器”选项卡  
 为将提供客户端连接点的[可用性组侦听器](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)指定你的首选项，其中包括：  
  
 **不立即创建可用性组侦听器。**  
 选择以跳过此步骤。 您可以稍后创建侦听器。 有关详细信息，请参阅 [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。  
  
 **创建可用性组侦听器。**  
 为该可用性组指定侦听器首选项。如下所示：  
  
 **侦听器 DNS 名称**  
 指定侦听器的网络名称。 该名称在域中必须是唯一的，且只能包含字母数字字符、破折号 ( **-** ) 和连字符 ( **_** )，顺序不分先后。 如果使用 **“侦听器”** 选项卡指定该名称，则 DNS 名称的长度可多达 15 个字符。  
  
> [!IMPORTANT]  
>  如果在“侦听程序”选项卡上输入无效的 DNS 侦听程序名称（或端口号），则在“指定副本”页上将禁用“下一步”按钮。  
  
 端口  
 指定该侦听器使用的 TPC 端口。  
  
> [!NOTE]  
>  如果在“侦听程序”选项卡上输入无效的端口号（或 DNS 侦听程序名称），则在“指定副本”页上将禁用“下一步”按钮。  
  
 **网络模式**  
 使用下拉列表选择该侦听器要使用的网络模式，其中包括：  
  
 **静态 IP**  
 如果您希望侦听器侦听多个子网，则选中此选项。 若要使用静态 IP 网络模式，可用性组侦听器必须侦听承载该可用性组的可用性副本的每个子网。 对于每个子网，请单击 **“添加”** ，以选择子网地址并指定 IP 地址。  
  
 如果选择“静态 IP”作为网络模式（这是默认选择），网格中将显示“子网”和“IP 地址”列，并显示关联的“添加”按钮和“删除”按钮。 在添加第一个子网之前，网格为空。  
  
 “子网”列  
 显示您为针对该侦听器添加的每个子网选择的子网地址。  
  
 “IP 地址”列  
 显示您为给定子网指定的 IPv4 或 IPv6 地址。  
  
 **添加**  
 单击以将子网添加到此侦听器。 这将打开 **“添加 IP 地址”** 对话框。 有关详细信息，请参阅[添加 IP 地址对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md) 帮助主题。  
  
 **删除**  
 单击可删除当前网格中选定的子网。  
  
 **DHCP**  
 如果您希望侦听器侦听单个子网并使用运行动态主机配置协议 (DHCP) 的服务器分配的动态 IPv4 地址，则选择此选项。 DHCP 仅限于承载可用性组的可用性副本的每个服务器实例所共有的单个子网。 DHCP 不适用于群集类型 EXTERNAL 或 NONE。  
  
> [!IMPORTANT]  
>  不建议在生产环境中使用 DHCP。 如果停止工作且 DHCP IP 租期已到，则需要额外的时间来注册与侦听器 DNS 名称相关联且影响客户端连接的新 DHCP 网络 IP 地址。 但是，DHCP 适合用于设置开发和测试环境以验证可用性组的基本功能并适合与应用程序集成。  
  
 选择 **DHCP** 后，将显示 **“子网”** 字段。  
  
 **子网**  
 如果你选择“DHCP”作为网络模式，则使用“子网”下拉列表为承载可用性组的可用性副本的子网选择地址。  
  
> [!IMPORTANT]
>  在定义可用性组侦听器后，我们强烈建议您执行以下操作：  
> 
>  -   请求您的网络管理员将该侦听器的 IP 地址保留为专用。 将该侦听器的 DNS 主机名提供给应用程序开发人员，以便在请求与此可用性组的客户端连接时用于连接字符串中。  
> -   将该侦听器的 DNS 主机名提供给应用程序开发人员，以便在请求与此可用性组的客户端连接时用于连接字符串中。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [使用可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [使用“将副本添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
-   [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
