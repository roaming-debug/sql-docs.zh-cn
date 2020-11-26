---
title: 升级故障转移群集实例
description: 使用安装介质升级 SQL Server Always On 故障转移群集实例的步骤。 了解如何滚动升级以及升级多子网群集。
ms.custom: seo-lt-2019
ms.date: 11/21/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- upgrading failover cluster instances
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
ms.assetid: daac41fe-7d0b-4f14-84c2-62952ad8cbfa
author: cawrites
ms.author: chadam
ms.openlocfilehash: cad44bde76e3915aeb5f99d8eeb415d89b02359e
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127585"
---
# <a name="upgrade-a-failover-cluster-instance"></a>升级故障转移群集实例 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集升级到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的新版本、新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务包或积累更新，或在所有故障转移节点上分别将其安装到新的 Windows 服务包或积累更新时进行升级，故障时间限制为一次手动故障转移（或者如果无法故障转移回原始的主要副本，则限制为两次手动故障转移）。  

  
 [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] 之前的操作系统不支持升级包含故障转移群集实例的节点的 Windows Server 操作系统。 若要升级在 [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] 或更高版本上运行的 Windows Server 故障转移群集节点，请参阅[执行滚动升级或更新](#perform-a-rolling-upgrade-or-update)。  
  
 支持详细信息如下所示：  
  
-   既支持通过用户界面进行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 升级，也支持从命令提示符进行升级。 可以从每个故障转移群集节点上的命令提示符处运行升级，也可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序 UI 来升级每个群集节点。 有关详细信息，请参阅：

   - 安装新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例
   - [从命令提示符安装 SQL Server](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 升级不支持以下方案：  
  
    -   不能从独立的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例升级到故障转移群集实例。  
  
    -   不能向故障转移群集实例中添加功能。 例如，不能向现在仅限 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的故障转移群集实例中添加 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]。  
  
    -   不能将故障转移群集实例降级为 Windows Server 故障转移群集的任何节点上的独立实例。  
  
    -   仅允许在某些方案中更改故障转移群集实例的版本。 有关详细信息，请参阅 [支持的版本升级](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。  
  
-   在故障转移群集实例升级过程中，停机时间限制为故障转移时间和运行升级脚本所需的时间。 在开始进行升级过程之前，如果遵循下方的故障转移群集实例滚动升级过程并满足所有节点的所有先决条件，则故障时间将最短。 在内存优化表处于使用中时升级 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会耗费一些额外的时间。 有关详细信息，请参阅 [计划并测试数据库引擎升级计划](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)。  
  
## <a name="prerequisites"></a>先决条件  
 开始之前，请仔细阅读以下重要信息：  
  
-   [支持的版本和版本升级](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md)：验证是否可以从你的 Windows 操作系统版本和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]。 例如，不能直接从 SQL Server 2005 故障转移群集实例升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或不能升级在 [!INCLUDE[winxpsvr-md](../../../includes/winxpsvr-md.md)] 上运行的故障转移群集实例。  
  
-   [选择数据库引擎升级方法](../../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)：检查支持的版本和版本升级以及环境中安装的其他组件，并据此选择适当的升级方法和步骤，按正确顺序升级组件。  
  
-   [计划并测试数据库引擎升级计划](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)：查看发行说明和已知升级问题、预升级清单，并制定和测试升级计划。  
  
-   [安装 SQL Server 的硬件和软件要求](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)：查看安装 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 的软件要求。 如果需要其他软件，则应在升级过程开始之前在每个节点上安装该软件，从而最大程度减少故障时间。  
  
## <a name="perform-a-rolling-upgrade-or-update"></a>执行滚动升级或更新  
 若要将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]，请使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序，从被动节点开始逐个升级加入故障转移群集实例的每个节点。 升级每个节点时，节点被放在故障转移群集实例的可能所有者之外。 如果发生意外故障转移，已升级的节点将不参与故障转移，直到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将 Windows Server 故障转移群集角色的所有权转移给已升级的节点。  
  
 默认情况下， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序自动确定何时将故障转移到已升级的节点。 这取决于故障转移群集实例中节点的总数和已经升级的节点数。 如果有一半或更多节点已经升级，则当你在下一个节点上执行升级时， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序会导致故障转移到已升级的节点。 在故障转移到已升级的节点后，群集组将移至已升级的节点。 所有已升级的节点都放在可能的所有者列表中，所有尚未升级的节点都将从可能的所有者列表中删除。 升级剩余的每个节点时，节点被添加到故障转移群集实例的可能所有者那里。  
  
 此过程使停机时间限制为整个故障转移群集升级过程中的一次故障转移时间和数据库升级脚本执行时间。  
  
 若要控制升级过程中群集节点的故障转移行为，请从命令提示符运行升级操作，并使用 /FAILOVERCLUSTERROLLOWNERSHIP 参数。 有关详细信息，请参阅 [从命令提示符安装 SQL Server](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)。  

 ## <a name="upgrade-with-installation-media"></a>使用安装介质升级 
  
1.  在与你要升级的版本相匹配的版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装媒体中，双击根文件夹中的 setup.exe。 如果之前未安装必备组件，可能会要求您安装它们。  
  
2.  必备组件安装完成后，安装向导会启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装中心。 若要升级 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的现有实例，请选择你的实例。  
  
3.  如果需要使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序支持文件， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将安装它们。 如果安装程序指示您重新启动计算机，请在继续操作之前重新启动。  
  
4.  系统配置检查器将在您的计算机上运行发现操作。 若要继续， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。  
  
5.  在“产品密钥”页上输入与旧产品版本匹配的新版本的 PID 密钥。 例如，若要升级 Enterprise 故障转移群集，必须提供 [!INCLUDE[ssEnterprise](../../../includes/ssenterprise-md.md)]的 PID 密钥。 单击“下一步”以继续。 请注意，对于同一 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例中的所有故障转移群集节点，用于故障转移群集升级的 PID 密钥必须一致。  
  
6.  在“许可条款”页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../../includes/msconame-md.md)]。 单击 **“下一步”** 继续。 若要结束安装程序，请单击 **“取消”** 。  
  
7.  在“选择实例”页上指定要升级到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]实例。 单击 **“下一步”** 继续。  
  
8.  在“功能选择”页上会预先选择要升级的功能。 选择功能名称后，右侧窗格中会显示每个组件组的说明。 请注意，您不能更改要升级的功能，并且不能在升级操作过程中添加功能。 若要在升级操作完成后向 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] 的已升级实例中添加功能，请参阅 [向 SQL Server 2016 的实例添加功能（安装程序）](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md)。  
  
     在右侧窗格中显示所选功能的必备组件。 SQL Server 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。 为了节省时间，应在每个节点上预安装这些必备组件。  
  
9. 在“实例配置”页上，那些字段自动从旧实例进行填充。 您可以选择指定新的 InstanceID 值。  
  
     **实例 ID** - 默认情况下，实例名称用作实例 ID。 这用于标识 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的安装目录和注册表项。 默认实例和命名实例都是如此。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认的实例 ID，请选中 **“实例 ID”** 复选框，并提供一个值。 如果覆盖默认值，则必须为所有故障转移群集节点上要升级的实例指定相同的实例 ID。 已升级的实例的实例 ID 必须在所有节点上匹配。  
  
     **检测到的实例和功能** - 该网格显示运行安装程序的计算机上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 单击 **“下一步”** 继续。  
  
10. “磁盘空间要求”页计算指定的功能所需的磁盘空间，并将磁盘空间要求与正在运行安装程序的计算机上的可用磁盘空间进行比较。  
  
11. 在“全文搜索升级”页上为所升级的数据库指定升级选项。 有关详细信息，请参阅 [全文搜索升级选项](../../../database-engine/install-windows/install-sql-server.md)。  
  
12. 在 **“错误报告”** 页上，指定要发送到 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 以帮助改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的信息。 默认情况下，将启用用于错误报告的选项。  
  
13. 在升级操作开始之前，系统配置检查器将运行多组规则来针对您指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。  
  
14. “群集升级报告”页显示故障转移群集实例中的节点列表和每个节点上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组件的实例版本信息。 它显示数据库脚本状态和复制脚本状态。 此外，还会显示有关单击 **“下一步”** 时会发生的情况的信息性消息。 根据已升级的故障转移群集节点数和节点总数，安装程序会显示你单击“下一步” 时发生的故障转移行为。 如果您尚未安装必备组件，还会就潜在的不必要停机时间向您发出警告。   
  
15. “准备升级”页显示您在安装过程中指定的安装选项的树视图。 若要继续，请单击 **“升级”** 。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将首先安装所选功能的必备组件，然后安装所选功能。  
  
16. 在升级过程中，“进度”页会提供相应的状态，因此您可以在安装程序进行的过程中监视当前节点上的升级进度。  
  
17. 完成当前节点的升级后，“群集升级报告”页将显示所有故障转移群集节点的升级状态信息、每个故障转移群集节点上的功能及其版本信息。 确认显示的版本信息正确，然后继续对剩余节点进行升级。 如果发生了向已升级的节点进行的故障转移，则也会显示在状态页上。 您还可以登入 Windows 群集管理员工具以进行确认。  
  
18. 升级完成后，“完成”页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。  
  
19. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关安装程序日志文件的详细信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
20. 若要完成升级过程，请在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的所有其他节点上重复这些步骤。  
  
## <a name="upgrade-a-multi-subnet-failover-cluster-instance"></a>升级多子网故障转移群集实例  

按照以下步骤在多子网环境中升级 Always On 故障转移群集实例。 
  
### <a name="to-upgrade-to-a-ssnoversion-multi-subnet-failover-cluster-instance-existing-ssnoversion-cluster-is-a-non-multi-subnet-cluster"></a>升级到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集实例（现有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集是非多子网群集）。  
  
1.  按照上述步骤升级故障转移群集实例。  
  
2.  使用 AddNode 安装程序操作添加不同子网上的节点，并确认在“群集网络配置”页将 IP 地址资源依赖关系更改为 OR。 有关详细信息，请参阅[在 Always On 故障转移群集实例中添加或删除节点（安装程序）](../install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
### <a name="to-upgrade-a-multi-subnet-failover-cluster-instance-currently-using-stretch-vlan-to-use-multi-subnet"></a>将当前使用 Stretch VLAN 的多子网故障转移群集实例升级为使用多子网。  
  
1.  按照上述步骤将群集升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]。  
  
2.  更改网络设置以将远程节点移到不同的子网。  
  
3.  使用故障转移群集管理器或 PowerShell 添加新子网的新 IP 地址，将 IP 地址资源依赖关系设置为 OR。  
  
## <a name="next-steps"></a>后续步骤  
 升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]后，请完成下列任务：  
  
-   [完成数据库引擎升级](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)  
  
-   [更改数据库兼容性模式和使用 Query Store](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)  
  
-   [利用新的 SQL Server 2016 功能](../../what-s-new-in-sql-server-2017.md)  
  

  
