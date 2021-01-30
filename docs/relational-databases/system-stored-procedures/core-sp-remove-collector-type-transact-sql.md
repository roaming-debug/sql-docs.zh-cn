---
description: core.sp_remove_collector_type (Transact-SQL)
title: core.sp_remove_collector_type (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_remove_collector_type
- sp_remove_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- core.sp_remove_collector_type stored procedure
- management data warehouse, data collector stored procedures
- sp_remove_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 88ceba25-e41a-405f-a416-bb68918a0024
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 537a933dfd53a3ed227c14e2945c747e61793aed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187985"
---
# <a name="coresp_remove_collector_type-transact-sql"></a>core.sp_remove_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从管理数据仓库数据库中的 core.supported_collector_types 视图中删除一个条目。 必须在管理数据仓库数据库的上下文中执行此过程。  
  
 core.supported_collector_types 视图显示可以向管理数据仓库上载数据的已注册收集器类型。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
core.sp_remove_collector_type [ @collector_type_uid = ] 'collector_type_uid'  
```  
  
## <a name="arguments"></a>参数  
 [ @collector_type_uid =] "*collector_type_uid*"  
 收集器类型的 GUID。 *collector_type_uid* 是 **uniqueidentifier**，没有默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="permissions"></a>权限  
 需要具有 EXECUTE 权限的 **mdw_admin** (中的成员资格) 固定数据库角色。  
  
## <a name="examples"></a>示例  
 下面的示例从 core.supported_collector_types 视图中删除一般 T-SQL 查询收集器类型。  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @RC int;  
DECLARE @collector_type_uid uniqueidentifier;  
SELECT @collector_type_uid = (SELECT collector_type_uid FROM msdb.dbo.syscollector_collector_types WHERE name = N'Generic T-SQL Query Collector Type');  
EXECUTE @RC = core.sp_remove_collector_type @collector_type_uid;  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [管理数据仓库](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
