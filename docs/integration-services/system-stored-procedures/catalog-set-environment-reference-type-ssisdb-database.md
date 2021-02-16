---
description: catalog.set_environment_reference_type（SSISDB 数据库）
title: catalog.set_environment_reference_type（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: b79e3a06-22c0-40e5-8933-1b3414db3329
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9948b4cdc9c1c9fc6467ba718f3a1a7c59b78e54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273208"
---
# <a name="catalogset_environment_reference_type-ssisdb-database"></a>catalog.set_environment_reference_type（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中某个项目的现有环境引用关联的引用类型和环境名称。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_environment_reference_location [ @reference_id = reference_id  
    , [ @reference_type = ] reference_type  
 [  , [ @environment_folder_name = ] environment_folder_name ]  
```  
  
## <a name="arguments"></a>参数  
 [ @reference_id = ] reference_id   
 要更新的环境引用的唯一标识符。 reference_id 为 bigint   。  
  
 [ @reference_type = ] reference_type   
 指示环境是可以位于与项目相同的文件夹中（相对引用），还是位于其他文件夹中（绝对引用）中。 使用值 `R` 指示相对引用。 使用值 `A` 指示绝对引用。 reference_type 为 char(1)   。  
  
 [ @environment_folder_name = ] environment_folder_name   
 环境所在的文件夹。 此值对于绝对引用是必需的。 environment_folder_name 为 nvarchar(128)   。  
  
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
  
-   文件夹名称、环境名称或引用 ID 无效  
  
-   用户没有相应的权限  
  
-   通过使用 reference_location 参数中的 `A` 字符指定绝对引用，而未使用 environment_folder_name 参数指定该文件夹的名称   。  
  
## <a name="remarks"></a>备注  
 项目可以具有相对或绝对的环境引用。 相对引用通过名称引用环境，并要求它与项目位于相同文件夹中。 绝对引用通过名称和文件夹引用环境，可能引用与项目不在同一文件夹中的环境。 项目可以引用多个环境。  
  
> [!IMPORTANT]  
>  如果指定了相对引用，则不会使用 environment_folder_name 参数值，并且环境文件夹名称将自动设置为 NULL。 如果指定了绝对引用，则必须在 environment_folder_name 参数中提供环境文件夹名称。  
  
  
