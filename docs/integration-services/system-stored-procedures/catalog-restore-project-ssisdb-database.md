---
description: catalog.restore_project（SSISDB 数据库）
title: catalog.restore_project（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 8adee525-579b-4d2f-b807-e2ecc07fb2e9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6ad50f86bc3c4f6f52262df591c1504139590cab
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129746"
---
# <a name="catalogrestore_project-ssisdb-database"></a>catalog.restore_project（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目还原到以前的版本。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.restore_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project _name  
    , [ @object_version_lsn = ] object_version_lsn  
  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含项目的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @project _name = ] project_name  
 项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [ @object_version_lsn = ] object_version_lsn  
 项目的版本。 object_version_lsn 为 bigint。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 如果找到 project_name，则项目详细信息将作为结果的一部分返回为 varbinary(MAX)。  
  
 如果无法将项目还原到指定的文件夹，则返回 NO RESULT SET。  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   项目版本不存在，或与项目名称不匹配  
  
-   项目不存在。  
  
-   用户没有相应的权限  
  
## <a name="remarks"></a>注解  
 还原某个项目后，将为所有参数分配默认值，并且所有环境引用都保持不变。 目录中保留的最大项目版本数量由目录属性 MAX_VERSIONS_PER_PROJECT 决定，如 [catalog_property](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) 视图中所示。  
  
> [!WARNING]  
>  在还原项目后，环境引用可能不再有效。  
  
  
