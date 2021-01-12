---
description: RECEIVE (Transact-SQL)
title: RECEIVE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RECEIVE_TSQL
- RECEIVE
dev_langs:
- TSQL
helpviewer_keywords:
- queues [Service Broker], message retrieval
- messages [Service Broker], retrieving
- RECEIVE statement
- receiving messages
- retrieving messages
ms.assetid: 878c6c14-37ab-4b87-9854-7f8f42bac7dd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 840f023e1a906350f34d0709459842a3fe3c796a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98080829"
---
# <a name="receive-transact-sql"></a>RECEIVE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  从队列中检索一条或多条消息。 根据队列保持设置的不同，可以从队列中删除消息或更新队列中消息的状态。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
[ WAITFOR ( ]  
    RECEIVE [ TOP ( n ) ]   
        <column_specifier> [ ,...n ]  
        FROM <queue>  
        [ INTO table_variable ]  
        [ WHERE {  conversation_handle = conversation_handle  
                 | conversation_group_id = conversation_group_id } ]  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<column_specifier> ::=  
{    *   
  |  { column_name | [ ] expression } [ [ AS ] column_alias ]  
  |  column_alias = expression   
}     [ ,...n ]   
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WAITFOR  
 指定如果当前没有出现任何消息，则 RECEIVE 语句将等待消息到达队列。  
  
 TOP( n )  
 指定要返回的最大消息数。 如果未指定此子句，则返回所有满足语句条件的消息。  
  
 \*  
 指定结果集包含队列中的所有列。  
  
 column_name  
 要包含在结果集中的列的名称。  
  
 *expression*  
 列名、常量、函数或由运算符连接的列名、常量和函数的任意组合。  
  
 column_alias  
 用于替换结果集中的列名的可选名称。  
  
 FROM  
 指定包含要检索的消息的队列。  
  
 *database_name*  
 包含从中接收消息的队列的数据库的名称。 如果未提供 database_name，则默认为当前数据库。  
  
 *schema_name*  
 从中接收消息的队列所属架构的名称。 如果未提供 schema_name，则默认为当前用户的默认架构。  
  
 queue_name  
 从中接收消息的队列的名称。  
  
 INTO table_variable  
 指定 RECEIVE 将消息放入到的表变量。 表变量具有的列数必须与消息中的列数相同。 表变量中每列的数据类型必须可隐式转换为消息中对应列的数据类型。 如果未指定 INTO，则消息将作为结果集返回。  
  
 WHERE  
 指定用于已收到的消息的会话或会话组。 如果省略，则从下一个可用会话组返回消息。  
  
 conversation_handle = conversation_handle  
 指定用于已收到的消息的会话。 提供的“会话句柄”必须是 uniqueidentifer，或属于可转换为 uniqueidentifier 的类型。  
  
 conversation_group_id = conversation_group_id  
 指定用于已收到的消息的会话组。 提供的“会话组 ID”必须是 uniqueidentifer，或属于可转换为 uniqueidentifier 的类型。  
  
 TIMEOUT timeout  
 指定语句等待消息的时间（以毫秒为单位）。 此子句只能与 WAITFOR 子句一起使用。 如果未指定该子句，或者超时值为 1，则等待时间将为无限长。 如果超时时间已到，RECEIVE 将返回一个空结果集。  
  
## <a name="remarks"></a>注解  
  
> [!IMPORTANT]  
>  如果 RECEIVE 语句不是批处理或存储过程中的第一个语句，则必须用分号 (;) 终止前面的语句。  
  
 RECEIVE 语句将从队列读取消息并返回结果集。 结果集由零行或多行组成，每行均包含一条消息。 如果未使用 INTO 子句，并且 column_specifier 没有为局部变量分配值，则该语句将结果集返回到调用程序。  
  
 RECEIVE 语句返回的消息可以为不同的消息类型。 应用程序可使用 message_type_name 列将每条消息路由到用来处理关联消息类型的代码。 有两类消息类型：  
  
-   使用 CREATE MESSAGE TYPE 语句创建的应用程序定义消息类型。 会话中允许的应用程序定义消息类型集由为该会话指定的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 约定定义。  
  
-   返回状态信息或错误信息的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 系统消息。  
  
 RECEIVE 语句将从队列中删除已收到的消息，但队列指定消息保持时除外。 当队列的 RETENTION 设置为 ON 时，RECEIVE 语句将 status 列更新为 0，并使消息留在队列中。 当包含 RECEIVE 语句的事务回滚时，对该事务中的队列的所有更改也会随之回滚，并将消息返回到队列。  
  
 RECEIVE 语句返回的所有消息都属于同一会话组 RECEIVE 语句锁定返回的消息所属的会话组，直到包含该语句的事务完成为止。 RECEIVE 语句将返回“状态”为 1 的消息。 将 RECEIVE 语句返回的结果集进行隐式排序：  
  
-   如果来自多个会话的消息均符合 WHERE 子句的条件，则 RECEIVE 语句将先返回来自一个会话的所有消息，然后再返回任何其他会话的消息。 将按优先级降序对这些会话进行处理。  
  
-   对于给定的会话，RECEIVE 语句将按 message_sequence_number 的升序返回消息。  
  
 RECEIVE 语句的 WHERE 子句只能包含一个使用 conversation_handle 或 conversation_group_id 的搜索条件。 搜索条件不能包含队列中的一个或多个其他列。 conversation_handle 或 conversation_group_id 不能为表达式。 返回的消息集取决于在 WHERE 子句中指定的条件。  
  
-   如果指定了 conversation_handle，则 RECEIVE 将从指定会话中返回在队列中可用的所有消息。  
  
-   如果指定了 conversation_group_id，则 RECEIVE 将从属于指定会话组成员的任何会话中返回队列中可用的所有消息。  
  
-   如果没有 WHERE 子句，则 RECEIVE 会确定使用哪些会话组：  
  
    -   在队列中具有一条或多条消息。  
  
    -   未被其他 RECEIVE 语句锁定。  
  
    -   在符合这些条件的所有会话组中具有最高优先级。  
  
     然后，RECEIVE 将从属于所选会话组成员的任何会话中返回队列中可用的所有消息。  
  
 如果在 WHERE 子句中指定的会话句柄或会话组标识符不存在，或者未与指定的队列相关联，则 RECEIVE 语句将返回一个错误。  
  
 如果在 RECEIVE 语句中指定的队列的队列状态设置为 OFF，则该语句将失败，并返回一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 错误。  
  
 指定 WAITFOR 子句时，该语句将等待指定的超时时间，或等到结果集可用为止。 如果在语句等待时删除队列或将队列的状态设置为 OFF，则该语句立即返回一个错误。 如果 RECEIVE 语句指定了会话组或会话句柄，并且用于该会话的服务被删除或移动到其他队列，则 RECEIVE 语句将报告一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 错误。  
  
 RECEIVE 在用户定义函数中无效。  
  
 RECEIVE 语句不具有防止优先级不足的功能。 如果单个 RECEIVE 语句锁定会话组并检索来自低优先级会话的大量消息，则将无法从该组的高优先级会话中收到任何消息。 为避免出现这种情况，在从低优先级会话中检索消息时，可使用 TOP 子句限制每个 RECEIVE 语句检索的消息数目。  
  
## <a name="queue-columns"></a>队列列  
 下表列出了队列中的列：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**status**|**tinyint**|消息的状态。 对于由 RECEIVE 命令返回的消息，其状态始终为 0。 队列中的消息可以包含下列值之一：<br /><br /> 0=就绪 1=已收到消息 2=尚未完成 3=已保留发送的消息|  
|**priority**|**tinyint**|应用于消息的会话优先级。|  
|**queuing_order**|**bigint**|消息在队列中的序号。|  
|**conversation_group_id**|**uniqueidentifier**|此消息所属会话组的标识符。|  
|**conversation_handle**|**uniqueidentifier**|此消息所属会话的句柄。|  
|**message_sequence_number**|**bigint**|消息在会话中的序列号。|  
|**service_name**|**nvarchar(512)**|要进行会话的服务的名称。|  
|**service_id**|**int**|要进行会话的服务的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象标识符。|  
|**service_contract_name**|**nvarchar(256)**|会话遵循的约定的名称。|  
|**service_contract_id**|**int**|会话遵循的约定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象标识符。|  
|**message_type_name**|**nvarchar(256)**|用于说明消息格式的消息类型的名称。 消息可以是应用程序消息类型，也可以是 Broker 系统消息。|  
|**message_type_id**|**int**|对消息进行说明的消息类型的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象标识符。|  
|**validation**|**nchar(2)**|对消息所用的验证。<br /><br /> E=Empty N=None X=XML|  
|**message_body**|**varbinary(MAX)**|消息的内容。|  
  
## <a name="permissions"></a>权限  
 若要接收消息，则当前用户必须对队列有 RECEIVE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-receiving-all-columns-for-all-messages-in-a-conversation-group"></a>A. 接收会话组中所有消息的所有列  
 下面的示例接收 `ExpenseQueue` 队列中下一个可用会话组的所有可用消息。 该语句将消息作为结果集返回。  
  
```sql  
RECEIVE * FROM ExpenseQueue ;  
```  
  
### <a name="b-receiving-specified-columns-for-all-messages-in-a-conversation-group"></a>B. 接收用于会话组中所有消息的指定列  
 下面的示例接收 `ExpenseQueue` 队列中下一个可用会话组的所有可用消息。 该语句将消息作为包含 `conversation_handle`、`message_type_name` 和 `message_body` 列的结果集返回。  
  
```sql  
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue ;  
```  
  
### <a name="c-receiving-the-first-available-message-in-the-queue"></a>C. 接收队列中的第一条可用消息  
 下面的示例将 `ExpenseQueue` 队列中的第一条可用消息作为结果集接收。  
  
```sql  
RECEIVE TOP (1) * FROM ExpenseQueue ;  
```  
  
### <a name="d-receiving-all-messages-for-a-specified-conversation"></a>D. 接收指定会话的所有消息  
 下面的示例将 `ExpenseQueue` 队列中指定会话的所有可用消息作为结果集接收。  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER ;  
  
SET @conversation_handle = <retrieve conversation from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_handle = @conversation_handle ;  
```  
  
### <a name="e-receiving-messages-for-a-specified-conversation-group"></a>E. 接收指定会话组的消息  
 下面的示例将 `ExpenseQueue` 队列中指定会话组的所有可用消息作为结果集接收。  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id =   
    <retrieve conversation group ID from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="f-receiving-into-a-table-variable"></a>F. 接收到表变量  
 下面的示例将 `ExpenseQueue` 队列中指定会话组的所有可用消息接收到表变量。  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
DECLARE @procTable TABLE(  
     service_instance_id UNIQUEIDENTIFIER,  
     handle UNIQUEIDENTIFIER,  
     message_sequence_number BIGINT,  
     service_name NVARCHAR(512),  
     service_contract_name NVARCHAR(256),  
     message_type_name NVARCHAR(256),  
     validation NCHAR,  
     message_body VARBINARY(MAX)) ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database> ;  
  
RECEIVE TOP (1)  
    conversation_group_id,  
    conversation_handle,  
    message_sequence_number,  
    service_name,  
    service_contract_name,  
    message_type_name,  
    validation,  
    message_body  
FROM ExpenseQueue  
INTO @procTable  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="g-receiving-messages-and-waiting-indefinitely"></a>G. 接收消息并无限期等待  
 下面的示例将接收 `ExpenseQueue` 队列中下一个可用会话组的所有可用消息。 该语句将等到至少有一条消息变为可用为止，然后返回一个包含所有消息列的结果集。  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue) ;  
```  
  
### <a name="h-receiving-messages-and-waiting-for-a-specified-interval"></a>H. 接收消息并等待指定的间隔  
 下面的示例将接收 `ExpenseQueue` 队列中下一个可用会话组的所有可用消息。 该语句将等待 60 秒或直到至少有一条消息变为可用为止，以二者中最先发生的为准。 如果至少有一条消息可用，则该语句将返回一个包含所有消息列的结果集。 否则，该语句将返回一个空结果集。  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="i-receiving-messages-modifying-the-type-of-a-column"></a>I. 接收消息，修改列的类型  
 下面的示例将接收 `ExpenseQueue` 队列中下一个可用会话组的所有可用消息。 当消息类型声明该消息包含一个 XML 文档时，该语句便将消息正文转换为 XML。  
  
```sql  
WAITFOR (  
    RECEIVE message_type_name,  
        CASE  
            WHEN validation = 'X' THEN CAST(message_body as XML)  
            ELSE NULL  
         END AS message_body   
         FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="j-receiving-a-message-extracting-data-from-the-message-body-retrieving-conversation-state"></a>J. 接收消息，从消息正文中提取数据，检索会话状态  
 下面的示例将接收 `ExpenseQueue` 队列中下一个可用会话组的下一条可用消息。 当消息的类型为 `//Adventure-Works.com/Expenses/SubmitExpense` 时，该语句将从消息正文中提取雇员 ID 和一个项列表。 该语句还会从 `ConversationState` 表中检索会话的状态。  
  
```sql  
WAITFOR(  
    RECEIVE   
    TOP(1)  
      message_type_name,  
      COALESCE(  
           (SELECT TOP(1) ConversationState  
            FROM CurrentConversations AS cc  
            WHERE cc.ConversationHandle = conversation_handle),  
           'NEW')  
      AS ConversationState,  
      COALESCE(  
          (SELECT TOP(1) ErrorCount  
           FROM CurrentConversations AS cc  
           WHERE cc.ConversationHandle = conversation_handle),   
           0)  
      AS ConversationErrors,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).value(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"  
                   (/rpt:ExpenseReport/rpt:EmployeeID)[1]', 'nvarchar(20)')  
         ELSE NULL  
      END AS EmployeeID,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).query(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"   
                     /rpt:ExpenseReport/rpt:ItemDetail')  
          ELSE NULL  
      END AS ItemList  
    FROM ExpenseQueue   
), TIMEOUT 60000 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [BEGIN CONVERSATION TIMER (Transact-SQL)](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [CREATE CONTRACT (Transact-SQL)](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/create-message-type-transact-sql.md)   
 [SEND (Transact-SQL)](../../t-sql/statements/send-transact-sql.md)   
 [CREATE QUEUE (Transact SQL)](../../t-sql/statements/create-queue-transact-sql.md)   
 [ALTER QUEUE (Transact SQL)](../../t-sql/statements/alter-queue-transact-sql.md)   
 [DROP QUEUE (Transact SQL)](../../t-sql/statements/drop-queue-transact-sql.md)  
  
  
