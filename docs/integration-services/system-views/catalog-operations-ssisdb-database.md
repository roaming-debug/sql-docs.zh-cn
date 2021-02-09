---
description: catalog.operations（SSISDB 数据库）
title: catalog.operations（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- operations view [Integration Services]
- catalog.operations view [Integration Services]
ms.assetid: 9455c5b1-60ff-45fc-8599-cc3abbd6daf5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 05f34ebf6652983dd1c8ea007e9ba68d47ef1d00
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835248"
---
# <a name="catalogoperations-ssisdb-database"></a>catalog.operations（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中所有操作的详细信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|operation_id|**bigint**|操作的唯一标识符 (ID)。|  
|operation_type|**smallint**|操作的类型。|  
|created_time|**datetimeoffset**|创建操作的时间。|  
|object_type|**smallint**|受操作影响的对象的类型。 该对象可能是文件夹 (`10`）、项目 (`20`）、包 (`30`)、环境 (`40`） 或执行实例 （`50`)。|  
|object_id|**bigint**|操作影响的对象的 ID。|  
|object_name|**nvarchar(260)**|对象的名称。|  
|状态|**int**|操作的状态。 可能的值是已创建 (`1`)、正在运行 (`2`)、已取消 (`3`)、失败 (`4`)、挂起 (`5`)、意外结束 (`6`)、已成功 (`7`)、停止 (`8`) 和已完成 (`9`)。|  
|start_time|**datetimeoffset**|开始操作的时间。|  
|end_time|**datetimeoffsset**|操作结束的时间。|  
|caller_sid|**varbinary(85)**|如果是使用 Windows 身份验证进行登录的，则为用户的安全 ID (SID)。|  
|caller_name|**nvarchar(128)**|执行备份操作的帐户的名称。|  
|process_id|**int**|外部进程的进程 ID（如果适用）。|  
|stopped_by_sid|**varbinary(85)**|停止操作的用户的 SID。|  
|stopped_by_name|**nvarchar(128)**|停止操作的用户名。|  
|server_name|**nvarchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 指定实例的 Windows 服务器和实例信息。|  
|machine_name|**nvarchar(128)**|运行服务器实例的计算机名称。|  
  
## <a name="remarks"></a>备注  
 此视图对于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的每个操作显示一行。 它允许管理员枚举在服务器上执行的所有逻辑操作，如部署项目或执行包。  
  
 此视图显示以下操作类型，如 **operation_type** 列中所列出：  
  
|**operation_type** 值|**operation_type** 说明|**object_id** 说明|**object_name** 说明|  
|-------------------------------|-------------------------------------|--------------------------------|----------------------------------|  
|`1`|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 初始化|**NULL**|**NULL**|  
|`2`|保留窗口<br /><br /> （SQL 代理作业）|**NULL**|**NULL**|  
|`3`|MaxProjectVersion<br /><br /> （SQL 代理作业）|**NULL**|**NULL**|  
|`101`|**deploy_project**<br /><br /> （存储过程）|项目 ID|项目名称|  
|`106`|**restore_project**<br /><br /> （存储过程）|项目 ID|项目名称|  
|`200`|**create_execution** 和 **start_execution**<br /><br /> （存储过程）|项目 ID|**NULL**|  
|`202`|**stop_operation**<br /><br /> （存储过程）|项目 ID|**NULL**|  
|`300`|**validate_project**<br /><br /> （存储过程）|项目 ID|项目名称|  
|`301`|**validate_package**<br /><br /> （存储过程）|项目 ID|包名称|  
|`1000`|**configure_catalog**<br /><br /> （存储过程）|**NULL**|**NULL**||  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对操作的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
  
