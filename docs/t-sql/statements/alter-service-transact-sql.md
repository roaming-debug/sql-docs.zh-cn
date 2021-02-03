---
description: ALTER SERVICE (Transact-SQL)
title: ALTER SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SERVICE
- ALTER_SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying services
- contracts [Service Broker], modifying
- ALTER SERVICE statement
- services [Service Broker], modifying
ms.assetid: 2b4608f7-bb2e-4246-aa29-b52c55995b3a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c74ca3071940e198b48f0f49e8a08e57a472e4c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182166"
---
# <a name="alter-service-transact-sql"></a>ALTER SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改现有的服务。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
ALTER SERVICE service_name   
   [ ON QUEUE [ schema_name . ]queue_name ]   
   [ ( < opt_arg > [ , ...n ] ) ]  
[ ; ]  
  
<opt_arg> ::=  
   ADD CONTRACT contract_name | DROP CONTRACT contract_name  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 service_name  
 要更改的服务的名称。 不能指定服务器、数据库和架构名称。  
  
 ON QUEUE [ schema_name. ] queue_name  
 为此服务指定新队列。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将此服务的所有消息从当前队列移动到新队列。  
  
 ADD CONTRACT *contract_name*  
 指定要添加到由此服务公开的约定集中的约定。  
  
 DROP CONTRACT *contract_name*  
 指定要从由此服务公开的约定集中删除的约定。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将对使用该约定的、与此服务进行的所有现有会话发送错误消息。  
  
## <a name="remarks"></a>备注  
 当 ALTER SERVICE 语句从某个服务中删除一条约定后，此服务便不可再作为使用该约定的会话的目标。 因此，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 将不允许使用该约定与此服务建立新会话。 使用该约定的现有会话不受影响。  
  
 若要更改服务的 AUTHORIZATION，请使用 ALTER AUTHORIZATION 语句。  
  
## <a name="permissions"></a>权限  
 默认情况下，服务所有者、db_ddladmin 或 db_owner 固定数据库角色的成员以及 sysadmin 固定服务器角色的成员拥有更改服务的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-queue-for-a-service"></a>A. 更改服务队列  
 下面的示例将 `//Adventure-Works.com/Expenses` 服务更改为使用队列 `NewQueue`。  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE NewQueue ;  
```  
  
### <a name="b-adding-a-new-contract-to-the-service"></a>B. 向服务中添加新约定  
 下面的示例将 `//Adventure-Works.com/Expenses` 服务更改为允许在 `//Adventure-Works.com/Expenses` 约定上进行对话。  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="c-adding-a-new-contract-to-the-service-dropping-existing-contract"></a>C. 向此服务中添加新约定，并删除现有约定  
 下面的示例将 `//Adventure-Works.com/Expenses` 服务更改为允许在 `//Adventure-Works.com/Expenses/ExpenseProcessing` 约定上进行对话，而不允许在 `//Adventure-Works.com/Expenses/ExpenseSubmission` 约定上进行对话。  
  
```sql  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing],   
     DROP CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SERVICE (Transact SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP SERVICE (Transact-SQL)](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
