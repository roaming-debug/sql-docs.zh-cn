---
description: catalog.delete_project（SSISDB 数据库）
title: catalog.delete_project（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f3431445-8dd2-443b-813e-b99db893977e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 40e9a9a404c17b1a86b48fef45cdc32bf399ace3
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129827"
---
# <a name="catalogdelete_project-ssisdb-database"></a>catalog.delete_project（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个文件夹删除现有项目。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.delete_project [ @folder_name = ] folder_name , [ @project_name = ] project_name  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含项目的文件夹的名称。 folder_name 为 nvarchar(128)。  
  
 [ @project_name = ] project_name   
 要删除的项目的名称。 project_name 为 nvarchar(128)。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能导致 delete_project 存储过程引发错误的情况：  
  
-   项目不存在。  
  
-   文件夹不存在  
  
-   用户没有相应的权限  
  
## <a name="remarks"></a>注解  
 对应项目的所有对象和环境引用将与项目一起删除。 但是，直到下次运行操作清除作业之前，将保留项目版本和相关的操作记录。  
  
  
