---
description: 'sys.security_policies (Transact-sql) '
title: sys.security_policies (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_POLICIES_TSQL
- SECURITY_POLICIES_TSQL
- SYS.SECURITY_POLICIES
- SECURITY_POLICIES
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_policies catalog view
- security_policies catalog view
ms.assetid: 35362f5b-e601-4049-9e1d-c5307e823831
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c33e75e803b7b5d9972d373f4cc5738601197860
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182498"
---
# <a name="syssecurity_policies-transact-sql"></a>sys.security_policies (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  为数据库中的每个安全策略返回一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|安全策略的名称，在数据库中是唯一的。|  
|object_id|**int**|安全策略的 ID。|  
|principal_id|**int**|注册到数据库的安全策略所有者的 ID。 如果通过架构确定所有者，则为 NULL。|  
|schema_id|**int**|对象所在架构的 ID。|  
|parent_object_id|**int**|策略所属对象的 ID。 必须为 0。|  
|type|**vachar (2)**|必须为 **SP**。|  
|type_desc|**nvarchar(60)**|**SECURITY_POLICY**。|  
|create_date|**datetime**|所创建的安全策略的 UTC 日期。|  
|modify_date|**datetime**|最近一次修改的安全策略的 UTC 日期。|  
|is_ms_shipped|**bit**|始终为 false。|  
|is_enabled|**bit**|安全策略规范状态：<br /><br /> 0 = 已禁用<br /><br /> 1 = 已启用|  
|is_not_for_replication|**bit**|策略是使用 NOT FOR REPLICATION 选项创建的。|  
|uses_database_collation|**bit**|使用与数据库相同的排序规则。|  
|is_schemabinding_enabled|**bit**|安全策略的 Schemabinding 状态：<br /><br /> 0或 NULL = 已启用<br /><br /> 1 = 禁用|  
  
## <a name="permissions"></a>权限  
 具有 " **更改任意安全策略** " 权限的主体有权访问此目录视图中的所有对象以及对该对象具有 **view DEFINITION** 的任何人。  
  
## <a name="see-also"></a>另请参阅  
 [行级安全性](../../relational-databases/security/row-level-security.md)   
 [sys.security_predicates &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)   
 [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
