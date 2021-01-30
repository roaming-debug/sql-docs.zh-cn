---
description: " (Transact-sql) 的系统兼容性视图"
title: 系统兼容性视图 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- compatibility views [SQL Server]
- system tables [SQL Server], compatibility views
- type IDs [SQL Server]
- system views [SQL Server], compatibility
- metadata [SQL Server], views
- backward compatibility [SQL Server], compatibility views
- compatibility [SQL Server], compatibility views
- backward compatibility [SQL Server], system tables
- compatibility [SQL Server], system tables
- user IDs [SQL Server]
ms.assetid: 8e4624f5-9d36-4ce7-9c9e-1fe010fa2122
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ff5bb1e1c95c2cd1429e101f4ebebb5dffda10a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160637"
---
# <a name="system-compatibility-views-transact-sql"></a> (Transact-sql) 的系统兼容性视图
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本中的许多系统表现在都作为一组视图实现。 这些视图称为兼容性视图，仅用于向后兼容。 兼容性视图公开的元数据在 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 中也提供。 但是，兼容性视图不公开与在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本中引入的功能有关的任何元数据。 因此，当您使用新功能（例如 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 或分区）时，必须切换到使用目录视图。  
  
 升级到目录视图的另一个原因是，存储用户 ID 和类型 ID 的兼容性视图列可能返回 NULL 或触发算术溢出。 这是因为您可以创建超过 32,767 个用户、组和角色，以及超过 32,767 种数据类型。 例如，如果您要创建32768用户，然后运行以下查询： `SELECT * FROM sys.sysusers` 。 如果 ARITHABORT 设置为 ON，则查询会失败，并出现算术溢出错误。 如果 ARITHABORT 设置为 OFF，则 **uid** 列返回 NULL。  
  
 若要避免这些问题，建议您使用新增的目录视图，这些视图可以处理增加的用户 ID 和类型 ID 数目。 下表列出了会出现此溢出的列。  
  
|列名称|兼容性视图|SQL Server 2005 视图|  
|-----------------|------------------------|--------------------------|  
|**xusertype**|**syscolumns**|**sys.columns**|  
|**usertype**|**syscolumns**|**sys.columns**|  
|**memberuid**|**sysmembers**|**sys.database_role_members**|  
|**groupuid**|**sysmembers**|**sys.database_role_members**|  
|**标识号**|**sysobjects**|**sys.objects**|  
|**标识号**|**sysprotects**|**sys.database_permissions**<br /><br /> **sys.server_permissions**|  
|**授权者**|**sysprotects**|**sys.database_permissions**<br /><br /> **sys.server_permissions**|  
|**xusertype**|**systypes**|**sys.types**|  
|**标识号**|**systypes**|**sys.types**|  
|**标识号**|**sysusers**|**sys.database_principals**|  
|**altuid**|**sysusers**|**sys.database_principals**|  
|**gid**|**sysusers**|**sys.database_principals**|  
|**标识号**|**syscacheobjects**|**sys.dm_exec_plan_attributes**|  
|**标识号**|**sysprocesses**|**sys.dm_exec_requests**|  
  
 在用户数据库中引用时，在 SQL Server 2000 (（如 **sys.syslanguages** 或 **sys.syscacheobjects**) ）中被公布为弃用的系统表现在将绑定到 **sys** 架构中的后向兼容性视图。 因为多个版本均已不推荐使用 SQL Server 2000 系统表，此更改不被视为重大更改。  
  
 示例：如果用户在用户数据库中创建名为 **sys.syslanguages** 的用户表，则在 SQL Server 2008 中， `SELECT * from dbo.syslanguages;` 该数据库中的语句将返回用户表中的值。 从 SQL Server 2012 开始，此做法将从系统视图 **sys.sys语言** 返回数据。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
