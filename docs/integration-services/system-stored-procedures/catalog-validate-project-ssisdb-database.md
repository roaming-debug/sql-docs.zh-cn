---
description: catalog.validate_project（SSISDB 数据库）
title: catalog.validate_project（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5270689a-46d4-4847-b41f-3bed1899e955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c491a8914fb11da815d0887ae5b2248f1e2a7c19
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129568"
---
# <a name="catalogvalidate_project-ssisdb-database"></a>catalog.validate_project（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  以异步方式验证 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目。  
  
## <a name="syntax"></a>语法  
  
```sql
catalog.validate_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @validate_type = ] validate_type  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含项目的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @project_name = ] project_name   
 项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [ @validate_type = ] validate_type  
 指示要执行的验证类型。 使用字符 `F` 执行完全验证。 此参数是可选的，默认情况下使用字符 `F`。 validate_type 为 char(1)。  
  
 [ @validation_id = ] validation_id  
 返回验证的唯一标识符 (ID)。 validation_id 为 bigint。  
  
 [ @use32bitruntime = ] use32bitruntime  
 指示是否应使用 32 位运行时在 64 位操作系统上运行包。 在 64 位操作系统上运行时，使用值 `1` 以便使用 32 位运行时执行此包。 在 64 位操作系统上运行时，使用值 `0` 以便使用 64 位运行时执行此包。 此参数是可选的。 use32bitruntime 为 bit。  
  
 [ @environment_scope = ] environment_scope  
 指示由验证考虑的环境引用。 如果值为 `A`，则验证中包括与项目关联的所有环境引用。 值为 `S` 时，只包括一个环境引用。 当值为 `D` 时，不包括环境引用，并且每个参数必须有文字默认值才能通过验证。 此参数是可选的，默认情况下使用字符 `D`。 environment_scope 为 char(1)。  
  
 [ @reference_id = ] reference_id   
 环境引用的唯一 ID。 如果 environment_scope 为 `S`，仅当在验证中包含单个环境引用时，才需要此参数。 reference_id 为 bigint   。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 验证步骤的输出结果作为结果集的不同部分返回。  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 权限，如果适用，则包含针对引用环境的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表提供了一些可能引发错误或警告的情况：  
  
-   验证针对项目中的一个或多个包失败  
  
-   如果验证中包含的一个或多个引用环境不包含引用的变量，则验证失败  
  
-   指定的验证类型无效  
  
-   项目名称或环境引用 ID 无效  
  
-   用户没有相应的权限  
  
## <a name="remarks"></a>注解  
 验证有助于识别将阻止项目中的包成功运行的问题。 使用 [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) 或 [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) 视图以监视验证状态。  
  
 在验证中只可使用此用户可访问的环境。 验证输出作为结果集发送到客户端。  
  
 在这一版本中，项目验证不支持依赖项验证。  
  
 完全验证确认在验证包含的引用环境中可以找到所有引用的环境变量。 完全验证结果列出了无效的环境引用和在验证中包含的任何引用环境中找不到的引用环境变量。  
  
  
