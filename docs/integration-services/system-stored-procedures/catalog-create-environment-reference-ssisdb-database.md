---
description: catalog.create_environment_reference（SSISDB 数据库）
title: catalog.create_environment_reference（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 48069bea-31cb-4a0e-9849-a07edc94088f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c7c513bf3fcd720418be0eb79a99f75194d0fb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346835"
---
# <a name="catalogcreate_environment_reference-ssisdb-database"></a>catalog.create_environment_reference（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中为项目创建环境引用。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.create_environment_reference [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @environment_name = ] environment_name  
     , [ @reference_type = ] reference_type  
  [  , [ @environment_folder_name = ] environment_folder_name ]  
  [  , [ @reference_id = ] reference_id OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 正在引用环境的项目的文件夹名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @project_name = ] project_name   
 正在引用环境的项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [ @environment_name = ] environment_name   
 被引用环境的名称。 environment_name 为 nvarchar(128)   。  
  
 [ @reference_type = ] reference_type   
 指示环境是可以位于与项目相同的文件夹中（相对引用），还是位于其他文件夹中（绝对引用）中。 使用值 `R` 指示相对引用。 使用值 `A` 指示绝对引用。 reference_type 为 char(1)   。  
  
 [ @environment_folder_name = ] environment_folder_name   
 被引用的环境所在的文件夹的名称。 此值对于绝对引用是必需的。 environment_folder_name 为 nvarchar(128)   。  
  
 [ @reference_id = ] reference_id   
 返回新引用的唯一标识符。 此参数是可选的。 reference_id 为 bigint   。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对项目的 READ 和 MODIFY 权限，以及针对环境的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   文件夹名称无效  
  
-   项目名称无效  
  
-   用户没有相应的权限  
  
-   通过使用 reference_location 参数中的 `A` 字符指定绝对引用，而未使用 environment_folder_name 参数指定该文件夹的名称   。  
  
## <a name="remarks"></a>备注  
 项目可以具有相对或绝对的环境引用。 相对引用通过名称引用环境，并要求它与项目位于相同文件夹中。 绝对引用通过名称和文件夹引用环境，可能引用与项目不在同一文件夹中的环境。 项目可以引用多个环境。  
  
  
