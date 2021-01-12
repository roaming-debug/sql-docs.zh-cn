---
description: DROP SERVICE (Transact-SQL)
title: DROP SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_SERVICE_TSQL
- DROP SERVICE
dev_langs:
- TSQL
helpviewer_keywords:
- deleting services
- services [Service Broker], removing
- dropping services
- DROP SERVICE statement
- removing services
ms.assetid: 2351bba7-0f2a-4cda-b3b2-6a88b8747c53
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0aa5475984945592d968b237c1129a4f5b55d6b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099415"
---
# <a name="drop-service-transact-sql"></a>DROP SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  删除现有的服务。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP SERVICE service_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 service_name  
 要删除的服务的名称。 不能指定服务器、数据库和架构名称。  
  
## <a name="remarks"></a>注解  
 如果任何会话优先级引用了某个服务，则不能删除该服务。  
  
 删除服务将从该服务使用的队列中删除该服务的所有消息。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将向使用该服务的所有已打开会话的远程端发送错误。  
  
## <a name="permissions"></a>权限  
 删除服务的权限默认授予该服务的所有者、db_ddladmin 或 db_owner 固定数据库角色的成员和 sysadmin 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例将删除 `//Adventure-Works.com/Expenses` 服务。  
  
```sql  
DROP SERVICE [//Adventure-Works.com/Expenses] ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [ALTER SERVICE (Transact-SQL)](../../t-sql/statements/alter-service-transact-sql.md)   
 [CREATE SERVICE (Transact SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
