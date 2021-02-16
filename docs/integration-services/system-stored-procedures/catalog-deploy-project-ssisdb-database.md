---
description: catalog.deploy_project（SSISDB 数据库）
title: catalog.deploy_project（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2e3439b4-7226-4b61-a993-7a1d161eac7e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ef918ad60dd685cf90fc043eba72dcc6dd7987ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342250"
---
# <a name="catalogdeploy_project-ssisdb-database"></a>catalog.deploy_project（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的文件夹，或更新以前部署的现有项目。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.deploy_project [@folder_name =] folder_name   
      , [ @project_name = ] project_name   
      , [ @project_stream = ] projectstream   
    [ , [ @operation_id = ] operation_id OUTPUT ]   
```  
  
## <a name="arguments"></a>参数  
 [@folder_name =] folder_name  
 在其中部署项目的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [@project_name =] project_name  
 文件夹中新的或更新的项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [@projectstream =] projectstream  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目部署文件（扩展名为.ispac）的二进制内容。  
  
 您可以使用带 OPENROWSET 函数的 SELECT 语句以及大容量行集提供程序来检索该文件的二进制内容。 有关示例，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。 有关 OPENROWSET 的详细信息，请参阅 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)。  
  
 projectstream 为 varbinary(MAX)  
  
 [@operation_id =] operation_id  
 返回部署操作的唯一标识符。 operation_id 为 bigint。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   对文件夹具有 CREATE_OBJECTS 权限才能部署新项目；对项目具有 MODIFY 权限才能更新项目  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能导致此存储过程引发错误的情况：  
  
-   参数引用的对象不存在，参数试图创建的对象已存在，或者参数在某个其他方面无效  
  
-   参数 \@project_name 的值与部署文件中的项目的名称不匹配。  
  
-   用户不具备足够的权限  
  
## <a name="remarks"></a>注解  
 在项目部署或更新期间，存储过程不检查项目中各个包的保护级别。  
  
  
