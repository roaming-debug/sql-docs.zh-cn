---
title: 升级：安装向导（安装程序）
description: SQL Server 安装向导提供了一个用来将 SQL Server 组件就地升级到最新版本的 SQL Server 的功能树。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- upgrading Database Engine
- Database Engine [SQL Server], upgrading
ms.assetid: cef118a5-a7ce-4bfa-8b9d-c81996284cfc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a5621b9444d56307d21855c9b07aefe736d382d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353848"
---
# <a name="upgrade-sql-server-using-the-installation-wizard-setup"></a>使用安装向导升级 SQL Server（安装程序）

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导提供了一个用来将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件就地升级到最新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的功能树。  
  
>[!WARNING]  
>升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后，早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将被覆盖，在计算机中不再存在。 
>
>因此在升级前，请备份 SQL Server 数据库以及与早期的 SQL Server 实例相关的其他对象。  
  
> [!CAUTION]  
> 对于许多生产和某些开发环境，新的安装升级或滚动升级比就地升级更合适。  有关升级方法的详细信息，请参阅：
> * [选择数据库引擎升级方法](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)
> * [升级 Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)
> * [升级 Integration Services](../../integration-services/install-windows/upgrade-integration-services.md)
> * [升级 Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md)
> * [升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)
> * [升级 Analysis Services](../../database-engine/install-windows/upgrade-analysis-services.md)
> * [升级 PowerPivot for SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md)。  
  
## <a name="prerequisites"></a>先决条件  
您必须以管理员身份运行安装程序。 如果从远程共享位置安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，必须使用对该远程共享位置具有读取和执行权限的域帐户且是本地管理员。  
  
> [!WARNING]  
>  请注意，您不能更改要升级的功能，并且不能在升级操作过程中添加功能。 有关如何在升级操作完成后向 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的已升级实例中添加功能的详细信息，请参阅[向 SQL Server 的实例添加功能（安装程序）](./add-features-to-an-instance-of-sql-server-setup.md)。  
  
 如果正在升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)]，请查看 [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md) ，然后根据环境执行以下任务：  
  
-   如有必要，请备份要升级的实例中的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库文件，以便可以还原这些文件。  
  
-   在要升级的数据库上运行适当的数据库控制台命令 (DBCC)，以确保这些数据库处于一致状态。  
  
-   估计升级 SQL Server 组件以及用户数据库所需的磁盘空间。 有关 SQL Server 组件所需磁盘空间的信息，请参阅[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
-   确保将现有的 SQL Server 系统数据库（master、model、msdb 和 tempdb）配置为自动增长，并确保它们具有足够的硬盘空间。  
  
-   确保所有数据库服务器的 master 数据库中都有登录信息。 这对还原数据库很重要，因为 master 数据库中有系统登录信息。  
  
-   禁用所有启动存储过程，因为升级过程在升级 SQL Server 实例时将停止然后再启动服务。 在启动时进行处理的存储过程可能会阻塞升级过程。  
  
-   当升级在 MSX/TSX 关系中登记了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时，应在升级主服务器之前升级目标服务器。 如果您在升级目标服务器之前升级主服务器，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将无法连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的主实例。  
  
-   退出所有应用程序，包括所有依赖 SQL Server 的服务。 如果有本地应用程序连接到要升级的实例，则升级可能会失败。  
  
-   确保复制正在进行，然后停止复制。   
    要了解在复制环境中执行滚动升级的详细步骤，请参阅[升级复制的数据库](../../database-engine/install-windows/upgrade-replicated-databases.md)。
  
## <a name="procedure"></a>过程  
  
### <a name="to-upgrade-ssnoversion"></a>升级 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]  
  
1.  插入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质，然后双击根文件夹中的 Setup.exe。 若要从网络共享进行安装，请移动到共享中的根文件夹，然后双击 Setup.exe。  
  
2.  安装向导启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心。 若要对现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例进行升级，请单击左侧导航区域中的“安装”，然后单击“升级版本...”，从以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级 。  
  
3.  在“产品密钥”页上单击相应的选项，以指示您是升级到免费版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，还是您拥有该产品生产版本的 PID 密钥。 有关详细信息，请参阅 [SQL Server 2016 的版本和组件](../../sql-server/editions-and-components-of-sql-server-2016.md) 和 [支持的版本和版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。  
  
4.  在“许可条款”页上查看许可协议，如果同意，请选中 **“我接受许可条款”** 复选框，然后单击 **“下一步”** 。 为了帮助改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，您还可以启用功能使用情况选项并将报告发送给 [!INCLUDE[msCoName](../../includes/msconame-md.md)]。  
  
5.  在“全局规则”窗口中，如果没有规则错误，安装过程将自动前进到“产品更新”窗口。  
  
6.  如果未在“控制面板”\“所有控制面板项”\“Windows 更新”\“更改设置”中选中“ [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新”复选框，则接下来将显示“ [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新”页。 在“ [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新”页中选中选项会将计算机设置更改为在您浏览 Windows 更新时包括最新更新。  
  
7.  在“产品更新”页中，将显示最近提供的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品更新。 如果你不想包括更新，则取消选中“包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品更新”复选框。 如果未发现任何产品更新， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将不会显示该页并且自动前进到 **“安装安装程序文件”** 页。  
  
8.  在“安装安装程序文件”页上，安装程序将提供下载、提取和安装这些安装程序文件的进度。 如果找到了针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的更新，并且指定了包括该更新，则也将安装该更新。  
  
9. 在“升级规则”窗口中，如果没有规则错误，安装过程将自动前进到“选择实例”窗口。  
  
10. 在“选择实例”页上指定要升级的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 若要升级管理工具和共享功能，请选择 **“仅升级共享功能”** 。  
  
11. 在“选择功能”页上会预先选择要升级的功能。 选择功能名称后，右侧窗格中会显示每个组件组的说明。  
  
     在右侧窗格中显示所选功能的必备组件。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将在本过程后面所述的安装步骤中安装尚未安装的必备组件。  
  
    > [!NOTE]  
    >  如果已通过在“选择实例”页上选择“\<Upgrade shared features only>”升级了共享功能，则在“功能选择”页上将预先选中所有共享功能 。 所有共享组件将同时升级。  
  
12. 在“实例配置”页上，指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的实例 ID。  
  
     **实例 ID** - 默认情况下，实例名称用作实例 ID。 这用于标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的安装目录和注册表项。 默认实例和命名实例都是如此。 对于默认实例，实例名称和实例 ID 为 MSSQLSERVER。 若要使用非默认的实例 ID，请为“实例 ID”文本框提供一个值。  
  
     所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Pack 和升级都将应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的每个组件。  
  
     **已安装的实例** - 该网格显示安装程序正在其中运行的计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 如果计算机上已经安装了一个默认实例，则必须安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的命名实例。  
  
13. 本文中的其余工作流取决于要安装的功能。 您可能不会看到所有的页面，具体取决于您进行的选择。  
  
14. 在“服务器配置 - 服务帐户”页上，显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的默认服务帐户。 此页上配置的实际服务取决于要升级的功能。  
  
     身份验证和登录信息将从早期的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例继承。 您可以为所有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务分配相同的登录帐户，也可以单独配置各个服务帐户。 您还可以指定是自动启动、手动启动还是禁用服务。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议对各服务帐户进行单独配置，以便向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务授予它们完成各自任务所必须拥有的最低权限。 有关详细信息，请参阅 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)预览版本升级问题的解答。  
  
     若要为此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中的所有服务帐户指定相同的登录帐户，请在页面底部的字段中提供凭据。  
  
     **安全说明** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务指定登录信息后，请单击 **“下一步”** 。  
  
15. 在“全文搜索升级选项”页上为所升级的数据库指定升级选项。 有关详细信息，请参阅 [全文搜索升级选项](./install-sql-server.md)。  
  
16. 如果所有规则均通过验证，则“功能规则”窗口将自动前进。  
  
17. “准备升级”页显示您在安装过程中指定的安装选项的树视图。 若要继续，请单击 **“安装”** 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将首先安装所选功能的必备组件，然后安装所选功能。  
  
18. 在安装过程中，进度页会提供相应的状态，因此您可以在安装过程中监视安装进度。  
  
19. 安装完成后，“完成”页会提供指向安装摘要日志文件以及其他重要说明的链接。 若要完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程，请单击 **“关闭”** 。  
  
20. 如果安装程序指示您重新启动计算机，请立即重新启动。 安装完成后，请务必阅读来自安装向导的消息。 有关安装程序日志文件的详细信息，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
## <a name="next-steps"></a>后续步骤  
 升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]后，请完成下列任务：  
  
-   **注册服务器** - 升级会删除早期的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的注册表设置。 升级之后，必须重新注册服务器。  
  
-   **更新统计信息** - 为了帮助优化查询性能，建议在升级之后更新所有数据库的统计信息。 使用 **sp_updatestats** 存储过程可以更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中用户定义的表中的统计信息。  
  
-   **配置新安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** - 为了减少系统的可攻击外围应用，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将有选择地安装和启用一些关键服务和功能。 有关外围应用配置器工具的详细信息，请参阅此版本的自述文件。  
  
## <a name="see-also"></a>另请参阅  
 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [向后兼容性_已删除](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
