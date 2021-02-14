---
description: 安装 Data Quality Services
title: 安装 Data Quality Services
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 486e4216-a946-4c6e-828c-61bc905f7ec1
author: swinarko
ms.author: sawinark
ms.openlocfilehash: c7a91729430078f5913d5f4d83ca75015c50ac15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339898"
---
# <a name="install-data-quality-services"></a>安装 Data Quality Services

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  [!INCLUDE[ssDQSnoversionLong](../../includes/ssdqsnoversionlong-md.md)] (DQS) 包含以下两个组件： **[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]** 和 **[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]** 。  
  
|DQS 组件|描述|  
|-------------------|-----------------|  
|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 在 [!INCLUDE[ssNoversion](../../includes/ssNoVersion-md.md)] 数据库引擎的基础上安装，包括三个数据库：DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA。 DQS_MAIN 包含 DQS 存储过程、DQS 引擎和已发布的知识库。 DQS_PROJECTS 包含数据质量项目信息。 DQS_STAGING_DATA 是临时区域，您可以从中复制源数据来执行 DQS 操作，然后导出已处理的数据。|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] 是一个独立的应用程序，使你可以连接到 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]，并提供一个高度直观的图形用户界面来执行数据质量操作以及与 DQS 相关的其他管理任务。|  
  
> [!IMPORTANT]  
>  除了以上两个 DQS 组件外，您还可以：  
>   
>  使用 Integration Services 中的 DQS 数据清除转换，在 Integration Services 打包过程中执行数据清除，并且在您安装 Integration Services 时会自动安装此组件。 有关安装 Integration Services 的信息，请参阅 [安装 Integration Services](../../integration-services/install-windows/install-integration-services.md)。  
>   
>  在 Master Data Services 中启用 DQS 集成，以使用 Master Data Services Excel 外接程序中与 DQS 相符的功能。 有关详细信息，请参阅 [实现 Data Quality Services 与 Master Data Services 的集成](../../master-data-services/install-windows/enable-data-quality-services-integration-with-master-data-services.md)。  
  
 DQS 安装过程包括三个部分：  
  
-   [安装前任务](#PreInstallationTasks)：在安装 DQS 前验证系统要求。  
  
-   [Data Quality Services 安装任务](#DQSInstallation)：通过使用 SQL Server 安装程序安装 DQS。  
  
-   [安装后任务](#PostInstallationTasks)：使用 SQL Server 安装程序完成安装 DQS 后执行这些任务。  
  
> [!NOTE]  
>  本主题不包括有关从命令行运行安装程序的说明。 若要了解用于安装[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]和客户端的命令行选项，请参阅[从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Feature) 中的[功能参数](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)。  
  
##  <a name="pre-installation-tasks"></a><a name="PreInstallationTasks"></a> 预安装任务  
 在安装 DQS 之前，确保您的计算机满足最低系统要求。 下表提供有关 DQS 组件的最低系统要求的信息：  
  
|DQS 组件|最低系统要求|  
|-------------------|---------------------------------|  
|[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]|内存 (RAM)：最小值：2 GB/建议：4 GB 或更多<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 数据库引擎。 有关详细信息，请参阅 [安装 SQL Server 数据库引擎](../../database-engine/install-windows/install-sql-server-database-engine.md)。|  
|[!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]|.NET Framework 4.0（如果尚未安装，则在 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] 安装期间安装）<br /><br /> Internet Explorer 6.0 SP1 或更高版本|  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 和 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] 既可安装在同一台计算机上，也可以安装在不同计算机上。 这两个组件可以彼此独立并按任何顺序进行安装。 但是，要使用 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]，您将需要安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 以进行连接。  
>   
>  使用当前或以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 和 DQS 清除转换，您可以连接到 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 版本的 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] 。 有关升级现有 DQS 版本到 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]的信息，请参阅 [升级 Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)。  
>   
>  尽管 Microsoft Excel 不是用于安装数据质量客户端的先决条件，但 Microsoft Excel 2003 或更高版本必须安装在数据质量客户端计算机上以在客户端应用程序中执行各种操作，例如从某一 Excel 文件导入域值，或者映射到 Excel 文件中的源数据以便进行知识发现、清理或匹配活动。  
  
 若要详细了解安装 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 所需满足的最低系统要求，请参阅[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
##  <a name="data-quality-services-installation-tasks"></a><a name="DQSInstallation"></a> Data Quality Services 安装任务  
 您必须使用 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 安装程序安装 DQS 组件。 运行 SQL Server 安装程序时，必须完成一系列的安装向导页面，以根据您的要求选择适当的选项。 下表仅列出了安装向导页面中供选择的选项将对 DQS 安装产生影响的那些页面：  
  
|页|操作|  
|----------|------------|  
|特征选择|选择：<br /><br /> **“数据库引擎服务”** 下的 **“Data Quality Services”** 以安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]。 <br />如果你选中 **“数据库引擎服务”** 复选框，SQL Server 安装程序会将安装程序文件 DQSInstaller.exe 复制到你的计算机上的 SQL Server 实例目录下。 在完成 SQL Server 安装程序以 *完成*[!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装后，您必须运行此文件。 此外，您必须执行一些附加步骤来配置 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] ，然后才能使用它。 有关详细信息，请参阅 [安装后任务](#PostInstallationTasks)。<br /><br /> **“数据质量客户端”** 以安装 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]。<br /><br />  (推荐的) **管理工具-完整** 的 **管理工具-基本** 安装 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。 它为您提供一个图形用户界面来管理您的 SQL Server 实例，并将帮助您执行在下一部分中列出的其他安装后任务。|  
|数据库引擎配置|单击 **“添加当前用户”** 以便将您的用户 Windows 帐户添加到 sysadmin 固定服务器角色。 若要能在稍后运行 DQSInstaller.exe 文件以完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装，则必须执行此操作。|  
  
##  <a name="post-installation-tasks"></a><a name="PostInstallationTasks"></a> 安装后任务  
 在您完成 SQL Server 安装向导之后，您必须执行此部分中提到的额外步骤来完成您的 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装、对其进行配置，然后使用该服务器。  
  
1.  若要完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装，必须运行 DQSInstaller.exe 文件。 在运行 DQSInstaller.exe 文件后：  
  
    -   创建 DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA 数据库。  
  
    -   创建 ##MS_dqs_db_owner_login## 和 ##MS_dqs_service_login## 登录名。  
  
    -   dqs_administrator、dqs_kb_editor 和 dqs_kb_operator 角色在 DQS_MAIN 数据库中创建。  
  
    -   DQInitDQS_MAIN 存储过程在 master 数据库中创建。  
  
    -   DQS_install.log 文件通常在 C:\Program Files\Microsoft SQL Server\MSSQL13.*<instance_name>* \MSSQL\Log 文件夹中创建。 该文件包含与对 DQSInstaller.exe 文件执行的操作有关的信息。  
  
    -   如果 Master Data Services 数据库与 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]处于相同的 SQL Server 实例中，则会创建一个映射到 Master Data Services 登录名的用户，并向该用户授予对 DQS_MAIN 数据库的 dqs_administrator 角色。  
  
     此时就完成了 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 的安装。  
  
     有关详细信息，请参阅 [运行 DQSInstaller.exe 以完成数据质量服务器安装](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md)。  
  
2.  将 DQS 角色授予用户：  
  
     若要使用 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 登录到 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]，用户必须对 DQS_MAIN 数据库具有以下三种角色的任何一种：  
  
    -   **dqs_administrator**  
  
    -   **dqs_kb_editor**  
  
    -   **dqs_kb_operator**  
  
     默认情况下，如果您的用户帐户是 sysadmin 固定服务器角色的成员，则您可以使用 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 登录到 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)] ，即使在没有任何 DQS 角色授予您的用户帐户的情况下也是如此。 有关这三个 DQS 角色的信息，请参阅 [DQS 安全](../../data-quality-services/dqs-security.md)。  
  
     有关详细信息，请参阅 [将 DQS 角色授予用户](../../data-quality-services/install-windows/grant-dqs-roles-to-users.md).  
  
    > [!NOTE]  
    >  这三个 DQS 角色不适用于 DQS_PROJECTS 和 DQS_STAGING_DATA 数据库。  
  
3.  使数据可供 DQS 操作使用。 确保您可以访问用于 DQS 操作的源数据，并且可以将已处理的数据导出到数据库中的某个表。  
  
     有关详细信息，请参阅  
                    [访问 DQS 操作的数据](../../data-quality-services/install-windows/access-data-for-the-dqs-operations.md)。  
  
## <a name="see-also"></a>另请参阅  
 [视频：安装和配置 DQS](/previous-versions/dn912438(v=msdn.10))   
 [.NET Framework 更新后升级 SQLCLR 程序集](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)   
 [使用 DQSInstaller.exe导出和导入 DQS 知识库 ](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)   
 [升级 Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md)   
 [删除 Data Quality Server 对象](../../sql-server/install/remove-data-quality-server-objects.md)   
 [安装 SQL Server 商业智能功能](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
 [卸载 SQL Server](../../sql-server/install/uninstall-sql-server.md)   
 [Data Quality Services](../../data-quality-services/data-quality-services.md)   
 [解决 DQS 中的安装和配置问题](https://social.technet.microsoft.com/wiki/contents/articles/3776.aspx)  
  
