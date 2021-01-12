---
title: KILL QUERY NOTIFICATION SUBSCRIPTION
description: 从实例中删除查询通知订阅。 该语句可以删除特定订阅或所有订阅。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- KILL QUERY NOTIFICATION SUBSCRIPTION
- KILL_QUERY_NOTIFICATION_SUBSCRIPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- KILL QUERY NOTIFICATION SUBSCRIPTION statement
- removing subscriptions
- subscriptions [SQL Server query notifications], stopping
- query notifications [SQL Server], subscriptions
ms.assetid: 8aeadf51-286c-4748-bef2-d25858b250bf
author: cawrites
ms.author: chadam
ms.openlocfilehash: f9ba5c75e44fcedd0f99e051b94812dbd3d94e5e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98084720"
---
# <a name="kill-query-notification-subscription-transact-sql"></a>KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从实例中删除查询通知订阅。 该语句可以删除特定订阅或所有订阅。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
KILL QUERY NOTIFICATION SUBSCRIPTION   
   { ALL | subscription_id }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ALL  
 删除实例中的所有订阅。  
  
 subscription_id   
 删除订阅 ID 为 subscription_id 的订阅  。  
  
## <a name="remarks"></a>备注  
 KILL QUERY NOTIFICATION SUBSCRIPTION 语句删除查询通知订阅，而不生成通知消息。  
  
 subscription_id 是动态管理视图 [sys.dm_qn_subscriptions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/query-notifications-sys-dm-qn-subscriptions.md) 中显示的订阅 ID  。  
  
 如果指定的订阅 ID 不存在，该语句将生成错误。  
  
## <a name="permissions"></a>权限  
 该语句的执行权限只限于 sysadmin 固定服务器角色的成员  。  
  
## <a name="examples"></a>示例  
  
### <a name="a-removing-all-query-notification-subscriptions-in-the-instance"></a>A. 删除实例中的所有查询通知订阅  
 下例将删除实例中的所有查询通知订阅。  
  
```sql  
KILL QUERY NOTIFICATION SUBSCRIPTION ALL ;  
```  
  
### <a name="b-removing-a-single-query-notification-subscription"></a>B. 删除单个查询通知订阅  
 下例将删除 ID 为 `73` 的查询通知订阅。  
  
```sql  
KILL QUERY NOTIFICATION SUBSCRIPTION 73 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_qn_subscriptions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/query-notifications-sys-dm-qn-subscriptions.md)  
  
  
