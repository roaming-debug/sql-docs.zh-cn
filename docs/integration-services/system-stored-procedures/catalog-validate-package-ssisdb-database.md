---
description: catalog.validate_package（SSISDB 数据库）
title: catalog.validate_package（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- validate_package stored procedure [Integration Services]
- catalog.validate_package stored procedure [Integration Services]
ms.assetid: 0dc03df1-b793-408f-af4c-c11188729abf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2aca1e5c0b1ace9e1020b58464459415a8939ce1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350266"
---
# <a name="catalogvalidate_package-ssisdb-database"></a>catalog.validate_package（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  以异步方式验证 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的包。  
  
## <a name="syntax"></a>语法  
  
```sql
catalog.validate_package [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @package_name = ] package_name  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含包的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @project_name = ] project_name   
 包含此包的项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [ @package_name = ] package_name  
 包的名称。 package_name 为 nvarchar(260)。  
  
 [ @validation_id = ] validation_id  
 返回验证的唯一标识符 (ID)。 validation_id 为 bigint。  
  
 [ @use32bitruntime = ] use32bitruntime  
 指示是否应使用 32 位运行时在 64 位操作系统上运行包。 在 64 位操作系统上运行时，使用值 `1` 以便使用 32 位运行时执行此包。 在 64 位操作系统上运行时，使用值 `0` 以便使用 64 位运行时执行此包。 此参数是可选的。 use32bitruntime 为 bit。  
  
 [ @environment_scope = ] environment_scope  
 指示由验证考虑的环境引用。 如果值为 `A`，则验证中包括与项目关联的所有环境引用。 值为 `S` 时，只包括一个环境引用。 当值为 `D` 时，不包括环境引用，并且每个参数必须有文字默认值才能通过验证。 此参数是可选的。 默认情况下使用字符 `D`。 environment_scope 为 char(1)。  
  
 [ @reference_id = ] reference_id   
 环境引用的唯一 ID。 如果 environment_scope 为 `S`，仅当在验证中包含单个环境引用时，才需要此参数。 reference_id 为 bigint   。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 权限，如果适用，则包含针对引用环境的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   项目名称或包名称无效  
  
-   用户没有相应的权限  
  
-   验证中包含的一个或多个引用环境不包含引用的变量  
  
-   包验证失败  
  
-   所引用的环境不存在  
  
-   在验证中包含的引用环境中找不到引用的变量  
  
-   包参数中引用了变量，但验证中不包括任何引用的环境  
  
## <a name="remarks"></a>注解  
 验证有助于识别可能阻止包成功运行的问题。 使用 [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) 或 [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) 视图以监视验证状态。  
  
  
