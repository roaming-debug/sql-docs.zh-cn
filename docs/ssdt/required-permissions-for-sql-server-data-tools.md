---
title: 所需的权限
description: 了解 SQL Server Data Tools 中各种任务所需的权限，如创建数据库、运行单元测试及比较架构。
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b27038c4-94ab-449c-90b7-29d87ce37a8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.openlocfilehash: 6e519a3786b45d87344f8f5f5355353fd5865d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072451"
---
# <a name="required-permissions-for-sql-server-data-tools"></a>SQL Server Data Tools 所需权限

在 Visual Studio 中对某一数据库执行操作之前，必须使用对该数据库具有某些权限的帐户登录。 您所需的特定权限将基于您要执行的操作而异。 下面几节介绍了您可能要执行的各操作以及执行该操作所需的特定权限。  
  
-   [用于创建或部署数据库的权限](#DatabaseCreationAndDeploymentPermissions)  
  
-   [用于重构数据库的权限](#DatabaseRefactoringPermissions)  
  
-   [用于在 SQL Server 数据库上执行单元测试的权限](#DatabaseUnitTestingPermissions)  
  
-   [用于生成数据的权限](#DataGenerationPermissions)  
  
-   [用于比较架构和数据的权限](#SchemaAndDataComparePermissions)  
  
-   [用于运行 Transact-SQL 编辑器的权限](#Transact-SQLEditorPermissions)  
  
-   [针对 SQL Server 公共语言运行时 (SQL CLR) 项目的权限](#SQLCLRPermissions)  
  
## <a name="permissions-to-create-or-deploy-a-database"></a><a name="DatabaseCreationAndDeploymentPermissions"></a>用于创建或部署数据库的权限  
必须具有以下权限才能创建或部署数据库。  
  
|操作|所需的权限|  
|-|-|  
|导入数据库对象和设置|您必须能够连接到源数据库。<br /><br />如果源数据库基于 SQL Server 2005，则还必须对每个对象都拥有或具有 VIEW DEFINITION 权限。<br /><br />如果源数据库基于 SQL Server 2008 或更高版本，则还必须对每个对象都拥有或具有 VIEW DEFINITION 权限。 登录名必须具有 **VIEW SERVER STATE** 权限（对于数据库密钥）。|  
|导入服务器对象和设置|您必须能够连接到指定服务器上的 master 数据库。<br /><br />如果服务器正在运行 SQL Server 2005，则必须对该服务器具有 VIEW ANY DEFINITION 权限。<br /><br />如果源数据库基于 SQL Server 2008 或更高版本，则必须对该服务器具有 VIEW ANY DEFINITION 权限。 登录名必须具有 **VIEW SERVER STATE** 权限（对于数据库密钥）。|  
|创建或更新数据库项目|您不需要任何数据库权限即可创建或修改数据库项目。|  
|部署新数据库或在已设置“始终重新创建数据库”选项的情况下进行部署|必须具有 **CREATE DATABASE** 权限，或者必须是目标服务器上 **dbcreator** 角色的成员。<br /><br />在创建数据库时，Visual Studio 将连接到模型数据库并且复制其内容。 用于连接目标数据库的初始登录名（例如 *yourLogin*）必须具有 **db_creator** 和 **CONNECT SQL** 权限。 此登录名必须在模型数据库上拥有用户映射。 如果具有 sysadmin 权限，则可以通过发出以下 Transact\-SQL 语句创建映射：<br /><br />`USE [model] CREATE USER yourUser FROM LOGIN yourLogin`<br /><br />用户（在此示例中为 yourUser）必须对模型数据库具有 **CONNECT** 和 **VIEW DEFINITION** 权限。 如果具有 sysadmin 权限，则可以通过发出以下 Transact\-SQL 语句授予这些权限：<br /><br />`USE [model] GRANT CONNECT to yourUser GRANT VIEW DEFINITION TO yourUser`<br /><br />如果部署包含未命名约束的数据库并且启用了 **CheckNewContraints** 选项（默认启用），则必须具有 **db_owner** 或 **sysadmin** 权限，否则部署将失败。 这一原则仅适用于未命名的约束。 有关 **CheckNewConstraints** 选项的详细信息，请参阅 [数据库项目设置](../ssdt/database-project-settings.md)。|  
|部署对现有数据库的更新|您必须是有效数据库用户。 还必须是 **db_ddladmin** 角色的成员、拥有架构或者拥有要在目标数据库上创建或修改的对象。 您需要其他权限以便在预先部署或后期部署脚本中使用更高级概念，例如登录名或链接服务器。<br /><br />**注意：** 如果部署到 master 数据库，则还必须对部署到的服务器具有 **VIEW ANY DEFINITION** 权限。|  
|在数据库项目中将程序集与 EXTERNAL_ACCESS 选项一起使用|您必须为数据库项目设置 TRUSTWORTHY 属性。 你必须为你的 SQL Server 登录名具有 EXTERNAL ACCESS ASSEMBLY 权限。|  
|将程序集部署到新的或现有的数据库|您必须是目标部署服务器上 sysadmin 角色的成员。|  
  
有关详细信息，请参阅 SQL Server 联机丛书。  
  
## <a name="permissions-to-refactor-a-database"></a><a name="DatabaseRefactoringPermissions"></a>用于重构数据库的权限  
*数据库重构* 仅在数据库项目内发生。 您必须有权使用数据库项目。 在您在目标数据库上部署更改之前，对目标数据库不需要具有权限。  
  
## <a name="permissions-to-perform-unit-testing-on-a-sql-server-database"></a><a name="DatabaseUnitTestingPermissions"></a>用于在 SQL Server 数据库上执行单元测试的权限  
必须具有以下权限才能在数据库上执行单元测试。  
  
|操作|所需的权限|  
|-|-|   
|执行测试操作|您必须使用执行上下文数据库连接。 有关详细信息，请参阅[连接字符串和权限概述](../ssdt/overview-of-connection-strings-and-permissions.md)。|  
|执行预先测试或后期测试操作|您必须使用特权上下文数据库连接。 此数据库连接具有比执行上下文连接更高的权限。|  
|运行 TestInitialize 和 TestCleanup 脚本|您必须使用特权上下文数据库连接。|  
|在运行测试前部署数据库更改|您必须使用特权上下文数据库连接。 有关详细信息，请参阅[操作说明：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)。|  
|在运行测试前生成数据|您必须使用特权上下文数据库连接。 有关详细信息，请参阅[操作说明：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)。|  
  
## <a name="permissions-to-generate-data"></a><a name="DataGenerationPermissions"></a>用于生成数据的权限  
必须对目标数据库中的对象具有 **INSERT** 和 **SELECT** 权限，才能通过使用数据生成器生成测试数据。 如果在生成数据前清除数据，则还必须对目标数据库中的对象具有 **DELETE** 权限。 若要重置某个表中的 **IDENTITY** 列，则必须拥有该表，或者必须是 db_owner 或 db_ddladmin 角色的成员。  
  
## <a name="permissions-to-compare-schemas-and-data"></a><a name="SchemaAndDataComparePermissions"></a>用于比较架构和数据的权限  
您必须具有以下权限才能比较架构或数据。  
  
|操作|所需的权限|  
|-|-|   
|比较两个数据库的架构|必须具有从数据库导入对象和设置的权限，如[用于创建或部署数据库的权限](#DatabaseCreationAndDeploymentPermissions)中所述。|  
|比较数据库和数据库项目的架构|必须具有从数据库导入对象和设置的权限，如[用于创建或部署数据库的权限](#DatabaseCreationAndDeploymentPermissions)中所述。 还必须具有在 Visual Studio 中打开的数据库项目。|  
|将更新写入目标数据库|必须具有将更新部署到目标数据库的权限，如[用于创建或部署数据库的权限](#DatabaseCreationAndDeploymentPermissions)中所述。|  
|比较两个数据库的数据|除了比较两个数据库的架构所需的权限之外，还需要拥有要比较的所有表的 **SELECT** 权限以及 **VIEW DATABASE STATE** 权限。|  
  
有关详细信息，请参阅 SQL Server 联机丛书。  
  
## <a name="permissions-to-run-the-transact-sql-editor"></a><a name="Transact-SQLEditorPermissions"></a>用于运行 Transact\-SQL 编辑器的权限  
可以在 Transact\-SQL 编辑器内执行的工作由你对目标数据库的执行上下文确定。  
  
## <a name="permissions-for-sql-server-common-language-run-time-projects"></a><a name="SQLCLRPermissions"></a>针对 SQL Server 公共语言运行时项目的权限  
下表列出了您为部署或调试 CLR 项目而必须具有的权限：  
  
|操作|所需的权限|  
|-----------|------------------------|  
|部署（初始或增量）安全权限集程序集|db_DDLAdmin - 此权限为您部署的程序集和对象类型授予 CREATE 和 ALTER 权限<br /><br />数据库级别 VIEW DEFINITION - 部署所必需的<br /><br />数据库级别 CONNECT - 授予连接到数据库的能力|  
|部署 external_access 权限集程序集|db_DDLAdmin - 此权限为您部署的程序集和对象类型授予 CREATE 和 ALTER 权限<br /><br />数据库级别 VIEW DEFINITION - 部署所必需的<br /><br />数据库级别 CONNECT - 授予连接到数据库的能力<br /><br />此外，您还必须具有：<br /><br />设置为 ON 的 TRUSTWORTHY 数据库选项<br /><br />您用来部署的登录名必须具有 External Access Assembly 服务器权限。|  
|部署非安全权限集程序集|db_DDLAdmin - 此权限为您部署的程序集和对象类型授予 CREATE 和 ALTER 权限<br /><br />数据库级别 VIEW DEFINITION - 部署所必需的<br /><br />数据库级别 CONNECT - 授予连接到数据库的能力<br /><br />此外，您还必须具有：<br /><br />设置为 ON 的 TRUSTWORTHY 数据库选项<br /><br />您用来部署的登录名必须具有 Unsafe Assembly 服务器权限。|  
|远程调试 SQL CLR 程序集|您必须具有 sysadmin 固定角色权限。|  
  
> [!IMPORTANT]  
> 在所有情况下，程序集所有者都必须是您用于部署程序集的用户，或者所有者必须是该用户为其成员的角色。 该要求还适用于您部署的程序集引用的任何程序集。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
  
