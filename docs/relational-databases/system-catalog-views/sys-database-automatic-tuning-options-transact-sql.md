---
title: sys.database_automatic_tuning_options (Transact-sql) |Microsoft Docs
description: 了解如何在 SQL 数据库上查看自动优化选项。 查看所需权限并查看其他可用资源。
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4da712a23dde26d12164957718c3bdfbf89eb487
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475218"
---
# <a name="sysdatabase_automatic_tuning_options-transact-sql"></a>sys. 数据库 \_ 自动 \_ Tuning_options (transact-sql) 
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  返回此数据库的自动优化选项。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|自动优化选项的名称。 请参阅 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) 了解可用选项。|  
|**desired_state**|**smallint**|指示自动优化选项的所需操作模式，由用户显式设置。<br />0 = OFF<br />1 = ON|  
|**desired_state_desc**|**nvarchar(60)**|自动优化选项所需操作模式的文本说明。<br />OFF<br />ON|  
|**actual_state**|**smallint**|指示自动优化选项的操作模式。<br />0 = OFF<br />1 = ON|  
|**actual_state_desc**|**nvarchar(60)**|自动优化选项的实际操作模式的文本说明。<br />OFF<br />ON|  
|**reason**|**smallint**|指示实际和所需状态的不同原因。<br />2 = 已禁用<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|实际和所需状态不同的原因的文本说明。<br />DISABLED = 选项已被系统禁用<br />QUERY_STORE_OFF = 查询存储已关闭<br />QUERY_STORE_READ_ONLY = 查询存储处于只读模式<br />NOT_SUPPORTED = 仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise edition| 
  
## <a name="permissions"></a>权限  
 需要 `VIEW DATABASE STATE` 权限。  
  
## <a name="see-also"></a>另请参阅  
 [自动优化](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [将数据库集 AUTOMATIC_TUNING &#40;Transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 
