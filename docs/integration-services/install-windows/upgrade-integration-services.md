---
description: 升级 Integration Services
title: 升级 Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, upgrading
- SSIS, upgrading
- SQL Server Integration Services, upgrading
- upgrading Integration Services
ms.assetid: 04f9863c-ba0b-47c5-af91-f2d41b078a23
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: de42bca3c6cbcc2ec459bce77ba31e76b572715b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352053"
---
# <a name="upgrade-integration-services"></a>升级 Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  如果计算机上当前安装了 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 或更高版本，可以升级到 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)]。  
  
 当您在已安装了这些 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 早期版本之一的计算机上升级到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 时， [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 将与该早期版本并行安装。  
  
 通过采用这一并行安装，将安装 dtexec 实用工具的多个版本。 若要确保运行正确版本的实用工具，请在命令提示符处输入完整路径 (\<drive>:\Program Files\Microsoft SQL Server\\<version\>\DTS\Binn) 来运行此实用工具。 有关 dtexec 的详细信息，请参阅 [dtexec Utility](../../integration-services/packages/dtexec-utility.md)。  
  
> [!NOTE]  
>  在以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，在您安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后，默认情况下 Users 组中的所有用户都已对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务具有访问权限。 如果你安装 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 及更高版本，则用户无法访问 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。 该服务默认是安全的。 在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员必须运行 DCOM 配置工具 (Dcomcnfg.exe) 以便授予特定用户对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的访问权限。 有关详细信息，请参阅 [Integration Services 服务（SSIS 服务）](../../integration-services/service/integration-services-service-ssis-service.md)。  
  
## <a name="before-upgrading-integration-services"></a>升级 Integration Services 前的准备工作  
 建议您在升级到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]之前先运行升级顾问。 如果将现有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包迁移到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] 所采用的新的包格式，则可能会遇到升级顾问报表问题。  
  
> [!NOTE]
>  SQL Server 2012 版已不支持迁移或运行 Data Transformation Services (DTS) 包。 不再提供以下 DTS 功能：  
> 
>  -   DTS 运行时  
> -   DTS API  
> -   用于将 DTS 包迁移到下一版本的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
> -   中对 DTS 包维护的支持 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
> -   执行 DTS 2000 包任务  
> -   升级 DTS 包的顾问扫描。  
> 
>  有关其他已停止的功能的信息，请参阅 [SQL Server 2016 中已停止使用的 Integration Services 功能](/previous-versions/sql/sql-server-2016/bb500429(v=sql.130))。  
  
## <a name="upgrading-integration-services"></a>升级 Integration Services  
 可以通过使用下列方法之一来进行升级：  
  
-   运行 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] 安装程序，然后选择“从 SQL Server 2008、SQL Server 2008 R2、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 升级 **”选项**。  
  
-   在命令提示符处运行 **setup.exe** ，然后指定 **/ACTION=upgrade** 选项。 有关详细信息，请参阅[从命令提示符安装 SQL Server 2016](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) 中的“[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的安装脚本”部分。  
  
 不能通过升级执行下列操作：  
  
-   重新配置 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的现有安装。  
  
-   从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 32 位版本迁移到 64 位版本，或从 64 位版本迁移到 32 位版本。  
  
-   从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的一个本地化版本迁移到另一个本地化版本。  
  
 升级时，可以同时升级 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和 [!INCLUDE[ssDE](../../includes/ssde-md.md)]，也可以只升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)]，或只升级 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。 如果仅升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)]，则 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 或更高版本仍将正常运行，但是不具有 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)]的功能。 如果仅升级 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，则 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 完全可以正常运行，但只能将包存储在文件系统中，除非其他计算机上有可用的 [!INCLUDE[ssDECurrent](../../includes/ssdecurrent-md.md)] 实例。  
  
## <a name="upgrading-both-integration-services-and-the-database-engine-to-sssql19"></a>将 Integration Services 和数据库引擎同时升级到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]  
 本节介绍执行符合以下条件的升级的影响：  
  
-   将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例同时升级到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]。  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例在同一台计算机上。  
  
### <a name="what-the-upgrade-process-does"></a>升级过程执行的操作  
 升级过程将执行以下任务：  
  
-   安装 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 文件、服务和工具（[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]）。 当同一台计算机上存在多个 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 实例时，在首次将任何实例升级到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]时，将安装 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 文件、服务和工具。  
  
-   将 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]、 or [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例升级到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] 版本。  
  
-   将 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 或更高版本系统表中的数据移到 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 系统表中，如下所示：  
  
    -   移动包而无需将 msdb.dbo.sysdtspackages90 系统表更改为 msdb.dbo.sysssispackages 系统表。  
  
        > [!NOTE]  
        >  虽然数据移动到另一个系统表中，但是升级过程并不将包迁移到新的格式。  
  
    -   将文件夹元数据从 msdb.sysdtsfolders90 系统表移至 msdb.sysssisfolders 系统表。  
  
    -   将日志数据从 msdb.sysdtslog90 系统表移至 msdb.sysssislog 系统表。  
  
-   将数据移动到新的 msdb.sysssis\* 表后，删除 msdb.sysdts*90 系统表和用于访问它们的存储过程。 但是，升级过程将使用一个具有相同名称的 sysdtslog90 视图来替换 sysdtslog90 表。 这个新 sysdtslog90 视图将公开新的 msdb.sysssislog 系统表。 这可确保基于日志表的报表将继续运行而不会中断。  
  
-   为了控制对包的访问，将新建三个固定的数据库级角色：db_ssisadmin、db_ssisltduser 和 db_ssisoperator。 系统不会删除 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 角色 db_dtsadmin、db_dtsltduser 和 db_dtsoperator，而是将其作为对应的新角色的成员。  
  
-   如果 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区（即由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务管理的文件系统位置）为 **\SQL Server\90**、 **\SQL Server\100**、 **\SQL Server\110** 或 **\SQL Server\120** 下的默认位置，则将这些包移到 **\SQL Server\130** 下的新默认位置。  
  
-   更新 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务配置文件以指向升级后的 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例。  
  
### <a name="what-the-upgrade-process-does-not-do"></a>升级过程不执行的操作  
 升级过程不执行以下任务：  
  
-   **不** 删除 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 或更高版本服务。  
  
-   不会将现有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包迁移到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] 所采用的新的包格式。 有关如何迁移包的信息，请参阅 [升级 Integration Services 包](../../integration-services/install-windows/upgrade-integration-services-packages.md)。  
  
-   不会移动位于默认位置以外的文件系统位置（已在服务配置文件中添加这些位置）中的包。 如果之前已向服务配置文件中添加了多个文件系统文件夹，不会将存储在这些文件夹中的包移至新位置。  
  
-   在直接调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dtexec **实用工具 (dtexec.exe) 的** 代理作业步骤中，不要更新 **dtexec** 实用工具的文件系统路径。 必须手动编辑这些作业步骤，才能更新文件系统路径，从而为 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] dtexec **实用工具指定** 位置。  
  
### <a name="what-you-can-do-after-upgrading"></a>升级后可执行的操作  
 升级过程完成后，可以执行下列任务：  
  
-   运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业来运行包。  
  
-   使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 管理存储在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 、 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]或 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]实例中的 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]包。 你需要修改服务配置文件，以便将 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 实例添加到由服务管理的位置列表中。  
  
    > [!NOTE]  
    >  [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的早期版本无法连接到 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 服务。  
  
-   通过检查 PackageFormat 列中的值，确定 msdb.dbo.sysssispackages 系统表中包的版本。 表中包含一个标识每个包的版本的 PackageFormat 列。 值 3 表示 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 包。 在将包迁移到新的包格式之前，packageformat 列中的值将不更改。  
  
-   不能使用 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 工具来设计、运行或管理 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 工具包括 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的相应版本、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导以及包执行实用工具 (dtexecui.exe)。 升级过程不删除 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]工具。 但是，你将无法在已升级的服务器上使用这些工具继续处理 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 或更高版本包。  
  
-   默认情况下，在升级安装中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 被配置为将与运行包相关的事件记录到应用程序事件日志中。 使用 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]的数据收集器功能时，此设置可能生成太多事件日志条目。 记录的事件包括 EventID 12288“包已启动”和 EventID 12289“包已成功完成”。 若要停止将这两个事件记录到应用程序事件日志，请打开注册表进行编辑。 然后在注册表中，找到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS 节点，并将 LogPackageExecutionToEventLog 设置的 DWORD 值从 1 更改为 0。  
  
## <a name="upgrading-only-the-database-engine-to-sssql19"></a>仅将数据库引擎升级到 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]  
 本节介绍执行符合以下条件的升级的影响：  
  
-   仅升级 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例。 也就是说， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例现在为 [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]的实例，但 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 实例和客户端工具均来自于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例位于一台计算机上，而 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和客户端工具位于另一台计算机上。  
  
### <a name="what-you-can-do-after-upgrading"></a>升级后可执行的操作  
 将包保存在已升级的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中的系统表不同于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]中使用的系统表。 因此， [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 的 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 版本和 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 无法发现在已升级的 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的系统表中的包。 由于无法发现这些包，因此在使用这些包时会受到一些限制：  
  
-   不能使用位于其他计算机上的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 工具 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]来加载或管理升级后的 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中的包。  
  
    > [!NOTE]  
    >  尽管升级后的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中的包尚未迁移到新的包格式，但 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 工具仍然无法发现它们。 因此， [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 工具无法使用这些包。  
  
-   不能使用其他计算机上的 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 来运行存储在升级后的 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例上的 msdb 中的包。  
  
-   不能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机上的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 代理作业来运行存储在升级后的 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 实例中的 [!INCLUDE[ssDE](../../includes/ssde-md.md)]包。  
  
## <a name="external-resources"></a>外部资源  
 blogs.msdn.com 上的博客文章： [使您的现有自定义 SSIS 扩展插件和应用程序在 Denali 下工作](https://techcommunity.microsoft.com/t5/sql-server-integration-services/making-your-existing-custom-ssis-extensions-and-applications/ba-p/387951)。  
  
