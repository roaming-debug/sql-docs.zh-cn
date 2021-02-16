---
description: SSIS 目录
title: SSIS 目录 | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.iscreatecatalog.f1
- sql13.ssis.ssms.iscatalogprop.general.f1
- sql13.ssis.dbupgradewizard.f1
ms.assetid: 24bd987e-164a-48fd-b4f2-cbe16a3cd95e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fa8c9cb8d507acd5766e441cbd7387161a283bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339295"
---
# <a name="ssis-catalog"></a>SSIS 目录

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  SSISDB 目录是使用已部署到 [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] 服务器的 [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] (SSIS) 项目的中心点。 例如，您可以设置项目和包参数，配置环境以便为包指定运行时值，执行包并对包进行故障排除，以及管理 [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] 服务器操作。  
 
> [!NOTE]
> 本文介绍常规 SSIS 目录及在本地运行的 SSIS 目录。 还可在 Azure SQL 数据库中创建 SSIS 目录，并在 Azure 中部署和运行 SSIS 包。 有关详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。
>
> 虽然也可在 Linux 上运行 SSIS 包，但在 Linux 上不支持 SSIS 目录。 有关详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../../linux/sql-server-linux-migrate-ssis.md)。
 
 **SSISDB** 目录中存储的对象包括项目、包、参数、环境和操作历史记录。  
  
 可通过查询 **“SSISDB”** 数据库中的视图来检查 **“SSISDB”** 目录中存储的对象、设置和操作数据。 可通过调用 **SSISDB** 数据库中的存储过程或通过使用 **SSISDB** 目录的 UI 来管理对象。 在很多情况下，同一个任务既可使用 UI 执行，也可以通过调用存储过程来执行。  
  
 要维护 **SSISDB** 数据库，建议您应用管理用户数据库的标准企业策略。 有关创建维护计划的信息，请参阅 [Maintenance Plans](../../relational-databases/maintenance-plans/maintenance-plans.md)。  
  
 **SSISDB** 目录和 **SSISDB** 数据库支持 Windows PowerShell。 有关将 SQL Server 与 Windows PowerShell 一起使用的详细信息，请参阅 [SQL Server PowerShell](../../powershell/sql-server-powershell.md)。 有关如何使用 Windows PowerShell 完成任务（如部署项目）的示例，请参阅 blogs.msdn.com 上的博客文章 [SQL Server 2012 中的 SSIS 和 PowerShell](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015)。  
  
 有关如何查看操作数据的详细信息，请参阅 [监视运行包和其他操作](../../integration-services/performance/monitor-running-packages-and-other-operations.md)。  
  
 您可以通过以下方式访问 **中的** “SSISDB” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 目录：连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎，然后展开对象资源管理器中的 **“Integration Services 目录”** 节点。 可通过展开对象资源管理器中的“数据库”节点来访问 **中的** SSISDB [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 数据库。  
  
> [!NOTE]
> 不能重命名 **SSISDB** 数据库。  
  
> [!NOTE]
> 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SSISDB **数据库附加到的** 实例停止或不响应，则 ISServerExec.exe 进程结束。 向 Windows 事件日志写入一条消息。  
>   
>  如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源在群集故障转移过程中进行故障转移，正在运行的包不会重启。 您可以使用检查点来重新启动包。 有关详细信息，请参阅 [通过使用检查点重新启动包](../../integration-services/packages/restart-packages-by-using-checkpoints.md)。  
  
## <a name="features-and-capabilities"></a>特性和功能  
  
-   [目录对象标识符](../../integration-services/catalog/ssis-catalog.md#CatalogObjectIdentifiers)  
  
-   [目录配置](../../integration-services/catalog/ssis-catalog.md#Configuration)  
  
-   [权限](../../integration-services/catalog/ssis-catalog.md#Permissions)  
  
-   [文件夹](../../integration-services/catalog/ssis-catalog.md#Folders)  
  
-   [项目和包](../../integration-services/catalog/ssis-catalog.md#ProjectsAndPackages)  
  
-   [参数](../../integration-services/catalog/ssis-catalog.md#Parameters)  
  
-   [服务器环境、服务器变量和服务器环境引用](../../integration-services/catalog/ssis-catalog.md#ServerEnvironments)  
  
-   [执行和验证](../../integration-services/catalog/ssis-catalog.md#Executions)  

##  <a name="catalog-object-identifiers"></a><a name="CatalogObjectIdentifiers"></a> 目录对象标识符  
 在目录中创建新对象时，为该对象指定一个名称。 对象名称就是一个标识符。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 定义了有关可在标识符中使用的字符的规则。 以下对象的名称必须遵循标识符规则。  
  
-   Folder  
  
-   Project  
  
-   环境  
  
-   参数  
  
-   环境变量  
  
###  <a name="folder-project-environment"></a><a name="Folder"></a> 文件夹、项目和环境  
 重命名文件夹、项目或环境时，请考虑以下规则。  
  
-   无效字符包括 ASCII/Unicode 字符 1 到 31、引号 (")、小于号 (\<), greater than (>)、竖线 (|)、退格符 (\b)、null (\0) 和制表符 (\t)。  
  
-   名称不得包含前导空格或尾随空格。  
  
-   首字母不得为 \@，但后续字符可以是 \@。  
  
-   名称的长度必须大于 0 且小于或等于 128。  
  
###  <a name="parameter"></a><a name="Parameter"></a> 参数  
 命名参数时，请考虑以下规则。  
  
-   名称的第一个字符必须是在 Unicode 标准 2.0 中定义的字母，或者是下划线 (_)。  
  
-   后续字符可以是在 Unicode 标准 2.0 中定义的字母或数字，或是下划线 (_)。  
  
###  <a name="environment-variable"></a><a name="EnvironmentVariable"></a> 环境变量  
 命名环境变量时，请考虑以下规则。  
  
-   无效字符包括 ASCII/Unicode 字符 1 到 31、引号 (")、小于号 (\<), greater than (>)、竖线 (|)、退格符 (\b)、null (\0) 和制表符 (\t)。  
  
-   名称不得包含前导空格或尾随空格。  
  
-   首字母不得为 \@，但后续字符可以是 \@。  
  
-   名称的长度必须大于 0 且小于或等于 128。  
  
-   名称的第一个字符必须是在 Unicode 标准 2.0 中定义的字母，或者是下划线 (_)。  
  
-   后续字符可以是在 Unicode 标准 2.0 中定义的字母或数字，或是下划线 (_)。  
  
##  <a name="catalog-configuration"></a><a name="Configuration"></a> 目录配置  
 通过调整目录属性来优化目录的行为方式。 目录属性定义如何对敏感数据进行加密，以及如何保留操作和项目版本控制数据。 若要设置目录属性，请使用“目录属性”对话框，或调用 [catalog.configure_catalog（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md)存储过程。 若要查看属性，请使用对话框或查询 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)。 可以通过右键单击对象资源管理器中的“SSISDB”来访问该对话框。  
  
###  <a name="operations-and-project-version-cleanup"></a><a name="Cleanup"></a> 操作和项目版本清理  
 目录中很多操作的状态数据都存储在内部数据库表中。 例如，目录会跟踪包执行和项目部署的状态。 为了维持操作数据的大小，使用 **中的** “SSIS Server 维护作业” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 来删除旧数据。 在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时创建此 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 代理作业。  
  
 您可以使用相同名称将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目部署到目录中的同一文件夹，以对其进行更新或重新部署。 默认情况下，每次重新部署某个项目时， **SSISDB** 目录都会保留早期版本的该项目。 为了维持操作数据的大小，使用了 **“SSIS 服务器维护作业”** 来删除旧版本的项目。  
 
为了运行“SSIS 服务器维护作业” ，SSIS 创建了 SQL Server 登录 **##MS_SSISServerCleanupJobLogin##** 。 此登录仅供 SSIS 进行内部使用。
  
 以下 **SSISDB** 目录属性将定义此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的行为方式。 可以使用“目录属性”对话框或使用 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)和 [catalog.configure_catalog（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md)查看和修改属性。  
  
 **定期清理日志**  
 当此属性设置为 **True** 时，操作清除作业步骤将会运行。  
  
 **保持期(天)**  
 定义可允许的操作数据的最长保存时间（以天为单位）。 将删除较旧的数据。  
  
 最小值为一天。 最大值仅受到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] int 数据的最大值的限制。 有关此数据类型的信息，请参阅 [int、bigint、smallint 和 tinyint (Transact-SQL)](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)。  
  
 **定期删除旧版本**  
 当此属性设置为 **True** 时，项目版本清除作业步骤将会运行。  
  
 **每个项目的最大版本数**  
 定义在目录中存储项目的多少个版本。 将删除较旧版本的项目。  
  
###  <a name="encryption-algorithm"></a><a name="Encryption"></a> 加密算法  
 **“加密算法”** 属性可指定用于对敏感参数值进行加密的加密类型。 可以从下列加密类型中选择。  
  
-   AES_256（默认值）  
  
-   AES_192  
  
-   AES_128  
  
-   DESX  
  
-   TRIPLE_DES_3KEY  
  
-   TRIPLE_DES  
  
-   DES  
  
 在向 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器部署 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目时，该目录会自动对包数据和敏感值加密。 该目录还会在检索数据时自动解密数据。 SSISDB 目录使用 **ServerStorage** 保护级别。 有关详细信息，请参阅 [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md)。  
  
 更改加密算法是一项很耗时的操作。 首先，服务器必须使用以前指定的算法来解密所有配置值。 然后，服务器必须使用新算法来重新对这些值进行加密。 此时，在服务器上不能有其他 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 操作。 因此，为使 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 操作继续运行而不会中断，加密算法在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]的对话框中是只读值。  
  
 若要更改 **“加密算法”** 属性设置，请将 **“SSISDB”** 数据库设置为单用户模式，然后调用 catalog.configure_catalog 存储过程。 将 ENCRYPTION_ALGORITHM 用于 *property_name* 参数。 有关支持的属性值，请参阅 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)。 有关该存储过程的详细信息，请参阅 [catalog.configure_catalog（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md)。  
  
 有关单用户模式的详细信息，请参阅 [将数据库设置为单用户模式](../../relational-databases/databases/set-a-database-to-single-user-mode.md)。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中加密和加密算法的信息，请参阅 [SQL Server 加密](../../relational-databases/security/encryption/sql-server-encryption.md)一节中的有关主题。  
  
 数据库主密钥用于加密。 创建目录时会创建此密钥。  
  
 下表列出了 **“目录属性”** 对话框中显示的属性名称和数据库视图中对应的属性。  
  
|属性名称（ **“目录属性”** 对话框）|属性名称（数据库视图）|  
|---------------------------------------------------------|-------------------------------------|  
|加密算法名称|ENCRYPTION_ALGORITHM|  
|定期清理日志|OPERATION_CLEANUP_ENABLEDâ€‹|  
|保持期(天)|RETENTION_WINDOW|  
|定期删除旧版本|VERSION_CLEANUP_ENABLED|  
|每个项目的最大版本数|MAX_PROJECT_VERSIONS|  
|服务器范围的默认日志记录级别|SERVER_LOGGING_LEVEL|  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 文件夹中包含的项目、环境和包是安全对象。 您可以授予对文件夹的权限，包括 MANAGE_OBJECT_PERMISSIONS 权限。 利用 MANAGE_OBJECT_PERMISSIONS，您可以将文件夹内容的管理委托给用户，而无需为 ssis_admin 角色授予用户成员身份。 您还可以授予对项目、环境和操作的权限。 操作包括初始化 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]、部署项目、创建和启动执行、验证项目和包以及配置 **SSISDB** 目录。  
  
 有关数据库角色的详细信息，请参阅 [数据库级别的角色](../../relational-databases/security/authentication-access/database-level-roles.md)。  
  
 SSISDB 目录使用 DDL 触发器 (ddl_cleanup_object_permissions) 强制 SSIS 安全对象的权限信息的完整性。 当从 SSISDB 数据库中删除数据库主体（如数据库用户、数据库角色或数据库应用程序角色）时，将会触发触发器。  
  
 如果该主体已对其他主体授予或拒绝权限，则应先撤消授权者授予的权限，然后才能删除主体。 否则，系统尝试删除主体时会返回一条错误消息。 触发器将删除数据库主体作为被授权者的所有权限记录。  
  
 建议不要禁用触发器，因为它可确保在从 **SSISDB** 数据库中删除数据库主体之后，不会出现孤立的权限记录。  
  
### <a name="managing-permissions"></a>管理权限  
 可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 用户界面、存储过程以及 <xref:Microsoft.SqlServer.Management.IntegrationServices> 命名空间来管理权限。  
  
 若要使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] UI 管理权限，请使用以下对话框： 
  
-   对于一个文件夹中，使用 **权限** 页 [Folder Properties Dialog Box](../../integration-services/catalog/folder-properties-dialog-box.md)。  
  
-   对于项目，使用 **权限** 页 [Project Properties Dialog Box](../../integration-services/catalog/project-properties-dialog-box.md)。  

 若要使用 Transact-SQL 管理权限，请调用 [catalog.grant_permission（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-grant-permission-ssisdb-database.md)、[catalog.deny_permission（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-deny-permission-ssisdb-database.md)和 [catalog.revoke_permission（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-revoke-permission-ssisdb-database.md)。 若要查看所有对象的当前主体的有效权限，请查询 [catalog.effective_object_permissions（SSISDB 数据库）](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md)。 本主题描述了不同类型的权限。 若要查看已显式分配给用户的权限，请查询 [catalog.explicit_object_permissions（SSISDB 数据库）](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md)。  
  
##  <a name="folders"></a><a name="Folders"></a> 文件夹  
 文件夹包含 **SSISDB** 目录中的一个或多个项目和环境。 可以使用 [catalog.folders（SSISDB 数据库）](../../integration-services/system-views/catalog-folders-ssisdb-database.md) 视图来访问有关目录中的文件夹的信息。 可使用以下存储过程管理文件夹：  
  
-   [catalog.create_folder（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-folder-ssisdb-database.md)  
  
-   [catalog.delete_folder（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-folder-ssisdb-database.md)  
  
-   [catalog.rename_folder（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-rename-folder-ssisdb-database.md)  
  
-   [catalog.set_folder_description（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-folder-description-ssisdb-database.md)  
  
##  <a name="projects-and-packages"></a><a name="ProjectsAndPackages"></a> 项目和包  
 每个项目可以包含多个包。 项目和包都可以包含参数和对环境的引用。 您可以使用 [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md)访问参数和环境引用。  
  
 可通过调用以下存储过程来执行其他项目任务： 
  
-   [catalog.delete_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-project-ssisdb-database.md)  
  
-   [catalog.deploy_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)  
  
-   [catalog.get_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)  
  
-   [catalog.move_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-move-project-ssisdb-database.md)  
  
-   [catalog.restore_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md)  
  
 以下视图提供了有关包、项目和项目版本的详细信息。  
  
-   [catalog.projects（SSISDB 数据库）](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
  
-   [catalog.packages（SSISDB 数据库）](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
  
-   [catalog.object_versions（SSISDB 数据库）](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameters  
 您可以使用参数在包执行时为包属性赋值。 若要设置包或项目参数的值和清除这些值，请调用 [catalog.set_object_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md)和 [catalog.clear_object_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md)。 若要为执行实例设置参数的值，请调用 [catalog.set_execution_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)。 可以通过调用 [catalog.get_parameter_values（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md)来检索默认参数值。  
  
 以下视图显示了所有包和项目的参数，以及用于执行实例的参数值。  
  
-   [catalog.object_parameters（SSISDB 数据库）](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
  
-   [catalog.execution_parameter_values（SSISDB 数据库）](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
  
##  <a name="server-environments-server-variables-and-server-environment-references"></a><a name="ServerEnvironments"></a> 服务器环境、服务器变量和服务器环境引用  
 服务器环境包含服务器变量。 当在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器上执行或验证包时，可以使用这些变量值。  
  
 利用以下存储过程，您可以为环境和变量执行很多其他管理任务。  
  
-   [catalog.create_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-environment-ssisdb-database.md)  
  
-   [catalog.delete_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-environment-ssisdb-database.md)  
  
-   [catalog.move_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-move-environment-ssisdb-database.md)  
  
-   [catalog.rename_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-rename-environment-ssisdb-database.md)  
  
-   [catalog.set_environment_property（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-property-ssisdb-database.md)  
  
-   [catalog.create_environment_variable（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-environment-variable-ssisdb-database.md)  
  
-   [catalog.delete_environment_variable（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-environment-variable-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_property（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-variable-property-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-variable-value-ssisdb-database.md)  
  
 通过调用 [catalog.set_environment_variable_protection（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-variable-protection-ssisdb-database.md)存储过程，可以设置变量的敏感性位。  
  
 若要使用服务器变量的值，请指定项目和服务器环境之间的引用。 您可以使用以下存储过程创建和删除引用。 还可以指示环境是可以位于与项目相同的文件夹中，还是位于其他文件夹中。  
  
-   [catalog.create_environment_reference（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-environment-reference-ssisdb-database.md)  
  
-   [catalog.delete_environment_reference（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-environment-reference-ssisdb-database.md)  
  
-   [catalog.set_environment_reference_type（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-reference-type-ssisdb-database.md)  
  
 有关环境和变量的详细信息，请查询以下视图。  
  
-   [catalog.environments（SSISDB 数据库）](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
  
-   [catalog.environment_variables（SSISDB 数据库）](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
  
-   [catalog.environment_references（SSISDB 数据库）](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
  
##  <a name="executions-and-validations"></a><a name="Executions"></a> 执行和验证  
 执行就是一个包执行实例。 调用 [catalog.create_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)和 [catalog.start_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)可创建并启动执行。 若要停止执行或停止包/项目验证，请调用 [catalog.stop_operation（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md)。  
  
 若要暂停正在运行的包和创建转储文件，请调用 catalog.create_execution_dump 存储过程。 转储文件提供了有关包执行的信息，可帮助您解决执行问题。 有关生成和配置转储文件的详细信息，请参阅 [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)。  
  
 有关执行、验证、操作期间记录的消息以及与错误有关的上下文信息的详细信息，请查询以下视图。  
  
-   [catalog.executions（SSISDB 数据库）](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
-   [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
  
-   [catalog.operation_messages（SSISDB 数据库）](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
  
-   [catalog.extended_operation_info（SSISDB 数据库）](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
  
-   [catalog.event_messages](../../integration-services/system-views/catalog-event-messages.md)  
  
-   [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
 可以通过调用 [catalog.validate_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-validate-project-ssisdb-database.md)和 [catalog.validate_package（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-validate-package-ssisdb-database.md)存储过程验证项目和包。 [catalog.validations（SSISDB 数据库）](../../integration-services/system-views/catalog-validations-ssisdb-database.md)视图提供了有关验证的详细信息，例如，验证中要考虑的服务器环境引用、验证是依赖项验证还是完整验证以及使用 32 位运行时还是 64 位运行时来运行包。  

## <a name="create-the-ssis-catalog"></a>创建 SSIS 目录
  当您在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中设计和测试包后，可将包含包的项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。 在可以将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器之前，该服务器必须包含 **SSISDB** 目录。 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 的安装程序并不会自动创建目录；您需要使用以下说明手动创建目录。  
  
 您可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中创建 SSISDB 目录。 还可以使用 Windows PowerShell 以编程方式创建目录。  
  
### <a name="to-create-the-ssisdb-catalog-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中创建 SSISDB  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
2.  连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎。  
  
3.  在“对象资源管理器”中，展开服务器节点，右键单击“Integration Services 目录”  节点，然后单击“创建目录” 。  
  
4.  单击 **“启用 CLR 集成”** 。  
  
     该目录使用 CLR 存储过程。  
  
5.  单击“在 SQL Server 启动时启用自动执行 Integration Services 存储过程”，使 [catalog.startup](../../integration-services/system-stored-procedures/catalog-startup.md) 存储过程在每次重启 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务器后运行。  
  
     该存储过程对 SSISDB 目录的操作状态进行维护。 它可以修复在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务器实例出现故障时运行的任何包的状态。  
  
6.  输入密码，然后单击 **“确定”** 。  
  
     该密码保护用于对目录数据进行加密的数据库主密钥。 将该密码保存在安全的位置。 同时建议您也备份数据库主密钥。 有关详细信息，请参阅 [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md)。  
  
### <a name="to-create-the-ssisdb-catalog-programmatically"></a>以编程方式创建 SSISDB 目录  
  
1.  执行以下 PowerShell 脚本：  
  
    ```  
    # Load the IntegrationServices Assembly  
    [Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices")  
  
    # Store the IntegrationServices Assembly namespace to avoid typing it every time  
    $ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"  
  
    Write-Host "Connecting to server ..."  
  
    # Create a connection to the server  
    $sqlConnectionString = "Data Source=localhost;Initial Catalog=master;Integrated Security=SSPI;"  
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString  
  
    # Create the Integration Services object  
    $integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection  
  
    # Provision a new SSIS Catalog  
    $catalog = New-Object $ISNamespace".Catalog" ($integrationServices, "SSISDB", "P@assword1")  
    $catalog.Create()  
  
    ```  
  
     有关如何使用 Windows PowerShell 和 <xref:Microsoft.SqlServer.Management.IntegrationServices> 命名空间的更多示例，请参阅 blogs.msdn.com 上的博客文章 [SQL Server 2012 中的 SSIS 和 PowerShell](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015)。 有关命名空间和代码示例的概述，请参阅 blogs.msdn.com 上的博客文章 [SSIS 目录托管对象模型一瞥](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)。  

## <a name="catalog-properties-dialog-box"></a>“目录属性”对话框
  使用“目录属性”对话框可以配置 SSISDB 目录。 目录属性定义如何对敏感数据进行加密、如何保留操作和项目版本控制数据以及验证操作何时超时。SSISDB 目录是用于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目、包、参数和环境的中心存储区和管理点。  
  
 还可以在 `catalog.catalog_properties` 视图中查看目录属性，并使用 `catalog.configure_catalog` 存储过程设置属性。 有关详细信息，请参阅 [catalog.catalog_properties（SSISDB 数据库）](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md)和 [catalog.configure_catalog（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md)。  
  
 **您希望做什么？**  
  
-   [打开“目录属性”对话框](#open_dialog)  
  
-   [配置选项](#options)  
  
###  <a name="open-the-catalog-properties-dialog-box"></a><a name="open_dialog"></a> 打开“目录属性”对话框  
  
1.  打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]。  
  
2.  连接到 Microsoft SQL Server 数据库引擎。  
  
3.  在对象资源管理器中，展开“Integration Services”节点，右键单击“SSISDB”，然后单击“属性”。  
  
###  <a name="configure-the-options"></a><a name="options"></a> 配置选项  
  
#### <a name="options"></a>选项  
 下表描述对话框中的某些属性以及 `catalog.catalog_properties` 视图中的相应属性。  
  
|属性名称（“目录属性”对话框）|属性名称（catalog.catalog_properties 视图）|说明|  
|-----------------------------------------------------|------------------------------------------------------|-----------------|  
|加密算法名称|ENCRYPTION_ALGORITHM|指定用于对于目录中的敏感参数值进行加密的加密类型。 下面是可能的值：<br /><br /> DES<br /><br /> TRIPLE_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESPX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256（默认值）|  
|每个项目的最大版本数|MAX_PROJECT_VERSIONS|指定可以在目录中存储项目的多少个版本。 当项目版本清理作业运行时，如果旧版项目数超过此上限，就会遭到删除。|  
|定期清理日志|OPERATION_CLEANUP_ENABLED|将该属性设置为 True 可指示 SQL Server 代理作业“操作清除”运行。 否则，将该属性设置为 False。|  
|保持期(天)|RETENTION_WINDOW|指定可允许操作数据的最长时间（天）。 指定天数前的数据由 SQL 代理作业“操作清理”删除。|  

## <a name="back-up-restore-and-move-the-ssis-catalog"></a>备份、还原和移动 SSIS 目录
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包含 SSISDB 数据库。 查询 SSISDB 数据库中的视图可以检查 **SSISDB** 目录中存储的对象、设置和操作数据。 本主题说明如何备份和还原该数据库。  
  
 SSISDB 目录存储部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的包。 有关该目录的详细信息，请参阅 [SSIS 目录](../../integration-services/catalog/ssis-catalog.md)。  
  
###  <a name="to-back-up-the-ssis-database"></a><a name="backup"></a> 备份 SSIS 数据库  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 并连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
2.  使用 BACKUP MASTER KEY Transact-SQL 语句备份 SSISDB 数据库的主密钥。 该密钥存储在您指定的文件中。 使用密码加密该文件中的主密钥。  
  
     有关语句的详细信息，请参阅 [BACKUP MASTER KEY (Transact-SQL)](../../t-sql/statements/backup-master-key-transact-sql.md)。  
  
     在下面的示例中，将主密钥导出到 `c:\temp directory\RCTestInstKey` 文件。 使用 `LS2Setup!` 密码加密主密钥。  
  
    ```  
    backup master key to file = 'c:\temp\RCTestInstKey'  
           encryption by password = 'LS2Setup!'  
  
    ```  
  
3.  在 **中使用** “备份数据库” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]对话框备份 SSISDB 数据库。 有关详细信息，请参阅[操作说明：备份数据库 (SQL Server Management Studio)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)。  
  
4.  通过执行以下操作，生成 ##MS_SSISServerCleanupJobLogin## 的 CREATE LOGIN 脚本。 有关详细信息，请参阅 [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md)。  
  
    1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的对象资源管理器中，展开 **“安全性”** 节点，然后展开 **“登录名”** 节点。  
  
    2.  右键单击 **##MS_SSISServerCleanupJobLogin##** ，然后依次单击“编写登录脚本为” > “CREATE 到” > “新查询编辑器窗口”。    
  
5.  若要将 SSISDB 数据库还原到从未创建 SSISDB 目录的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请执行以下操作，生成 sp_ssis_startup 的 CREATE PROCEDURE 脚本。 有关详细信息，请参阅 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)。  
  
    1.  在对象资源管理器中，展开“数据库”节点，然后展开“主” > “可编程性” > “存储过程”节点。     
  
    2.  右键单击“dbo.sp_ssis_startup”，再依次单击“将存储过程脚本编写为” > “CREATE，并编写到” > “新查询编辑器窗口”。  
  
6.  确认 SQL Server 代理已启动  
  
7.  若要将 SSISDB 数据库还原到从未创建 SSISDB 目录的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请执行以下操作，生成 SSIS 服务器维护作业的脚本。 创建 SSISDB 目录时自动在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理中创建该脚本。 该作业帮助清除保留期窗口之外的操作日志并删除较旧版本的项目。  
  
    1.  在对象资源管理器中，展开 **“SQL Server 代理”** 节点，然后展开 **“作业”** 节点。  
  
    2.  右键单击“SSIS 服务器维护作业”，再依次单击“将存储过程脚本编写为” > “CREATE，并编写到” > “新查询编辑器窗口”。  
  
### <a name="to-restore-the-ssis-database"></a>还原 SSIS 数据库  
  
1.  如果要将 SSISDB 数据库还原到从不创建 SSISDB 目录的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请通过运行 `sp_configure` 存储过程来启用公共语言运行时 (clr)。 有关详细信息，请参阅 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 和 [clr enabled 选项](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)。  
  
    ```  
    use master   
           sp_configure 'clr enabled', 1  
           reconfigure  
  
    ```  
  
2.  如果要将 SSISDB 数据库还原到从不创建 SSISDB 目录的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，请创建非对称密钥和对应非对称密钥的登录名并将 UNSAFE 权限授予该登录名。  

    ```  
    Create Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey  
           FROM Executable File = 'C:\Program Files\Microsoft SQL Server\YourSQLServerDefaultCompatibilityLevel\DTS\Binn\Microsoft.SqlServer.IntegrationServices.Server.dll'  
    ```  

    可以从 [SQL Server 默认兼容级别的列表](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments)中找到 `YourSQLServerDefaultCompatibilityLevel` 的值。
  
    [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] CLR 存储过程要求将 UNSAFE 权限授予该登录名，因为该登录名需要对受限制资源（如 Microsoft Win32 API）的更高访问权限。 有关 UNSAFE 代码权限的详细信息，请参阅 [Creating an Assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md)。  

    ```  
    Create Login ##MS_SQLEnableSystemAssemblyLoadingUser## FROM Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey   
    Grant Unsafe Assembly to ##MS_SQLEnableSystemAssemblyLoadingUser##    
    ```  
  
3.  在 **中使用** “还原数据库” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]对话框从备份中还原 SSISDB 数据库。 有关详情，请参阅以下主题：  
  
    -   [还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
    -   [还原数据库（“文件”页）](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
    -   [还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
4.  执行你在 [备份 SSIS 数据库](#backup) 中为 ##MS_SSISServerCleanupJobLogin##、sp_ssis_startup 和 SSIS 服务器维护作业创建的脚本。 确认 SQL Server 代理已启动。  
  
5.  运行以下语句，将 sp_ssis_startup 过程设置为自动执行。 有关详细信息，请参阅 [sp_procoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)。  
  
    ```  
    EXEC sp_procoption N'sp_ssis_startup','startup','on'  
    ```  
  
6.  通过在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中使用“登录属性”对话框，将 SSISDB 用户 ##MS_SSISServerCleanupJobUser##（SSISDB 数据库）映射到 ##MS_SSISServerCleanupJobLogin##。  
  
7.  使用下列方法之一还原主密钥。 有关加密的详细信息，请参阅 [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md)。  
  
    -   **方法 1**  
  
         如果已备份数据库主密钥且具有用于加密主密钥的密码，则使用此方法。  
  
        ```  
               Restore master key from file = 'c:\temp\RCTestInstKey'  
               Decryption by password = 'LS2Setup!' -- 'Password used to encrypt the master key during SSISDB backup'  
               Encryption by password = 'LS3Setup!' -- 'New Password'  
               Force  
  
        ```  
  
        > [!NOTE]  
        >  确认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户有权读取备份密钥文件。  
  
        > [!NOTE]  
        >  如果服务主密钥尚未加密数据库主密钥，[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中便会显示以下警告消息。 忽略警告消息。  
        >   
        >  **无法解密当前主密钥。此错误已被忽略，因为指定了 FORCE 选项。**  
        >   
        >  FORCE 参数指定即使当前数据库主密钥未打开，也应继续执行还原过程。 对于 SSISDB 目录，由于要在其中还原数据库的实例尚未启用数据库主密钥，因此会看到这条消息。  
  
    -   **方法 2**  
  
         如果您具有用于创建 SSISDB 的原始密码，则使用此方法。  
  
        ```  
        open master key decryption by password = 'LS1Setup!' --'Password used when creating SSISDB'  
               Alter Master Key Add encryption by Service Master Key  
        ```  
  
8.  通过运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] catalog.check_schema_version [，确定 SSISDB 目录架构与](../../integration-services/system-stored-procedures/catalog-check-schema-version.md)二进制文件（ISServerExec 和 SQLCLR 程序集）是否兼容。  
  
9. 若要确认 SSISDB 数据库已成功还原，请针对 SSISDB 目录执行操作，如运行部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的包。 有关详细信息，请参阅[运行 Integration Services (SSIS) 包](../../integration-services/packages/run-integration-services-ssis-packages.md)。  
  
### <a name="to-move-the-ssis-database"></a>移动 SSIS 数据库  
  
-   按移动用户数据库的说明操作。 有关详细信息，请参阅 [Move User Databases](../../relational-databases/databases/move-user-databases.md)。  
  
     确保您备份 SSISDB 数据库的主密钥并保护备份文件。 有关详细信息，请参阅 [备份 SSIS 数据库](#backup)。  
  
     确保在尚未创建 SSISDB 目录的新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中创建 Integration Services (SSIS) 相关对象。  

## <a name="upgrade-the-ssis-catalog-ssisdb"></a>升级 SSIS 目录 (SSISDB)
  当数据库早于 SQL Server 实例当前版本时，运行 SSISDB 升级向导来升级 SSIS 目录数据库 SSISDB。 如果符合下列任一条件，表明可能是旧数据库。  
  
-   从旧版 SQL Server 还原数据库。  
  
-   在升级 SQL Server 实例之前，未从 AlwaysOn 可用性组中删除数据库。 此条件可以防止数据库自动升级。 有关详细信息，请参阅 [Upgrading SSISDB in an availability group](#Upgrade)。  
  
 该向导只能升级本地服务器实例上的数据库。  
  
### <a name="upgrade-the-ssis-catalog-ssisdb-by-running-the-ssisdb-upgrade-wizard"></a>通过运行 SSISDB 升级向导升级 SSIS 目录 (SSISDB)  
  
1.  备份 SSIS 目录数据库 (SSISDB)。  
  
2.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，展开本地服务器，然后展开“Integration Services 目录” 。  
  
3.  右键单击“SSISDB”，然后选择“数据库升级”以启动 SSISDB 升级向导。 或者通过使用提升的权限在本地服务器上运行 `C:\Program Files\Microsoft SQL Server\140\DTS\Binn\ISDBUpgradeWizard.exe` 来启动 SSISDB 升级向导。
  
     ![启动 SSISDB 升级向导](../../integration-services/service/media/ssisdb-upgrade-wizard-1.png)

4.  在“选择实例”  页上，选择本地服务器上的 SQL Server 实例。  
  
    > [!IMPORTANT]  
    >  该向导只能升级本地服务器实例上的数据库。  
  
     选中复选框以指示，你已在运行此向导之前备份 SSISDB 数据库。  
  
     ![在 SSISDB 升级向导中选择服务器](../../integration-services/service/media/ssisdb-upgrade-wizard-2.png "在 SSISDB 升级向导中选择服务器")  
  
5.  选择“升级”  以升级 SSIS 目录数据库。  
  
6.  在“结果”  页上，查看结果。  
  
     ![查看 SSISDB 升级向导中的结果](../../integration-services/service/media/ssisdb-upgrade-wizard-3.png "查看 SSISDB 升级向导中的结果")  

## <a name="always-on-for-ssis-catalog-ssisdb"></a>对 SSIS 目录 (SSISDB) 使用 Always On
  AlwaysOn 可用性组功能是一个高可用性和灾难恢复解决方案，可以提供替代数据库镜像的企业级方案。 可用性组针对一组离散的用户数据库（称为可用性数据库，它们共同实现故障转移）支持故障转移环境。 有关详细信息，请参阅 [AlwaysOn 可用性组](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)。  
  
 为了对 SSIS 目录 (SSISDB) 及其内容（项目、包、执行日志等）提供高可用性，可以将 SSISDB 数据库（与其他任何用户数据库相同）添加到 AlwaysOn 可用性组。 当发生故障转移时，其中一个辅助节点将自动成为新的主节点。  
 
 > [!IMPORTANT]
 > 发生故障转移时，正在运行的包不会重启或恢复。 
 
 **本节内容：**  
  
1.  [先决条件](#prereq)  
  
2.  [为 AlwaysOn 配置 SSIS 支持](#Firsttime)  
  
3.  [在可用性组中升级 SSISDB](#Upgrade)  
  
###  <a name="prerequisites"></a><a name="prereq"></a>先决条件  
为 SSISDB 数据库启用 Always On 支持前，请先执行以下先决性步骤。  
  
1.  设置 Windows 故障转移群集。 请参阅 [安装适用于 Windows Server 2012 的故障转移群集功能和工具](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) 的博客文章以获取相关说明。 在所有群集节点上安装功能和工具。  
  
2.  在群集的每个节点上安装具有 Integration Services (SSIS) 功能的 SQL Server 2016。  
  
3.  为每个 SQL Server 实例启用 Always On 可用性组。 有关详细信息，请参阅 [启用 AlwaysOn 可用性组](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md) 。  
  
###  <a name="configure-ssis-support-for-always-on"></a><a name="Firsttime"></a> 为 AlwaysOn 配置 SSIS 支持  
  
-   [步骤 1：创建 Integration Services 目录](#Step1)  
  
-   [步骤 2：将 SSISDB 添加到 AlwaysOn 可用性组](#Step2)  
  
-   [步骤 3：为 AlwaysOn 启用 SSIS 支持](#Step3)  
  
> [!IMPORTANT]  
> -   必须在可用性组的 **主节点** 上执行这些步骤。
> -   将 SSISDB 添加到 Always On 可用性组后，必须启用“Always On 的 SSIS 支持”。  

> [!NOTE]
> 若要详细了解此过程，请参阅数据平台 MVP Marcos Freccia 发布的以下演练及附加屏幕截图：[将 SSISDB 添加到 SQL Server 2016 的 AG](https://marcosfreccia.com/2017/04/28/adding-ssisdb-to-ag-for-sql-server-2016/)。

####  <a name="step-1-create-integration-services-catalog"></a><a name="Step1"></a> 步骤 1：创建 Integration Services 目录  
  
1.  启动 **SQL Server Management Studio** 并连接到你想要设置为适用于 SSISDB 的 AlwaysOn 高可用性组的 **主节点** 的群集中的 SQL Server 实例。  
  
2.  在“对象资源管理器”中，展开服务器节点，右键单击“Integration Services 目录”  节点，然后单击“创建目录” 。  
  
3.  单击 **“启用 CLR 集成”** 。 该目录使用 CLR 存储过程。  
  
4.  单击“在 SQL Server 启动时启用自动执行 Integration Services 存储过程”  ，使 [catalog.startup](../system-stored-procedures/catalog-startup.md) 存储过程在每次重启 SSIS 服务器后运行。 该存储过程对 SSISDB 目录的操作状态进行维护。 它可以修复当 SSIS 服务器实例出现故障时正在运行的任何包的状态。  
  
5.  输入 **密码**，然后单击“确定” 。 该密码保护用于对目录数据进行加密的数据库主密钥。 将该密码保存在安全的位置。 同时建议您也备份数据库主密钥。 有关详细信息，请参阅 [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md)。  
  
####  <a name="step-2-add-ssisdb-to-an-always-on-availability-group"></a><a name="Step2"></a> 步骤 2：将 SSISDB 添加到 AlwaysOn 可用性组  
将 SSISDB 数据库添加到 AlwaysOn 可用性组的方法与将任何其他用户数据库添加到可用性组的方法几乎相同。 请参阅 [使用可用性组向导](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)。  
  
提供在“新建可用性组”向导的“选择数据库”页中创建 SSIS 目录时指定的密码。

![新建可用性组](../../integration-services/service/media/ssis-newavailabilitygroup.png "新建可用性组")  
  
####  <a name="step-3-enable-ssis-support-for-always-on"></a><a name="Step3"></a> 步骤 3：为 AlwaysOn 启用 SSIS 支持  
 创建 Integration Service 目录后，右键单击“Integration Service 目录”节点，再单击“启用 Always On 支持”。 应该能看到以下“为 AlwaysOn 启用支持”  的对话框。 如果此菜单被禁用，请确认已安装所有必备组件，然后单击“刷新” 。  
  
 ![为 Always On 启用支持](../../integration-services/service/media/ssis-enablesupportforalwayson.png)  
  
> [!WARNING]  
>  为 AlwaysOn 启用 SSIS 支持之前，不支持 SSISDB 数据库的自动故障转移。  
  
 表中列出了从 Always On 可用性组新添加的次要副本。 单击列表中每个副本的“连接…”按钮，然后输入身份验证凭据以连接到副本。 用户帐户必须是每个副本上 sysadmin 组的成员，才能为 Always On 启用 SSIS 支持。 成功连接到每个副本后，单击“确定”  按钮以启用对 AlwaysOn 的 SSIS 支持。  
 
如果在完成其他先决性步骤后，关联菜单中的“启用 Always On 支持”选项显示为已禁用，请尝试执行以下操作：
1.  单击“刷新”选项，刷新关联菜单。
2.  确保连接到主节点。 必须在主节点上启用 Always On 支持。
3.  确保 SQL Server 版本为 13.0 或更高版本。 仅在 SQL Server 2016 及更高版本上，SSIS 才支持 Always On。

###  <a name="upgrading-ssisdb-in-an-availability-group"></a><a name="Upgrade"></a> 在可用性组中升级 SSISDB  
 如果正在从以前的版本升级 SQL Server，并且 SSISDB 位于 AlwaysOn 可用性组中，则“AlwaysOn 可用性组中的 SSISDB 检查”规则可能会阻止你的升级。 出现这种阻止是因为升级是以单用户模式运行的，而可用性数据库必须是多用户数据库。 因此，在升级或修补过程中，所有可用性数据库（包括 SSISDB）均将处于脱机状态，不会进行升级或修补。 为了让升级继续进行下去，应先从可用性组中删除 SSISDB，升级或修补每一节点，再将 SSISDB 添加回可用性组中。  
  
 如果被“Always On 可用性组中的 SSISDB 检查”规则阻止，请按照以下步骤操作，升级 SQL Server。  
  
1.  从可用性组中删除 SSISDB 数据库。 有关详细信息，请参阅[将辅助数据库从可用性组删除 (SQL Server)](../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md) 和[将主数据库从可用性组删除 (SQL Server)](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)。  
  
2.  在升级向导中，单击“重新运行”。 此时，将会通过“Always On 可用性组中的 SSISDB 检查”规则。  
  
3.  单击“下一步”  以继续升级。  
  
4.  升级所有节点后，将 SSISDB 数据库添加回 AlwaysOn 可用性组中。 有关详细信息，请参阅[将数据库添加到可用性组 (SQL Server)](../../database-engine/availability-groups/windows/availability-group-add-a-database.md)。  
  
 如果升级 SQL Server 时没有受到阻止，并且 SSISDB 位于 Always On 可用性组中，请在升级 SQL Server 数据库引擎后单独升级 SSISDB。 按照以下过程中所述，使用 SSIS 升级向导升级 SSISDB。  
  
1.  将 SSISDB 数据库移出可用性组，或者如果 SSISDB 是可用性组中唯一的数据库，则删除可用性组。 在可用性组的主节点上，启动 SQL Server Management Studio 来执行此任务。  
  
2.  从所有 **副本节点** 删除 SSISDB 数据库。  
  
3.  在 **主节点** 上升级 SSISDB 数据库。 在 SQL Server Management Studio 内的“对象资源管理器” 中，展开“Integration Services 目录” ，右键单击“SSISDB” ，然后选择“数据库升级” 。 按照“SSISDB 升级向导”  中的说明来升级数据库。 在主节点上，本地启动 SSIDB 升级向导。  
  
4.  按照[步骤 2：将 SSISDB 添加到 AlwaysOn 可用性组](#Step2)中的说明将 SSISDB 添加回可用性组。  
  
5.  按照[步骤 3：为 AlwaysOn 启用 SSIS 支持](#Step3)中的说明进行操作。  


## <a name="ssisdb-catalog-and-delegation-in-double-hop-scenarios"></a>双跃点方案中的 SSISDB 目录和委派

默认情况下，针对 SSISDB 目录下存储的 SSIS 包的远程调用不支持凭据委派（这有时也称为双跃点）。 

假设有这样一种情况：用户登录客户端计算机 A 并启动 SQL Server Management Studio (SSMS)。 在 SSMS 中，用户连接到计算机 B 上托管的 SQL Server，该计算机具有 SSISDB 目录。 SSIS 包存储在此 SSISDB 目录下，并且该包又连接到在计算机 C 上运行的 SQL Server 服务（该包也可访问其他任何服务）。 当用户从计算机 A 调用 SSIS 包的执行时，SSMS 会先将用户凭据从计算机 A 成功传递到计算机 B（SSIS 运行时进程正在执行该包）。 要成功完成执行，现在需要 SSIS 执行运行时进程 (ISServerExec.exe) 才能将用户凭据从计算机 B 委派给计算机 C。 但是，默认情况下不启用凭据委派。

用户可通过将“信任此用户对任何服务的委派(仅 Kerberos)”的权限授予计算机B 上的 SQL Server 服务帐户（它将 ISServerExec.exe 作为子进程启动），启用凭据委派。 此过程是指针对 SQL Server 服务帐户设置无约束委派或开放式委派。 在授予此权限之前，请考虑它是否符合组织的安全要求。

SSISDB 不支持约束委派。 在双跃点环境中，如果为约束委派设置了托管 SSISDB 目录的 SQL Server 服务帐户（在本例中为计算机 B），则 ISServerExec.exe 将无法将凭据委派给第三台计算机（计算机 C）。 这适用于启用 Windows Credential Guard 的情况，该情况强制要求设置约束委派。

  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   blogs.msdn.com 上的博客文章 [SQL Server 2012 中的 SSIS 和 PowerShell](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015)。  
  
-   blogs.msdn.com 上的博客文章 [SSIS 目录访问控制提示](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-catalog-access-control-tips/ba-p/388057)。  
  
-   blogs.msdn.com 上的博客文章： [SSIS 目录托管对象模型一瞥](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)。