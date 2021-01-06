---
title: 创建新的故障转移群集实例
description: 本文介绍如何使用安装程序安装或升级 SQL Server AlwaysOn 故障转移群集，或将节点添加到现有故障转移群集实例。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- adding nodes
- failover clustering [SQL Server], creating clusters
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- clusters [SQL Server], creating
- removing nodes
ms.assetid: 30e06a7d-75e9-44e2-bca3-b3b0c4a33f61
author: cawrites
ms.author: chadam
ms.openlocfilehash: 841644a1429b4c143b7f2b31b5d6ec669bb0ff58
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642844"
---
# <a name="create-a-new-always-on-failover-cluster-instance-setup"></a>创建新的 AlwaysOn 故障转移群集实例（安装程序）

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  若要安装或升级 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI)，必须在基础 Windows Server 故障转移群集的每个节点上运行安装程序。 若要向现有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例添加节点，则必须在要添加至 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的节点上运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序。 不要在活动节点上运行安装程序以管理其他节点。  
  
 根据节点建立群集的方式不同，按以下方式配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例：  
  
-   同一子网或同一组子网上的节点 - 对于这些配置类型，将 IP 地址资源依赖关系设置为 AND。  
  
-   不同子网上的节点 - 将 IP 地址资源依赖关系设置为 OR，将此配置称为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集实例配置。 有关详细信息，请参阅 [SQL Server 多子网群集 (SQL Server)](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)。  
  
 对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集安装，提供以下选项：  
  
 选项 1：带“添加节点”功能的集成安装   
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 集成故障转移群集安装包括以下步骤：  
  
-   创建并配置单节点 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例。 该节点配置成功以后，您就会有一个完全能够正常运行的故障转移群集实例。 由于此处的故障转移群集实例中只有一个节点，因此，该实例不具有高可用性。  
  
-   在要添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例中的每个节点上，运行带“添加节点”功能的安装程序以添加该节点。  
  
    -   如果要添加的节点具有其他或不同子网，安装程序允许您指定其他 IP 地址。 如果要添加的节点在其他子网上，还需要确认 IP 地址资源依赖关系更改为 OR。 有关在“添加节点”操作期间可能的不同方案的详细信息，请参阅[在 AlwaysOn 故障转移群集实例中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
 **选项 2：** 高级/企业安装  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 高级/企业故障转移群集安装包括以下步骤：  
  
-   在有可能加入新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集的每个节点上，按照 [“准备”一节](#prepare)中列出的“准备故障转移群集”安装步骤执行操作。 您在一个节点上运行“准备故障转移群集”之后，安装程序就会创建 Configuration.ini 文件，该文件列出您指定的所有设置。 在要准备的其他节点上，可以将第一个节点自动生成的 Configuration.ini 文件作为安装程序命令行的输入，而不需要重复执行这些安装步骤。 有关详细信息，请参阅 [使用配置文件安装 SQL Server 2016](../../../database-engine/install-windows/install-sql-server-2016-using-a-configuration-file.md)。 此步骤将准备好节点使其可以加入群集，但在此步骤结束时不会有可供使用的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  
  
-   在准备好用于群集的节点后，在一个准备好的节点上运行安装程序。 此步骤将配置并完成故障转移群集实例。 完成此步骤以后，将有一个可供使用的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例，以前为该实例准备的所有节点将作为新建的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的可能节点。  
  
     如果要创建跨多个子网的故障转移群集实例，安装程序将在具有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 准备好的故障转移群集实例的所有节点检测所有子网的联合。 您可以为子网指定多个 IP 地址。 准备好的每个节点应至少是一个 IP 地址的所有者。  
  
     如果在所有准备好的节点上支持为子网指定的每个 IP 地址，则将依赖关系设置为 AND。  
  
     如果并非在所有准备好的节点上支持为子网指定的每个 IP 地址，则将依赖关系设置为 OR。 有关详细信息，请参阅 [SQL Server 多子网群集 (SQL Server)](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)。  
  
    > [!NOTE]  
    >  完成故障转移群集要求基础 Windows Server 故障转移群集存在。 如果 Windows Server 故障转移群集不存在，安装程序将会出错并退出。  
  
 有关如何向现有故障转移群集实例添加节点或从中删除节点的详细信息，请参阅[在 AlwaysOn 故障转移群集实例中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
 有关远程安装的详细信息，请参阅[支持的版本升级](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。  
  
 有关在 WSFC 中安装 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 的详细信息，请参阅 [如何安装群集 SQL Server Analysis Services](https://go.microsoft.com/fwlink/p/?LinkId=396548)。  
  
## <a name="prerequisites"></a>先决条件  
 在开始之前，请查阅以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书主题：  
  
-   [计划 SQL Server 安装](../../../sql-server/install/planning-a-sql-server-installation.md)  
  
-   [安装故障转移群集前的准备工作](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)  
  
-   [安装 SQL Server 的安全注意事项](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
-   [SQL Server 多子网群集 (SQL Server)](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)  
  
> [!NOTE]  
>  在运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序之前，请记录共享驱动器在群集管理器中的位置。 必须获得此信息以创建新的故障转移群集实例。  
  
### <a name="to-install-a-new-ssnoversion-failover-cluster-instance-using-integrated-install-with-add-node"></a>使用带“添加节点”功能的集成安装来安装新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例。  
  
1.  插入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装介质，然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请浏览到共享中的根文件夹，然后双击 Setup.exe。 有关如何安装必备组件的详细信息，请参阅 [Before Installing Failover Clustering](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)。  
  
2.  安装向导启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装中心。 若要创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的新群集安装，请单击安装页上的 **新 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集安装**。  
  
3.  系统配置检查器将在您的计算机上运行发现操作。 若要继续， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。  
  
4.  若要继续，请单击 **“下一步”** 。  
  
5.  在“安装程序支持文件”页，单击 **“安装”** 以安装安装程序支持文件。  
  
6.  系统配置检查器将在安装继续之前验证计算机的系统状态。 检查完成后，请单击 **“下一步”** 继续。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。  
  
7.  在“产品密钥”页上，指示您是安装免费版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，还是您拥有该产品生产版本的 PID 密钥。 有关详细信息，请参阅 [SQL Server 2016 的版本和组件](../../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
8.  在“许可条款”页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../../includes/msconame-md.md)]。 单击“下一步”以继续。 若要结束安装程序，请单击 **“取消”** 。  
  
9. 在“功能选择”页上，选择要安装的组件。 选择功能名称后，右侧窗格中会显示每个组件组的说明。 您可以选择复选框的任意组合，但仅有 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]、表格模式下的 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 和多维模式下的 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 支持故障转移群集。 其他所选组件将作为不带故障转移功能的独立功能在运行安装程序的当前节点上运行。 有关 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 模式的详细信息，请参阅 [确定 Analysis Services 实例的服务器模式](https://docs.microsoft.com/analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance)。  
  
     在右侧窗格中显示所选功能的必备组件。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。  
  
     您可以使用此页底部的字段为共享组件指定自定义目录。 若要更改共享组件的安装路径，请更新对话框底部字段中所提供的路径，或单击省略号按钮以浏览到另一个安装目录。 默认安装路径为 C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] \\。  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 还支持在服务器消息块 (SMB) 文件共享上安装系统数据库（Master、Model、MSDB 和 TempDB）以及 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 用户数据库。 有关将 SMB 文件共享作为存储安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的详细信息，请参阅 [安装 SQL Server，并使用 SMB 文件共享作为存储选项](../../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md)。  
  
     为共享组件指定的路径必须是绝对路径。 文件夹不能压缩或加密。 不支持映射的驱动器。 如果正在 64 位操作系统上安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，您将看到以下选项：  
  
    1.  共享功能目录  
  
    2.  共享功能目录 (x86)  
  
     为以上每个选项指定的路径必须不同。  
  
    > [!NOTE]  
    >  选择“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 服务”功能时，复制和全文搜索功能将会被自动选中。 在您选择 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 服务功能时，也选择了 Data Quality Services (DQS)。 取消选择这些子功能中的任意一个也会取消选择“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 服务”功能。  
  
10. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序会再运行一组规则，这些规则基于您选择用于验证配置的功能。  
  
11. 在“实例配置”页上指定是安装默认实例还是命名实例。 有关详细信息，请参阅 [Instance Configuration](../../install/instance-configuration.md)。  
  
     **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 网络名称** - 为新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例指定网络名称。 此名称用于在网络中标识故障转移群集实例。  
  
    > [!NOTE]  
    >  该名称在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的早期版本中被称为虚拟 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 名称。  
  
     **实例 ID** - 默认情况下，实例名称用作实例 ID。 这用于标识 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的安装目录和注册表项。 默认实例和命名实例都是如此。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认的实例 ID，请选中 **“实例 ID”** 框，并提供一个值。  
  
    > [!NOTE]  
    >  典型的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 独立实例（无论是默认实例还是命名实例）不会对“实例 ID”框使用非默认值。  
  
     **实例根目录** - 默认情况下，实例根目录为 C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\。 若要指定一个非默认的根目录，请使用所提供的字段，或单击省略号按钮以找到一个安装文件夹。  
  
     **在该计算机上检测到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例和功能** - 该网格将显示运行安装程序的计算机上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 如果计算机上已经安装了一个默认实例，则必须安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的命名实例。 单击“下一步”以继续。  
  
12. 使用“群集资源组”页指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 虚拟服务器资源所在的群集资源组名称或角色名称。 若要指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集资源组名称，您有两种选择：  
  
    -   使用下拉框指定一个要使用的现有组。  
  
    -   键入要创建的新组的名称。 请注意，名称“Available storage”不是有效的组名。  
  
13. 在“群集磁盘选择”页上，为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例选择共享群集磁盘资源。 群集磁盘用于存放 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据。 可以指定多个磁盘。 可用共享磁盘网格会显示可用磁盘的列表、每个磁盘是否可用作共享磁盘以及每个磁盘资源的说明。 单击“下一步”以继续。  
  
    > [!NOTE]  
    >  第一个驱动器用作所有数据库的默认驱动器，但是可以在 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 或 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 配置页上更改它。  
    >   
    >  在“群集磁盘选择”页上，如果您想要使用 SMB 文件共享作为数据文件夹，则可以选择跳过选择任何共享磁盘。  
  
14. 在“群集网络配置”页上，为故障转移群集实例指定网络资源：  
  
    -   **网络设置** - 指定故障转移群集实例的 IP 类型和 IP 地址。  
  
     单击“下一步”以继续。  
  
15. 使用此页以指定群集安全策略。  
  
    -   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和更高版本 - 服务 SID（服务器安全 ID）是推荐的和默认的设置。 没有将此设置更改为安全组的选项。 有关 [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]的服务 SID 功能的详细信息，请参阅 [配置 Windows 服务帐户和权限](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。 这已在 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]的独立安装程序和群集安装程序中进行了测试。  
  
    -   在 [!INCLUDE[winxpsvr](../../../includes/winxpsvr-md.md)]上，为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务指定域组。 所有资源权限都是由包括作为组成员的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的域级组控制。  
  
     单击“下一步”以继续。  
  
16. 本主题中的其余工作流取决于要安装的功能。 您可能不会看到所有的页面，具体取决于您进行的选择（[!INCLUDE[ssDE](../../../includes/ssde-md.md)]、 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]和 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]）。  
  
17. 在“服务器配置 - 服务帐户”页上指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务的登录帐户。 此页上配置的实际服务取决于您选择安装的功能。  
  
     您可以为所有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 将能够识别群集的服务（包括全文搜索和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理）的启动类型设置为“手动”，且在安装过程中不能进行更改。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 建议对各服务帐户进行单独配置，以便为每项服务提供最低特权，即向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务授予它们完成各自任务所必须拥有的最低权限。 有关详细信息，请参阅 [服务器配置 - 服务帐户](https://msdn.microsoft.com/library/c283702d-ab20-4bfa-9272-f0c53c31cb9f) 和 [配置 Windows 服务帐户和权限](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
     若要为此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例中的所有服务帐户指定同一个登录帐户，请在该页底部的字段中提供凭据。  
  
     **安全说明** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务指定登录信息后，请单击 **“下一步”** 。  
  
18. 使用“服务器配置 - 排序规则”选项卡为 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 指定非默认的排序规则。  
  
19. 使用“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 配置 - 帐户设置”页指定以下各项：  
  
    -   安全模式 - 为您的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例选择 Windows 身份验证或混合模式身份验证。 如果选择“混合模式身份验证”，则必须为内置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 系统管理员帐户提供一个强密码。  
  
         在设备与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]成功建立连接之后，用于 Windows 身份验证和混合模式身份验证的安全机制是相同的。  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理员 - 必须为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例至少指定一个系统管理员。 若要添加用以运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序的帐户，请单击 **“添加当前用户”** 。 若要向系统管理员列表中添加帐户或从中删除帐户，请单击 **“添加”** 或 **“删除”** ，然后编辑将拥有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的管理员特权的用户、组或计算机的列表。  
  
     完成对该列表的编辑后， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 验证配置对话框中的管理员列表。 完成此列表后，请单击 **“下一步”** 。  
  
20. 使用“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 配置 - 数据目录”页指定非默认的安装目录。 若要安装到默认目录，请单击 **“下一步”** 。  
  
    > [!IMPORTANT]  
    >  如果指定非默认的安装目录，请确保安装文件夹对于此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的目录共享。 对于故障转移群集实例，数据目录应位于共享群集磁盘上。  
  
    > [!NOTE]  
    >  若要将服务器消息块 (SMB) 文件服务器指定为数据目录，请将“默认数据根目录”指定为 \\\Servername\ShareName\\... 格式的文件共享  
   
21. 使用“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 配置 - FILESTREAM”页对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例启用 FILESTREAM。 单击“下一步”以继续。  
  
22. 使用“[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 配置 - 帐户设置”页指定将拥有 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 的管理员权限的用户或帐户。 您必须为 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]至少指定一个系统管理员。 若要添加用以运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序的帐户，请单击 **“添加当前用户”** 。 若要向系统管理员列表中添加帐户或从中删除帐户，请单击 **“添加”** 或 **“删除”** ，然后编辑将拥有 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]的管理员特权的用户、组或计算机的列表。
  
     完成对该列表的编辑后， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 验证配置对话框中的管理员列表。 完成此列表后，请单击 **“下一步”** 。  
  
23. 使用“ [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 配置 - 数据目录”页指定非默认的安装目录。 若要安装到默认目录，请单击 **“下一步”** 。  
  
    > [!IMPORTANT]  
    >  如果指定非默认的安装目录，请确保安装文件夹对于此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的目录共享。 对于故障转移群集实例，数据目录应位于共享群集磁盘上。  
   
24. 使用“ [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置”页指定要创建的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 安装类型。 对于故障转移群集实例安装，将选项设置为未配置的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 安装。 完成安装后必须配置 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 服务。  
  
 
25. 系统配置检查器将再运行一组规则来针对您指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。  
  
26. “准备安装”页显示安装期间指定的安装选项的树视图。 若要继续，请单击 **“安装”** 。 安装程序将首先安装所选功能的必备组件，然后安装所选功能。  
  
27. 在安装过程中，“安装进度”页会提供相应的状态，因此您可以在安装过程中监视安装进度。  
  
28. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。  
  
29. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关安装程序日志文件的信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
30. 若要在您刚刚创建的单节点故障转移中添加节点，则在每个其他节点上运行安装程序，然后遵照 AddNode 操作的步骤。 有关详细信息，请参阅[在 AlwaysOn 故障转移群集实例中添加或删除节点（安装程序）](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
    > [!NOTE]  
    >  若要添加多个节点，则可以使用配置文件来部署该安装。 有关详细信息，请参阅 [使用配置文件安装 SQL Server 2016](../../../database-engine/install-windows/install-sql-server-2016-using-a-configuration-file.md)。  
    >   
    >  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例中的所有节点上安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本必须相同。 若要向现有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例添加新节点，务必使安装的版本与现有故障转移群集实例的版本相同。  
  
##  <a name="prepare"></a><a name="prepare"></a> 准备  
  
#### <a name="advancedenterprise-failover-cluster-instance-install-step-1-prepare"></a>高级/企业故障转移群集实例安装步骤 1：准备  
  
1.  插入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装介质，然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请浏览到共享中的根文件夹，然后双击 Setup.exe。 有关如何安装必备组件的详细信息，请参阅 [Before Installing Failover Clustering](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)。 如果之前未安装必备组件，可能会要求您安装它们。  
  
2.  Windows Installer 4.5 是必需的，并且可能由安装向导进行安装。 如果系统提示您重新启动计算机，则重新启动计算机，然后再次启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序。  
  
3.  必备组件安装完成后，安装向导会启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装中心。 要准备加入群集的节点，请移至 **“高级”** 页，然后单击 **“高级群集准备”** 。  
  
4.  系统配置检查器将在您的计算机上运行发现操作。 若要继续， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。  
  
5.  在“安装程序支持文件”页，单击 **“安装”** 以安装安装程序支持文件。  
  
6.  系统配置检查器将在安装继续之前验证计算机的系统状态。 检查完成后，请单击 **“下一步”** 继续。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。  
  
7.  如果您在已本地化的操作系统上进行安装，并且安装介质包括针对英语以及与操作系统相对应的语言的语言包，则您可以在“语言选择”页上为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例指定语言。 有关跨语言支持和安装注意事项的详细信息，请参阅 [SQL Server 中的本地语言版本](../../../sql-server/install/local-language-versions-in-sql-server.md)。  
  
     若要继续，请单击 **“下一步”** 。  
  
8.  在“产品密钥”页上，指示您是安装免费版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，还是您拥有该产品生产版本的 PID 密钥。 有关详细信息，请参阅 [SQL Server 2016 的版本和组件](../../../sql-server/editions-and-components-of-sql-server-2016.md)。  
  
    > [!NOTE]  
    >  在为相同故障转移群集实例准备的所有节点上，必须指定相同的产品密钥。  
  
9. 在“许可条款”页上阅读许可协议，然后选中相应的复选框以接受许可条款和条件。 为了帮助改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../../includes/msconame-md.md)]。 单击“下一步”以继续。 若要结束安装程序，请单击 **“取消”** 。  
  
10. 在“功能选择”页上，选择要安装的组件。 选择功能名称后，右侧窗格中会显示每个组件组的说明。 您可以选择复选框的任意组合，但仅有 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]、表格模式下的 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 和多维模式下的 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 支持故障转移群集。 其他所选组件将作为不带故障转移功能的独立功能在运行安装程序的当前节点上运行。 有关 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 模式的详细信息，请参阅 [确定 Analysis Services 实例的服务器模式](https://docs.microsoft.com/analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance)。  
  
     在右侧窗格中显示所选功能的必备组件。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。  
  
     您可以使用此页底部的字段为共享组件指定自定义目录。 若要更改共享组件的安装路径，请更新对话框底部字段中所提供的路径，或单击省略号按钮以浏览到另一个安装目录。 默认安装路径为 C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\。  
  
    > [!NOTE]  
    >  选择“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 服务”功能时，复制和全文搜索功能将会被自动选中。 取消选择这些子功能中的任意一个也会取消选择“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 服务”功能。  
  
11. 在“实例配置”页上指定是安装默认实例还是命名实例。
  
     **实例 ID** - 默认情况下，实例名称用作实例 ID。 这用于标识 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的安装目录和注册表项。 默认实例和命名实例都是如此。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认的实例 ID，请选中 **“实例 ID”** 文本框，并提供一个值。  
  
    > [!NOTE]  
    >  典型的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 独立实例（无论是默认实例还是命名实例）不会对“实例 ID”文本框使用非默认值。  
  
    > [!IMPORTANT]  
    >  对于为故障转移群集实例准备的所有节点，使用相同的实例 ID  
  
     **实例根目录** - 默认情况下，实例根目录为 C:\Program Files\\[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]\\。 若要指定一个非默认的根目录，请使用所提供的字段，或单击省略号按钮以找到一个安装文件夹。  
  
     **已安装的实例** - 该网格显示安装程序正在其中运行的计算机上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 如果计算机上已经安装了一个默认实例，则必须安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的命名实例。 单击“下一步”以继续。  
  
12. “磁盘空间要求”页计算指定的功能所需的磁盘空间，并将磁盘空间要求与正在运行安装程序的计算机上的可用磁盘空间进行比较。  
  
13. 使用此页以指定群集安全策略。  
  
    -   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和更高版本 - 服务 SID（服务器安全 ID）是推荐的和默认的设置。 没有将此设置更改为安全组的选项。 有关 [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]的服务 SID 功能的详细信息，请参阅 [配置 Windows 服务帐户和权限](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。 这已在 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]的独立安装程序和群集安装程序中进行了测试。  
  
    -   在 [!INCLUDE[winxpsvr](../../../includes/winxpsvr-md.md)]上，为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务指定域组。 所有资源权限都是由包括作为组成员的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的域级组控制。  
  
     单击“下一步”以继续。  
  
14. 本主题中的其余工作流取决于要安装的功能。 您可能不会看到所有的页面，具体取决于您进行的选择。  
  
15. 在“服务器配置 - 服务帐户”页上指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务的登录帐户。 此页上配置的实际服务取决于您选择安装的功能。  
  
     您可以为所有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 将能够识别群集的服务（包括全文搜索和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理）的启动类型设置为“手动”，且在安装过程中不能进行更改。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 建议对各服务帐户进行单独配置，以便为每项服务提供最低特权，即向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务授予它们完成各自任务所必须拥有的最低权限。 有关详细信息，请参阅 [配置 Windows 服务帐户和权限](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)预览版本升级问题的解答。  
  
     若要为此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例中的所有服务帐户指定同一个登录帐户，请在该页底部的字段中提供凭据。  
  
     **安全说明** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务指定登录信息后，请单击 **“下一步”** 。  
  
16. 使用“服务器配置 - 排序规则”选项卡为 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 和 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 指定非默认的排序规则。  
  
17. 使用 **服务器配置 - 文件流** 对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例启用 FILESTREAM。  单击“下一步”以继续。  
  
18. 使用“ [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置”页指定要创建的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 安装类型。 对于故障转移群集实例安装，将选项设置为未配置的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 安装。 完成安装后必须配置 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 服务。  
   
19. 在“错误报告”页上，指定要发送给 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 的信息，这些信息有助于改进 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 默认情况下，将启用用于错误报告的选项。  
  
20. 系统配置检查器将再运行一组规则来针对您指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。  
  
21. “准备安装”页显示安装期间指定的安装选项的树视图。 若要继续，请单击 **“安装”** 。 安装程序将首先安装所选功能的必备组件，然后安装所选功能。  
  
     在安装过程中，“安装进度”页会提供相应的状态，因此您可以在安装过程中监视安装进度。 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。  
  
22. 若要完成 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。  
  
23. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关安装程序日志文件的信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
24. 重复以上步骤以准备故障转移群集实例的其他节点。 还可以使用自动生成的配置文件来准备其他节点。 有关详细信息，请参阅 [使用配置文件安装 SQL Server 2016](../../../database-engine/install-windows/install-sql-server-2016-using-a-configuration-file.md)。  
  
## <a name="complete"></a>完成  
  
#### <a name="advancedenterprise-failover-cluster-instance-install-step-2-complete"></a>高级/企业故障转移群集实例安装步骤 2：完成  
  
1.  按照 [准备步骤](#prepare)中的说明准备好所有节点后，在某个准备好的节点上运行安装程序，最好是在拥有共享磁盘的节点上运行。 在  安装中心的“高级” [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 页上，单击 **“高级群集完成”** 。  
  
2.  系统配置检查器将在您的计算机上运行发现操作。 若要继续， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。  
  
3.  在“安装程序支持文件”页，单击 **“安装”** 以安装安装程序支持文件。  
  
4.  系统配置检查器将在安装继续之前验证计算机的系统状态。 检查完成后，请单击 **“下一步”** 继续。 您可以通过单击 **“显示详细信息”** 在屏幕上查看详情，或通过单击 **“查看详细报告”** 从而以 HTML 报告的形式进行查看。  
  
5.  如果您在已本地化的操作系统上进行安装，并且安装介质包括针对英语以及与操作系统相对应的语言的语言包，则您可以在“语言选择”页上为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例指定语言。 有关跨语言支持和安装注意事项的详细信息，请参阅 [SQL Server 中的本地语言版本](../../../sql-server/install/local-language-versions-in-sql-server.md)。  
  
     若要继续，请单击 **“下一步”** 。  
  
6.  使用“群集节点配置”页来选择为群集准备的实例名称，并指定新 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的网络名称。 此名称用于在网络中标识故障转移群集实例。  
  
    > [!NOTE]  
    >  该名称在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的早期版本中被称为虚拟 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 名称。  
  
7.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序会再运行一组规则，这些规则基于您选择用于验证配置的功能。  
  
8.  使用“群集资源组”页指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 虚拟服务器资源所在的群集资源组名称。 若要指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集资源组名称， 可以使用两个选项：  
  
    -   使用列表指定一个现有组以供使用。  
  
    -   键入要创建的新组的名称。 请注意，名称“Available storage”不是有效的组名。  
  
9. 在“群集磁盘选择”页上，为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例选择共享群集磁盘资源。 群集磁盘用于存放 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据。 可以指定多个磁盘。 可用共享磁盘网格会显示可用磁盘的列表、每个磁盘是否可用作共享磁盘以及每个磁盘资源的说明。 单击“下一步”以继续。  
  
    > [!NOTE]  
    >  第一个驱动器用作所有数据库的默认驱动器，但是可以在 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 或 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 配置页上更改它。  
  
10. 在“群集网络配置”页上，为故障转移群集实例指定网络资源：  
  
    -   **网络设置** - 为故障转移群集实例的所有节点和子网指定 IP 类型和 IP 地址。 可以为一个多子网故障转移群集实例指定多个 IP 地址，但每个子网只支持一个 IP 地址。 准备好的每个节点应至少是一个 IP 地址的所有者。 如果你在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例中有多个子网，系统将提示你将 IP 地址资源依赖关系设置为 OR。  
  
     单击“下一步”以继续。  
  
11. 本主题中的其余工作流取决于要安装的功能。 您可能不会看到所有的页面，具体取决于您进行的选择。  
  
12. 使用“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 配置 - 帐户设置”页指定以下各项：  
  
    -   安全模式 - 为您的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例选择 Windows 身份验证或混合模式身份验证。 如果选择“混合模式身份验证”，则必须为内置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 系统管理员帐户提供一个强密码。  
  
         在设备与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]成功建立连接之后，用于 Windows 身份验证和混合模式身份验证的安全机制是相同的。  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理员 - 必须为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例至少指定一个系统管理员。 若要添加用以运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序的帐户，请单击 **“添加当前用户”** 。 若要向系统管理员列表中添加帐户或从中删除帐户，请单击 **“添加”** 或 **“删除”** ，然后编辑将拥有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的管理员特权的用户、组或计算机的列表。  
  
     完成对该列表的编辑后， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 验证配置对话框中的管理员列表。 完成此列表后，请单击 **“下一步”** 。  
  
13. 使用“ [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 配置 - 数据目录”页指定非默认的安装目录。 若要安装到默认目录，请单击 **“下一步”** 。  
  
    > [!IMPORTANT]  
    >  如果指定非默认的安装目录，请确保安装文件夹对于此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的目录共享。 对于故障转移群集实例，数据目录应位于共享群集磁盘上。  
  
  
14. 使用“[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 配置 - 帐户设置”页指定将拥有 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 的管理员权限的用户或帐户。 您必须为 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]至少指定一个系统管理员。 若要添加用以运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序的帐户，请单击 **“添加当前用户”** 。 若要向系统管理员列表中添加帐户或从中删除帐户，请单击 **“添加”** 或 **“删除”** ，然后编辑将拥有 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]的管理员特权的用户、组或计算机的列表。  
  
     完成对该列表的编辑后， [!INCLUDE[clickOK](../../../includes/clickok-md.md)]。 验证配置对话框中的管理员列表。 完成此列表后，请单击 **“下一步”** 。  
  
15. 使用“ [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 配置 - 数据目录”页指定非默认的安装目录。 若要安装到默认目录，请单击 **“下一步”** 。  
  
    > [!IMPORTANT]  
    >  如果指定非默认的安装目录，请确保安装文件夹对于此 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例是唯一的。 此对话框中的任何目录都不应与其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的目录共享。 对于故障转移群集，数据目录应位于共享群集磁盘上。  
  
  
16. 系统配置检查器将再运行一组规则来针对您指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能验证您的计算机配置。  
  
17. “准备安装”页显示安装期间指定的安装选项的树视图。 若要继续，请单击 **“安装”** 。 安装程序将首先安装所选功能的必备组件，然后安装所选功能。  
  
18. 在安装过程中，“安装进度”页会提供相应的状态，因此您可以在安装过程中监视安装进度。  
  
19. 安装完成后， **“完成”** 页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。 完成此步骤后，为同一故障转移群集实例准备的所有节点就变为完成后的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例的一部分。  
  
## <a name="next-steps"></a>后续步骤  
 **配置新安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** - 为了减少系统的可攻击外围应用，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将有选择地安装和启用一些关键服务和功能。 有关详细信息，请参阅 [Surface Area Configuration](../../../relational-databases/security/surface-area-configuration.md)。  
  
 有关日志文件位置的详细信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
## <a name="see-also"></a>另请参阅  
 [从命令提示符安装 SQL Server 2016](../../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)  
  
  
