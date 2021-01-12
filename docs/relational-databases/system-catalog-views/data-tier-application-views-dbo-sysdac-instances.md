---
description: 数据层应用程序视图-dbo.sysdac_instances
title: dbo.sysdac_instances (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysdac_instances_TSQL
- sysdac_instances
- sysdac_instances_TSQL
- dbo.sysdac_instances
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.sysdac_instances
- sysdac_instances
ms.assetid: 28285f3d-3889-439f-8b24-3bdef08e46b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5178fdb50b8a959471c8be973080d1416ba10f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092653"
---
# <a name="data-tier-application-views---dbosysdac_instances"></a>数据层应用程序视图-dbo.sysdac_instances
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为部署到[!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的每个数据层应用程序 (DAC) 实例显示一行。 sysdac_instances 属于 msdb 数据库中的 dbo 架构。 下表描述了 sysdac_instances 视图中的列。  
  
|列名称|数据类型|描述|  
|-----------------|---------------|-----------------|  
|instance_id|**uniqueidentifier**|DAC 实例的标识符。|  
|instance_name|**sysname**|在部署 DAC 时指定的 DAC 实例的名称。|  
|type_name|**sysname**|在创建 DAC 包时指定的 DAC 的名称。|  
|type_version|**nvarchar (64)**|在创建 DAC 包时指定的 DAC 的版本。|  
|说明|**nvarchar(4000)**|在创建 DAC 包时写入的 DAC 的说明。|  
|type_stream|**varbinary(max)**|包含 DAC 中包含的逻辑对象（如表和视图）的已编码表示形式的位流。|  
|date_created|**datetime**|DAC 实例的创建日期和时间。|  
|created_by|**sysname**|创建 DAC 实例的登录名。|  
|database_name|**sysname**|为 DAC 实例创建的数据库的名称。|  
  
## <a name="remarks"></a>备注  
 DAC 包括 DAC 类型，该类型是应用程序使用的逻辑数据层对象（例如表和视图）的定义。 DAC 包是用于部署 DAC 的文件。 DAC 包包含在 DAC 类型中包含的所有逻辑对象的表示形式。 DAC 包可用于将 DAC 的一个或多个副本或实例部署到[!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。 从同一 DAC 包部署的各 DAC 实例共享相同的类型，但分配有唯一的实例名称和标识符。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色的成员身份以便查看所有列。 公共角色的成员可以查看 instance_name、description 和 type_version 列。  
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [数据层应用程序视图 &#40;Transact-sql&#41;]()  
  
