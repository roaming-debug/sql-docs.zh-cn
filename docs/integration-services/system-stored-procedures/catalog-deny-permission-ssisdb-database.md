---
description: catalog.deny_permission（SSISDB 数据库）
title: catalog.deny_permission（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: de310bac-2ddc-4ef9-8783-43dcb02a94f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d32af051552c211592bcc4deb2bcd2c1c1c20079
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342262"
---
# <a name="catalogdeny_permission-ssisdb-database"></a>catalog.deny_permission（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  拒绝对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的某个安全对象的权限。  
  
## <a name="syntax"></a>语法  
  
```sql
catalog.deny_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>参数  
 [ @object_type = ] *object_type*  
 安全对象的类型。 安全对象类型包括文件夹 (`1`）、项目 (`2`)、环境 (`3`) 和操作 (`4`)。*object_type* 为 **smallint** _。_  
  
 [ @object_id = ] *object_id*  
 安全对象的唯一标识符 (ID) 或主键。 *object_id* 为 **bigint**。  
  
 [ @principal_id = ] *principal_id*  
 被拒绝的主体的 ID。 *principal_id* 为 **int**。  
  
 [ @permission_type = ] *permission_type*  
 要被拒绝的权限类型。 *permission_type* 为 **smallint**。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）  
  
 1（object_class 无效）  
  
 2（object_id 不存在）  
  
 3（主体不存在）  
  
 4（权限无效）  
  
 5（其他错误）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对对象的 MANAGE_PERMISSIONS 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="remarks"></a>备注  
 通过此存储过程可以拒绝下表中所示的权限类型：  
  
|permission_type 值|权限名称|权限说明|适用对象类型|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|允许主体读取被视为对象一部分的信息（如属性）。 它不允许主体枚举或读取该对象中包含的其他对象的内容。|文件夹、项目、环境、操作|  
|`2`|MODIFY|允许主体修改被视为对象一部分的信息（如属性）。 它不允许主体修改该对象中包含的其他对象。|文件夹、项目、环境、操作|  
|`3`|EXECUTE|允许主体执行项目中的所有包。|项目|  
|`4`|MANAGE_PERMISSIONS|允许主体向对象分配权限。|文件夹、项目、环境、操作|  
|`100`|CREATE_OBJECTS|允许主体在文件夹中创建对象。|文件夹|  
|`101`|READ_OBJECTS|允许主体读取文件夹中的所有对象。|文件夹|  
|`102`|MODIFY_OBJECTS|允许主体修改文件夹中的所有对象。|文件夹|  
|`103`|EXECUTE_OBJECTS|允许主体执行文件夹中所有项目的所有包。|文件夹|  
|`104`|MANAGE_OBJECT_PERMISSIONS|允许主体管理文件夹中所有对象的权限。|文件夹|  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   如果指定 permission_type，此过程会拒绝显式分配给指定主体的针对指定对象的指定权限。 即使不发生此类情况，该过程也仍将返回成功代码值 (`0`)。  
  
-   如果忽略 permission_type，此过程会拒绝指定主体针对指定对象的所有权限。  
  
  
