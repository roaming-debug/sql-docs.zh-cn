---
description: ALTER MESSAGE TYPE (Transact-SQL)
title: ALTER MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_MESSAGE_TYPE_TSQL
- ALTER MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER MESSAGE TYPE statement
- modifying message types
- message types [Service Broker], modifying
ms.assetid: 98c94176-2bdf-4725-b4bc-d33b6b14817d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c8b1cbf21f5875ee32174a37f29dfe4a32fbf195
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092123"
---
# <a name="alter-message-type-transact-sql"></a>ALTER MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改消息类型的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
ALTER MESSAGE TYPE message_type_name  
   VALIDATION =  
    {  NONE   
     | EMPTY   
     | WELL_FORMED_XML   
     | VALID_XML WITH SCHEMA COLLECTION schema_collection_name }  
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *message_type_name*  
 要更改的消息类型的名称。 不能指定服务器、数据库和架构名称。  
  
 VALIDATION  
 指定 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对此类型消息的消息正文的验证方式。  
  
 无  
 不执行任何验证。 消息正文可以包含任何数据，也可以为 NULL。  
  
 EMPTY  
 消息正文必须为 NULL。  
  
 WELL_FORMED_XML  
 消息正文必须包含格式正确的 XML。  
  
 VALID_XML_WITH_SCHEMA = *schema_collection_name*  
 消息正文必须包含符合指定架构集合中的某一架构的 XML。 *schema_collection_name* 必须是现有 XML 架构集合的名称。  
  
## <a name="remarks"></a>注解  
 更改消息类型的验证不会影响已传递到队列的消息。  
  
 若要更改消息类型的 AUTHORIZATION，请使用 ALTER AUTHORIZATION 语句。  
  
## <a name="permissions"></a>权限  
 默认情况下，消息类型的所有者、**db_ddladmin** 或 **db_owner** 固定数据库角色以及 **sysadmin** 固定服务器角色的成员拥有更改消息类型的权限。  
  
 如果 ALTER MESSAGE TYPE 语句指定了一个架构集合，则执行该语句的用户必须对指定的架构集合具有 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
 以下示例更改消息类型 `//Adventure-Works.com/Expenses/SubmitExpense`，以要求消息正文包含格式正确的 XML 文档。  
  
```sql  
ALTER MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = WELL_FORMED_XML ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [CREATE MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/create-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
