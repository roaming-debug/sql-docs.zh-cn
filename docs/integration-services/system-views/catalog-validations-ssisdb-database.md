---
description: catalog.validations（SSISDB 数据库）
title: catalog.validations（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: dbafe110-b480-48f3-b45f-31d71ca68f62
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 92a6fa341d231a4337b4377aab429b5d97eed5d4
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835139"
---
# <a name="catalogvalidations-ssisdb-database"></a>catalog.validations（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中所有项目和包验证的详细信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|validation_id|**bigint**|验证的唯一标识符 (ID)。|  
|environment_scope|**Char(1)**|指示由验证考虑的环境引用。 如果值为 `A`，则验证中包括与项目关联的所有环境引用。 值为 `S` 时，只包括一个环境引用。 当值为 `D` 时，不包括环境引用，并且每个参数必须有文字默认值才能通过验证。|  
|validate_type|**Char(1)**|要执行的验证类型。 可能的验证类型为依赖项验证 (`D`） 或完整验证 (`F`)。 包验证始终是完整验证。|  
|folder_name|**nvarchar(128)**|包含对应项目的文件夹的名称。|  
|project_name|**nvarchar(128)**|项目的名称。|  
|project_lsn|**bigint**|要对照其进行验证的项目版本。|  
|use32bitruntime|**bit**|指示是否应使用 32 位运行时在 64 位操作系统上运行包。 如果值为 `1`则使用 32 位运行时进行执行。 如果值为 `0`，则使用 64 位运行库来运行执行过程。|  
|reference_id|**bigint**|项目用来引用环境的项目环境引用的唯一 ID。|  
|operation_type|**smallint**|操作的类型。 在此视图中显示的操作包括项目验证 (`300`) 和包验证 (`301`)。|  
|object_name|**nvarhcar(260)**|对象的名称。|  
|object_type|**smallint**|对象的类型。 该对象可能是一个项目 (`20`) 或包 (`30`)。|  
|object_id|**bigint**|操作影响的对象的 ID。|  
|start_time|**datetimeoffset(7)**|开始操作的时间。|  
|end_time|**datetimeoffsset(7)**|操作结束的时间。|  
|状态|**int**|操作的状态。 可能的值是已创建 (`1`)、正在运行 (`2`)、已取消 (`3`)、失败 (`4`)、挂起 (`5`)、意外结束 (`6`)、已成功 (`7`)、停止 (`8`) 和已完成 (`9`)。|  
|caller_sid|**varbinary(85)**|如果是使用 Windows 身份验证进行登录的，则为用户的安全 ID (SID)。|  
|caller_name|**nvarchar(128)**|执行备份操作的帐户的名称。|  
|process_id|**int**|外部进程的进程 ID（如果适用）。|  
|stopped_by_sid|**varbinary(85)**|停止操作的用户的 SID。|  
|stopped_by_name|**nvarchar(128)**|停止操作的用户名。|  
|server_name|**nvarchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 指定实例的 Windows 服务器和实例信息。|  
|machine_name|**nvarchar(128)**|运行服务器实例的计算机名称。|  
|dump_id|**uniqueidentifier**|执行转储的 ID。|  
  
## <a name="remarks"></a>备注  
 此视图对于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的每个验证显示一行。  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对相应操作的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
  
