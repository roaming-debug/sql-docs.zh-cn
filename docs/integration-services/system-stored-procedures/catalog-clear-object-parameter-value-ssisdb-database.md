---
description: catalog.clear_object_parameter_value（SSISDB 数据库）
title: catalog.clear_object_parameter_value（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: dcbbb714-a051-4805-9e2b-2c2fb647c890
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9b8b3397514fc0dd2e7a4d8b5dfa704ab14a5989
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346882"
---
# <a name="catalogclear_object_parameter_value-ssisdb-database"></a>catalog.clear_object_parameter_value（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  清除存储在服务器上的现有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目或包的参数的值。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.clear_object_parameter [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @object_type = ] object_type   
    , [ @object_name = ] object_name   
    , [ @parameter_ name = ] parameter_name  
```  
  
## <a name="arguments"></a>参数  
 [ \@folder_name = ] *folder_name*  
 包含项目的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ \@project_name = ] *project_name*  
 项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [ \@object_type = ] *object_type*  
 对象的类型。 有效值包括 `20`（对应于项目）和 `30`（对应于包）。 *object_type* 为 **smallInt**。  
  
 [ \@ object _name = ] *object _name*  
 包的名称。 *object _name* 为 **nvarchar(260)**。  
  
 [ \@parameter_ name = ] *parameter_name*  
 参数的名称。 *parameter_ name* 为 **nvarchar(128)**。  
  
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
 下面的列表描述了一些可能导致 clear_object_parameter 存储过程引发错误的情况：  
  
-   指定了无效的对象类型，或在项目中找不到对象名称。  
  
-   项目不存在、项目无法访问，或者项目名称无效  
  
-   指定了无效的参数名称。  
  
-   用户不具备适当的权限。  
  
  
