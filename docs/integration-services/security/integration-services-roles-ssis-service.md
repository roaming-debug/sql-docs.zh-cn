---
description: Integration Services 角色（SSIS 服务）
title: Integration Services 角色（SSIS 服务）| Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtsserver.packageroles.f1
helpviewer_keywords:
- security [Integration Services], roles
- db_ssisoperator role
- db_ssisadmin role
- fixed database roles [Integration Services]
- packages [Integration Services], security
- roles [Integration Services]
- db_ssisltduser role
ms.assetid: 9702e90c-fada-4978-a473-1b1423017d80
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8801c1a4f7387f1ff5bc815b45897d5a4f68fdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340780"
---
# <a name="integration-services-roles-ssis-service"></a>Integration Services 角色（SSIS 服务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了某些固定数据库级角色，以帮助安全访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中存储的包。 可用的角色有所不同，具体取决于是将包保存在 SSIS 目录数据库 (SSISDB) 中还是 msdb 数据库中。  
  
## <a name="roles-in-the-ssis-catalog-database-ssisdb"></a>SSIS 目录数据库 (SSISDB) 中的角色  
 SSIS 目录数据库 (SSISDB) 提供了以下固定数据库级角色，以帮助安全访问包和有关包的信息。  
  
-   **ssis_admin**。 此角色提供对 SSIS 目录数据库的完全管理访问权限。  
  
-   **ssis_logreader** 此角色提供访问所有与 SSISDB 操作日志相关的视图的权限。  
  
     视图的列表包括：[catalog].[projects]、[catalog].[packages]、[catalog].[operations]、[catalog].[extended_operation_info]、[catalog].[operation_messages]、[catalog].[event_messages]、[catalog].[execution_data_statistics]、[catalog].[execution_component_phases]、[catalog].[execution_data_taps]、[catalog].[event_message_context]、[catalog].[executions]、[catalog].[executables]、[catalog].[executable_statistics]、[catalog].[validations]、[catalog].[execution_parameter_values] 和 [catalog].[execution_property_override_values]。  
  
## <a name="roles-in-the-msdb-database"></a>msdb 数据库中的角色  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包括三个固定数据库级角色（db_ssisadmin、db_ssisltduser 和 db_ssisoperator），用于控制对保存到 msdb 数据库的包的访问。 可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]将角色分配给包。 角色分配保存到 **msdb** 数据库中。  
  
### <a name="read-and-write-actions"></a>读取和写入操作  
 下表介绍了 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中 Windows 和固定数据库级角色的读写操作。  
  
|角色|读操作|写操作|  
|----------|-----------------|------------------|  
|**db_ssisadmin**<br /><br /> 或<br /><br /> **sysadmin**|枚举自己的包。<br /><br /> 枚举所有包。<br /><br /> 查看自己的包。<br /><br /> 查看所有包。<br /><br /> 执行自己的包。<br /><br /> 执行所有包。<br /><br /> 导出自己的包。<br /><br /> 导出所有包。<br /><br /> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理中执行所有包。|导入包。<br /><br /> 删除自己的包。<br /><br /> 删除所有包。<br /><br /> 更改自己的包角色。<br /><br /> 更改所有包角色。<br /><br /> <br /><br /> **\*\* 警告 \*\*** db_ssisadmin 角色和 dc_admin 角色的成员可以将其特权提升为 sysadmin。 因为这些角色可以修改 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包，而 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的 sysadmin 安全上下文可以执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包，所以可以实现特权提升。 若要在运行维护计划、数据收集组和其他 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时防止此权限提升，请将运行包的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业配置为使用具有有限权限的代理帐户，或只将 sysadmin 成员添加到 db_ssisadmin 和 dc_admin 角色。|  
|**db_ssisltduser**|枚举自己的包。<br /><br /> 枚举所有包。<br /><br /> 查看自己的包。<br /><br /> 执行自己的包。<br /><br /> 导出自己的包。|导入包。<br /><br /> 删除自己的包。<br /><br /> 更改自己的包角色。|  
|**db_ssisoperator**|枚举所有包。<br /><br /> 查看所有包。<br /><br /> 执行所有包。<br /><br /> 导出所有包。<br /><br /> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理中执行所有包。|无|  
|**Windows 管理员**|查看所有正在运行的包的详细执行信息。|停止所有当前正在运行的包。|  
  
### <a name="sysssispackages-table"></a>Sysssispackages 表  
 **msdb** 中的 **sysssispackages** 表包含保存到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的包。 有关详细信息，请参阅 [sysssispackages (Transact-SQL)](../../relational-databases/system-tables/sysssispackages-transact-sql.md)。  
  
 **sysssispackages** 表的列中包含关于分配给包的角色的信息。  
  
-   **readerrole** 列指定对包具有读访问权限的角色。  
  
-   **writerrole** 列指定对包具有写访问权限的角色。  
  
-   **ownersid** 列包含创建包的用户的唯一安全标识符。 此列定义包的所有者。  
  
### <a name="permissions"></a>权限  
 默认情况下， **db_ssisadmin** 和 **db_ssisoperator** 固定数据库级角色的权限以及创建包的用户的唯一安全标识符适用于包的读取者角色，而 **db_ssisadmin** 角色的权限以及创建包的用户的唯一安全标识符适用于写入者角色。 用户必须是 **db_ssisadmin**、 **db_ssisltduser** 或 **db_ssisoperator** 角色的成员，才能拥有对包的读访问权限。 用户必须是 **db_ssisadmin** 角色的成员，才能拥有写访问权限。  
  
### <a name="access-to-packages"></a>对包的访问  
 固定数据库级角色可以与用户定义的角色结合使用。 用户定义的角色就是你在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中创建的、随后用来为包分配权限的角色。 若要访问包，用户必须是用户定义的角色和相关的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 固定数据库级角色的成员。 例如，如果用户是分配给包的 **AuditUsers** 用户定义角色的成员，他们必须也是 **db_ssisadmin**、 **db_ssisltduser** 或 **db_ssisoperator** 角色的成员，才能拥有对包的读访问权限。  
  
 如果不为包分配用户定义角色，则对包的访问权限由固定数据库级角色确定。  
  
 如果希望使用用户定义的角色，则在将它们分配给包之前，必须先将其添加到 **msdb** 数据库中。 您可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中创建新的数据库角色。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据库级角色可授予 msdb 数据库中的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 系统表的权限。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须启动（MSSQLSERVER 服务），才能连接到数据库引擎以及访问 **msdb** 数据库。  
  
 为了向包分配角色，您需要完成以下任务。  
  
-   **打开对象资源管理器并连接到 Integration Services**  
  
     在使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]将角色分配给包之前，必须在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中打开对象资源管理器，并连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。  
  
     必须启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务才能连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。  
  
-   **将读取者角色和写入者角色分配给包**  
  
     可以将读取者角色和写入者角色分配给每个包。  

## <a name="assign-a-reader-and-writer-role-to-a-package"></a><a name="assign"></a> 将读取者角色和写入者角色分配给包
  可以将读取者角色和写入者角色分配给每个包。  
  
### <a name="assign-a-reader-and-writer-role-to-a-package"></a>将读取者角色和写入者角色分配给包  
  
1.  在对象资源管理器中，找到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 连接。  
  
2.  展开“已存储的包”文件夹，然后展开包含要将角色分配到的包的子文件夹。  
  
3.  右键单击要向其分配角色的包。  
  
4.  在 **“包角色”** 对话框中，从 **“读取者角色”** 列表中选择一个读取者角色，并从 **“写入者角色”** 列表中选择一个写入者角色。  
  
5.  单击“确定”。 

## <a name="create-a-user-defined-role"></a><a name="create"></a> 创建用户定义的角色
    
### <a name="to-create-a-user-defined-role"></a>创建用户定义角色  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
2.  在 **“视图”** 菜单上，单击 **“对象资源管理器”** 。  
  
3.  在对象资源管理器工具栏上，单击 **“连接”** ，再单击 **“数据库引擎”** 。  
  
4.  在 **“连接到服务器”** 对话框中，提供服务器名，并选择身份验证模式。 可以使用句点 (.)、(local) 或 **localhost** 来指示本地服务器。  
  
5.  单击“连接”。  
  
6.  展开“数据库”、“系统数据库”、“msdb”、“安全性”和“角色”。  
  
7.  在“角色”节点中，右键单击“数据库角色”，再单击“新建数据库角色”。  
  
8.  在“常规”页上，提供一个名称，还可以指定一个所有者、拥有的架构以及添加角色成员。  
  
9. 还可以单击 **“权限”** ，配置对象权限。  
  
10. 还可以单击 **“扩展属性”** ，配置任何扩展属性。  
  
11. 单击“确定”。 

## <a name="package-roles-dialog-box-ui-reference"></a><a name="roles_dialog"></a>“包角色”对话框 UI 参考
  可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“包角色”对话框，指定具有包读取访问权限的数据库级角色以及具有包写入访问权限的数据库级角色。 数据库级角色仅适用于  msdb 数据库中存储的包[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 该对话框中列出的角色是 **msdb** 系统数据库的当前数据库角色。 如果未选择任何角色，将应用默认的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 角色。 默认情况下，读取者角色包括 **db_ssisadmin**、 **db_ssisoperator** 以及创建包的用户。 作为以上任一角色的成员的用户或创建该包的用户，可以枚举、查看、导出和运行包。 默认情况下，写入者角色包括 **db_ssisadmin** 和创建包的用户。 作为此角色的成员的用户和创建该包的用户，可以导入、删除和更改包。  
  
 **sysssispackages** 表中的 **ownersid** 列列出了创建包的用户的唯一安全标识符。  
  
### <a name="options"></a>选项  
 **包名称**  
 指定包的名称。  
  
 **读取者角色**  
 从列表中选择一个角色。  
  
 **写入者角色**  
 从列表中选择一个角色  
