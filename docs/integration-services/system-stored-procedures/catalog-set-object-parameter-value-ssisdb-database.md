---
description: catalog.set_object_parameter_value（SSISDB 数据库）
title: catalog.set_object_parameter_value（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: fb887543-f92f-404d-9495-a1dd23a6716e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bdc1a5ff8944a29d7e2a2fcdeec297973c9849f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355301"
---
# <a name="catalogset_object_parameter_value-ssisdb-database"></a>catalog.set_object_parameter_value（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中设置参数的值。 将值与环境变量关联，或者未分配其他任何值时，分配默认情况下使用的文本值。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_object_parameter_value [ @object_type = ] object_type   
    , [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @parameter_name = ] parameter_name   
    , [ @parameter_value = ] parameter_value   
 [  , [ @object_name = ] object_name ]  
 [  , [ @value_type = ] value_type ]  
```  
  
## <a name="arguments"></a>参数  
 [@object_type =] object_type  
 参数的类型。 使用值 `20` 表示项目参数，使用值 `30` 表示包参数。 *object_type* 为 **smallInt**。  
  
 [@folder_name =] folder_name  
 包含参数的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [@project_name =] project_name  
 包含参数的项目的名称。 *project_name* 为 **nvarchar(128)** 。  
  
 [@parameter_name =] parameter_name  
 参数的名称。 parameter_name 为 nvarchar(128)。  
  
 [@parameter_value =] parameter_value  
 参数值。 parameter_value 为 sql_variant。  
  
 [@object_name =] object_name  
 包的名称。 当参数为包参数时，需要此参数。 object_name 为 nvarchar(260)。  
  
 [@value_type =] value_type  
 参数值的类型。 使用字符 `V` 指示 parameter_value 是在执行前未分配其他任何值时默认使用的文本值。 使用字符 `R` 指示 parameter_value 是被引用的值，并已设置为环境变量的名称。 此参数是可选的，默认情况下，将使用 `V` 字符。 value_type 为 char(1)。  
  
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
 下面的列表描述了一些可能导致此存储过程引发错误的情况：  
  
-   参数类型无效。  
  
-   项目名称无效  
  
-   对于包参数，项目名称无效  
  
-   值类型无效  
  
-   用户没有相应的权限  
  
## <a name="remarks"></a>注解  
  
-   如果未指定 value_type，则默认为 parameter_value 使用文本值。 使用文本值时，[object_parameters](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md) 视图中的 value_set 将设置为 `1`。 不允许 NULL 参数值。  
  
-   如果 value_type 包含字符 `R`指示引用值则 parameter_value 将引用环境变量的名称。  
  
-   值 `20` 可能用于 object_type 以表示项目参数。 在此情况下，*object_name* 的值不是必需的，将忽略为 object_name 指定的任何值。 当用户要设置项目参数时，将使用此值。  
  
-   值 `30` 可能用于 object_type 以表示包参数。 在此情况下，使用 object_name 的一个值来表示相应的包。 如果未指定 object_name，则存储过程返回一个错误并且终止。  
  
  
