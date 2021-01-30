---
description: 'sys.security_predicates (Transact-sql) '
title: sys.security_predicates (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_PREDICATES
- SECURITY_PREDICATES
- SECURITY_PREDICATES_TSQL
- SYS.SECURITY_PREDICATES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_predicates catalog view
- security_predicates catalog view
ms.assetid: c7a2f28c-98da-463d-8b8a-8e5619e2c6a6
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8220ca6cefc82682201e3b06244cbed5a7f27db1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182491"
---
# <a name="syssecurity_predicates-transact-sql"></a>sys.security_predicates (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  为数据库中的每个安全谓词返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|包含此谓词的安全策略的 ID。|  
|security_predicate_id|**int**|此安全策略内的谓词 ID。|  
|target_object_id|**int**|安全性谓词所绑定的对象的 ID。|  
|predicate_definition|**nvarchar(max)**|将用作安全性谓词的函数的完全限定名称，包括参数。 请注意，`schema.function` 名称可以规范化（即转义），也可以是一致性文本中的任何其他元素。 例如：<br /><br /> `[dbo].[fn_securitypredicate]([wing], [startTime], [endTime])`|  
|predicate_type|**int**|安全策略使用的谓词类型：<br /><br /> 0 = 筛选器谓词<br /><br /> 1 = 阻止谓词|  
|predicate_type_desc|**nvarchar(60)**|安全策略使用的谓词类型：<br /><br /> FILTER<br /><br /> 阻止|  
|operation|**int**|为谓词指定的操作类型：<br /><br /> NULL = 所有适用的操作<br /><br /> 1 = 插入后<br /><br /> 2 = 更新后<br /><br /> 3 = 更新之前<br /><br /> 4 = 删除之前|  
|operation_desc|**nvarchar(60)**|为谓词指定的操作类型：<br /><br /> Null<br /><br /> 插入后<br /><br /> AFTER UPDATE<br /><br /> 更新前<br /><br /> 删除之前|  
  
## <a name="permissions"></a>权限  
 具有 " **更改任意安全策略** " 权限的主体有权访问此目录视图中的所有对象以及对该对象具有 **view DEFINITION** 的任何人。  
  
## <a name="see-also"></a>另请参阅  
 [行级安全性](../../relational-databases/security/row-level-security.md)   
 [sys.security_policies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
