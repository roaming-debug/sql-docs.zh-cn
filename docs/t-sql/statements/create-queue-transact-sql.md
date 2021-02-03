---
description: CREATE QUEUE (Transact-SQL)
title: CREATE QUEUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- QUEUE_TSQL
- CREATE QUEUE
- QUEUE
- CREATE_QUEUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE QUEUE statement
- internal activation [Service Broker]
- stored procedure activation [Service Broker]
- message retention [Service Broker]
- unavailable queues [Service Broker]
- activation stored procedures [Service Broker]
- queues [Service Broker], creating
ms.assetid: fce80faf-2bdc-475d-8ca1-31438ed41fb0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 42b5b42ee88c6d562f1ebf8c03f92a1bd4be54f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188537"
---
# <a name="create-queue-transact-sql"></a>CREATE QUEUE (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

在数据库中创建一个新队列。 队列存储消息。 当一条针对某项服务的消息到达时，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 会将该消息放入与该服务关联的队列中。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
CREATE QUEUE <object>
   [ WITH
     [ STATUS = { ON | OFF } [ , ] ]
     [ RETENTION = { ON | OFF } [ , ] ]
     [ ACTIVATION (
         [ STATUS = { ON | OFF } , ]
           PROCEDURE_NAME = <procedure> ,
           MAX_QUEUE_READERS = max_readers ,
           EXECUTE AS { SELF | 'user_name' | OWNER }
            ) [ , ] ]
     [ POISON_MESSAGE_HANDLING (
         [ STATUS = { ON | OFF } ] ) ]
    ]
     [ ON { filegroup | [ DEFAULT ] } ]
[ ; ]

<object> ::=
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }

<procedure> ::=
{ database_name.schema_name.stored_procedure_name | schema_name.stored_procedure_name | stored_procedure_name }

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

*database_name* (object) 要在其中创建新队列的数据库的名称。 database_name 须指定现有数据库的名称。 如果未提供 database_name，将在当前数据库中创建队列。

*schema_name* (object) 新队列所属架构的名称。 默认情况下，该架构为执行该语句的用户的默认架构。 如果由 sysadmin 固定服务器角色成员或者 database_name 指定的数据库中的 db_dbowner 或 db_ddladmin 固定数据库角色成员执行 CREATE QUEUE 语句，则 schema_name 可以指定与当前连接的登录名关联的架构以外的架构 。 否则，schema_name 必须是执行该语句的用户的默认架构。

*queue_name* 要创建的队列的名称。 此名称必须符合有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符指南。

STATUS (Queue) 指定队列是可用 (ON) 还是不可用 (OFF)。 如果队列不可用，则不能向队列添加消息或从队列中删除消息。 可以以不可用状态创建队列，从而在使用 ALTER QUEUE 语句将该队列更改为可用之前，使消息无法到达该队列。 如果省略此子句，则默认值为 ON，该队列可用。

RETENTION 指定队列的保留期设置。 如果 RETENTION = ON，则使用此队列的会话发送或接收的所有消息都将保持在队列中，直到会话结束为止。 这样就可以保留消息以便审核，或者用于在出现错误时执行补偿事务。 如果未指定此子句，则默认的保留设置为 OFF。

> [!NOTE]
> 设置 RETENTION = ON 会降低性能。 应仅在应用程序需要时才使用此设置。

ACTIVATION 指定与为了处理此队列中的消息而必须启动的存储过程有关的信息。

STATUS (Activation) 指定 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 是否启动存储过程。 当 STATUS = ON 时，如果当前运行的过程数小于 MAX_QUEUE_READERS，并且消息抵达队列的速度比存储过程接收消息的速度快，则队列启动用 PROCEDURE_NAME 指定的存储过程。 当 STATUS = OFF 时，队列不启动存储过程。 如果未指定该子句，则默认为 ON。

PROCEDURE_NAME = \<procedure> 指定处理此队列中的消息时要启动的存储过程的名称。 此值必须是一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符。

*database_name*(procedure) 包含存储过程的数据库的名称。

*schema_name*(procedure) 包含存储过程的架构的名称。

*procedure_name* 存储过程的名称。

MAX_QUEUE_READERS =*max_readers* 指定队列同时启动的激活存储过程的最大实例数。 max_readers 值必须是 0 到 32767 之间的数字 。

EXECUTE AS 指定用于运行激活存储过程的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库用户帐户。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须能够在队列启动存储过程时检查此用户的权限。 对于域用户，在启动过程时服务器必须与域连接，否则激活将失败。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户，服务器始终可检查权限。

SELF 指定存储过程以当前用户身份执行。 （执行该 CREATE QUEUE 语句的数据库主体。）

'*user_name*' 执行存储过程时所用的用户名。 user_name 参数必须是指定为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符的有效 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户。 当前用户必须对指定的 user_name 具有 IMPERSONATE 权限。

OWNER 指定存储过程以队列所有者身份执行。

POISON_MESSAGE_HANDLING 指定是否为队列启用有害消息处理。 默认值为 ON。

将有害消息处理设置为 OFF 的队列在五个连续的事务回滚之后不会被禁用。 这样，应用程序就可以定义自定义的有害消息处理系统。

ON *filegroup |* [**DEFAULT**] 指定从中创建此队列的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件组。 可使用 filegroup 参数标识文件组，也可使用 DEFAULT 标识符以使用 Service Broker 数据库的默认文件组。 在此子句的上下文中，DEFAULT 不是关键字，必须作为标识符进行分隔。 如果未指定文件组，该队列使用数据库的默认文件组。

## <a name="remarks"></a>备注

队列可以是 SELECT 语句的目标。 但是，只能使用在 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话中运行的语句（如 SEND、RECEIVE 和 END CONVERSATION）来修改队列的内容。 队列不能是 INSERT、UPDATE、DELETE 或 TRUNCATE 语句的目标。

队列可能不是临时对象。 因此，以 # 开头的队列名称无效。

通过以不可用状态创建队列，可以先准备好服务的基础结构，然后再允许在队列中接收消息。

如果队列中没有消息，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 不会停止激活存储过程。 如果队列中在短时间内没有可用消息，应退出激活存储过程。

在 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 启动存储过程时将检查激活存储过程的权限，而不是在创建队列时检查。 CREATE QUEUE 语句不验证 EXECUTE AS 子句中指定的用户是否有权限执行 PROCEDURE NAME 子句中指定的存储过程。

队列不可用时，[!INCLUDE[ssSB](../../includes/sssb-md.md)] 将在数据库的传输队列中保存使用该队列的服务的消息。 sys.transmission_queue 目录视图提供传输队列的视图。

队列是一种属于架构的对象。 队列显示在 sys.objects 目录视图中。

下表列出了队列中的列。

|列名称|数据类型|说明|
|-----------------|---------------|-----------------|
|status|**tinyint**|消息的状态。 RECEIVE 语句将返回状态为 1 的所有消息。 如果启用了消息保持，则将状态设置为 0。 如果禁用了消息保持，则将消息从队列中删除。 队列中的消息可包含下列任一值：<br /><br /> 0=保留收到的消息<br /><br /> 1=准备接收<br /><br /> 2=尚未完成<br /><br /> 3=保留发送的消息|
|priority|**tinyint**|为此消息指定的优先级。|
|queuing_order|**bigint**|消息在队列中的序号。|
|conversation_group_id|**uniqueidentifier**|此消息所属会话组的标识符。|
|conversation_handle|**uniqueidentifier**|此消息所属会话的句柄。|
|message_sequence_number|**bigint**|消息在会话中的序列号。|
|service_name|**nvarchar(512)**|要进行会话的服务的名称。|
|service_id|**int**|要进行会话的服务的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象标识符。|
|service_contract_name|**nvarchar(256)**|会话遵循的约定的名称。|
|service_contract_id|**int**|会话遵循的约定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象标识符。|
|message_type_name|**nvarchar(256)**|对消息进行说明的消息类型的名称。|
|message_type_id|**int**|对消息进行说明的消息类型的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象标识符。|
|validation|**nchar(2)**|对消息所用的验证。<br /><br /> E = 空<br /><br /> N=None<br /><br /> X=XML|
|message_body|**varbinary(max)**|消息的内容。|
|message_id|**uniqueidentifier**|消息的唯一标识符。|

## <a name="permissions"></a>权限

`db_ddladmin` 或 `db_owner` 固定数据库角色或 `sysadmin` 固定服务器角色的成员拥有创建队列的权限。

默认情况下，队列所有者、`db_ddladmin` 或 `db_owner` 固定数据库角色的成员或 `sysadmin` 固定服务器角色的成员拥有队列的 `REFERENCES` 权限。

默认情况下，队列所有者、`db_owner` 固定数据库角色的成员或 `sysadmin` 固定服务器角色的成员具有队列的 `RECEIVE` 权限。

## <a name="examples"></a>示例

### <a name="a-creating-a-queue-with-no-parameters"></a>A. 不使用参数创建队列

下面的示例创建一个可用于接收消息的队列。 没有为该队列指定激活存储过程。

```sql
CREATE QUEUE ExpenseQueue ;
```

### <a name="b-creating-an-unavailable-queue"></a>B. 创建不可用队列

以下示例创建了一个不可用于接收消息的队列。 没有为该队列指定激活存储过程。

```sql
CREATE QUEUE ExpenseQueue WITH STATUS=OFF ;
```

### <a name="c-creating-a-queue-and-specify-internal-activation-information"></a>C. 创建队列并指定内部激活信息

下面的示例创建一个可用于接收消息的队列。 消息进入队列时，队列将启动存储过程 `expense_procedure`。 该存储过程以用户 `ExpenseUser` 的身份执行。 该队列最多启动存储过程的 `5` 个实例。

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS=ON,
    ACTIVATION (
        PROCEDURE_NAME = expense_procedure
        , MAX_QUEUE_READERS = 5
        , EXECUTE AS 'ExpenseUser' ) ;
```

### <a name="d-creating-a-queue-on-a-specific-filegroup"></a>D. 在特定文件组中创建一个队列

以下示例将在文件组 `ExpenseWorkFileGroup` 中创建一个队列。

```sql
CREATE QUEUE ExpenseQueue
    ON ExpenseWorkFileGroup ;
```

### <a name="e-creating-a-queue-with-multiple-parameters"></a>E. 创建具有多个参数的队列

以下示例在 `DEFAULT` 文件组中创建一个队列。 该队列不可用。 消息被保留在队列中，直到消息所属的会话结束。 通过 ALTER QUEUE 启用队列后，该队列将启动存储过程 `2008R2.dbo.expense_procedure` 来处理消息。 此存储过程以运行 `CREATE QUEUE` 语句的用户的身份执行。 该队列最多启动存储过程的 `10` 个实例。

```sql
CREATE QUEUE ExpenseQueue
    WITH STATUS = OFF
      , RETENTION = ON
      , ACTIVATION (
          PROCEDURE_NAME = AdventureWorks2012.dbo.expense_procedure
          , MAX_QUEUE_READERS = 10
          , EXECUTE AS SELF )
    ON [DEFAULT];
```

## <a name="see-also"></a>另请参阅

- [ALTER QUEUE (Transact-SQL)](../../t-sql/statements/alter-queue-transact-sql.md)
- [CREATE SERVICE (Transact SQL)](../../t-sql/statements/create-service-transact-sql.md)
- [DROP QUEUE (Transact SQL)](../../t-sql/statements/drop-queue-transact-sql.md)
- [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)
- [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
