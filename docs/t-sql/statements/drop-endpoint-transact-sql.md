---
description: DROP ENDPOINT (Transact-SQL)
title: DROP ENDPOINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_ENDPOINT_TSQL
- DROP ENDPOINT
dev_langs:
- TSQL
helpviewer_keywords:
- removing endpoints
- endpoints [SQL Server], removing
- deleting endpoints
- DROP ENDPOINT statement
- dropping endpoints
ms.assetid: 6aca7412-66a5-4fa4-86b2-061512ff2080
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1682eb03ed6f2997168ce0c4aa84ff5d1190ce6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171643"
---
# <a name="drop-endpoint-transact-sql"></a>DROP ENDPOINT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除现有的端点。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP ENDPOINT endPointName  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 endPointName  
 要删除的端点的名称。  
  
## <a name="remarks"></a>备注  
 不能在用户事务中执行 ENDPOINT DDL 语句。  
  
## <a name="permissions"></a>权限  
 用户必须是 **sysadmin** 固定服务器角色成员、端点的所有者，或必须被授予对端点的 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下示例删除先前创建的名为 `sql_endpoint` 的端点。  
  
```sql  
DROP ENDPOINT sql_endpoint;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
