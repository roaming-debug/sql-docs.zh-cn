---
description: 存储过程（Integration Services 目录）
title: 存储过程（Integration Services 目录）| Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- stored procedures [Integration Services]
ms.assetid: a6ccd884-108f-4fb6-95ad-00b9cb65d5d6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c5b0f5c9dad9633950d0c0fd8698d3f658d9d390
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495337"
---
# <a name="stored-procedures-integration-services-catalog"></a>存储过程（Integration Services 目录）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节介绍可用于管理已部署到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 实例的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程。  
  
 调用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 存储过程以添加、删除、修改或指定存储在 SSISDB 目录中的对象。  
  
 目录的默认名称为 SSISDB。 目录中存储的对象包括项目、包、参数环境和操作历史记录。  
  
 您可以直接使用数据库视图和存储过程，或者编写调用托管 API 的自定义代码。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和托管 API 查询这些视图，并调用本节中介绍的存储过程来执行许多任务。  
  
## <a name="in-this-section"></a>本节内容  
 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md)  
 在包数据流中添加组件输出的数据分流点。  
  
 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)  
 将数据分流点添加到包数据流中特定的数据流路径。  
  
 [catalog.check_schema_version](../../integration-services/system-stored-procedures/catalog-check-schema-version.md)  
 确定 SSISDB 目录架构与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 二进制文件（ISServerExec 和 SQLCLR 程序集）是否兼容。  
  
 [catalog.clear_object_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md)  
 清除存储在服务器上的现有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目或包的参数的值。  
  
 [catalog.configure_catalog（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md)  
 通过将目录属性设置为指定的值，配置 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录。  
  
 [catalog.create_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-environment-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建环境。  
  
 [catalog.create_environment_reference（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-environment-reference-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中为项目创建环境引用。  
  
 [catalog.create_environment_variable（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-environment-variable-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建环境变量。  
  
 [catalog.create_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建执行实例。  
  
 [catalog.create_execution_dump](../../integration-services/system-stored-procedures/catalog-create-execution-dump.md)  
 导致暂停正在运行的包并创建转储文件。  
  
 [catalog.create_folder（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-folder-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建一个文件夹。  
  
 [catalog.delete_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-environment-ssisdb-database.md)  
 从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个文件夹删除环境。  
  
 [catalog.delete_environment_reference（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-environment-reference-ssisdb-database.md)  
 从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录的项目中删除一个环境引用。  
  
 [catalog.delete_environment_variable（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-environment-variable-ssisdb-database.md)  
 从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个环境删除环境变量。  
  
 [catalog.delete_folder（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-folder-ssisdb-database.md)  
 从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录删除文件夹。  
  
 [catalog.delete_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-delete-project-ssisdb-database.md)  
 从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个文件夹删除现有项目。  
  
 [catalog.deny_permission（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-deny-permission-ssisdb-database.md)  
 拒绝对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个安全对象的权限。  
  
 [catalog.deploy_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)  
 将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的文件夹，或更新以前部署的现有项目。  
  
 [catalog.get_parameter_values（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md)  
 从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目和对应包中解析和检索默认参数值。  
  
 [catalog.get_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)  
 检索 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中现有项目的属性。  
  
 [catalog.grant_permission（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-grant-permission-ssisdb-database.md)  
 授予对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个安全对象的权限。  
  
 [catalog.move_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-move-environment-ssisdb-database.md)  
 将环境从一个文件夹移到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的另一个文件夹。  
  
 [catalog.move_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-move-project-ssisdb-database.md)  
 将项目从一个文件夹移到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的另一个文件夹。  
  
 [catalog.remove_data_tap](../../integration-services/system-stored-procedures/catalog-remove-data-tap.md)  
 从执行中的组件输出删除数据分流点。  
  
 [catalog.rename_environment（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-rename-environment-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中重命名环境。  
  
 [catalog.rename_folder（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-rename-folder-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中重命名一个文件夹。  
  
 [catalog.restore_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md)  
 将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目还原到以前的版本。  
  
 [catalog.revoke_permission（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-revoke-permission-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中撤消对某个安全对象的权限。  
  
 [catalog.set_environment_property（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-property-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置环境的属性。  
  
 [catalog.set_environment_reference_type（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-reference-type-ssisdb-database.md)  
 设置与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中某个项目的现有环境引用关联的引用类型和环境名称。  
  
 [catalog.set_environment_variable_property（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-variable-property-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置环境变量的属性。  
  
 [catalog.set_environment_variable_protection（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-variable-protection-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置环境变量的敏感性位。  
  
 [catalog.set_environment_variable_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-environment-variable-value-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置环境变量的值。  
  
 [catalog.set_execution_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
 为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的执行实例设置参数的值。  
  
 [catalog.set_execution_property_override_value](../../integration-services/system-stored-procedures/catalog-set-execution-property-override-value.md)  
 为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的执行实例设置属性的值。  
  
 [catalog.set_folder_description（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-folder-description-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置文件夹的说明。  
  
 [catalog.set_object_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置参数的值。 将值与环境变量关联，或者如果未指定任何其他值，则分配默认情况下使用的文本值。  
  
 [catalog.start_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中启动执行实例。  
  
 [catalog.startup](../../integration-services/system-stored-procedures/catalog-startup.md)  
 对 SSISDB 目录的操作状态进行维护。  
  
 [catalog.stop_operation（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md)  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中停止执行的验证或实例。  
  
 [catalog.validate_package（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-validate-package-ssisdb-database.md)  
 以异步方式验证 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的包。  
  
 [catalog.validate_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-validate-project-ssisdb-database.md)  
 以异步方式验证 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目。  
  
[catalog.add_execution_worker（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)   
将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Worker 添加到 Scale Out 中的一个执行实例。

[catalog.enable_worker_agent（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-enable-worker-agent-ssisdb-database.md)   
为使用此 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录的 Scale Out Master 启用 Scale Out Worker。

[catalog.disable_worker_agent（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-disable-worker-agent-ssisdb-database.md)   
为使用此 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录的 Scale Out Master 禁用 Scale Out Worker。


