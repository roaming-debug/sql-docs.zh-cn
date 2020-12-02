---
description: catalog.set_environment_variable_protection（SSISDB 数据库）
title: catalog.set_environment_variable_protection（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 005b6b2f-a5d9-4ea4-8d4e-beed6ab33c0d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5e5e93263d37acf72782f2b9a2fe3ef3f7c5d29d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129691"
---
# <a name="catalogset_environment_variable_protection-ssisdb-database"></a>catalog.set_environment_variable_protection（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置环境变量的敏感性位。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_environment_variable_protection [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @sensitive = ] sensitive  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含环境的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @environment_name = ] environment_name   
 环境的名称。 environment_name 为 nvarchar(128)   。  
  
 [ @variable_name = ] variable_name  
 环境变量名。 variable_name 为 nvarchar(128)。  
  
 [ @sensitive = ] sensitive  
 指示变量是否包含敏感值。 使用值 `1` 表示环境变量的值是敏感值，值为 `0` 表示该值不是敏感值。 存储敏感值时将对其加密。 不敏感的值以纯文本形式存储。 sensitive 参数为 bit。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对环境的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   文件夹名称无效  
  
-   环境名称无效  
  
-   环境变量名称无效  
  
-   用户没有相应的权限  
  
  
