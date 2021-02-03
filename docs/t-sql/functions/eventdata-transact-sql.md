---
description: EVENTDATA (Transact-SQL)
title: EVENTDATA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EVENTDATA
- fn_event_data
- EVENTDATA_TSQL
- fn_event_data_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server instance event data [SQL Server]
- event notifications [SQL Server], event status
- events [SQL Server], status information
- EVENTDATA function
- status information [SQL Server], events
- DDL triggers, returning event data
ms.assetid: 03a80e63-6f37-4b49-bf13-dc35cfe46c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: c1c09426976f6356c18c1bbc917599b2d22c1801
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174428"
---
# <a name="eventdata-transact-sql"></a>EVENTDATA (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回有关服务器或数据库事件的信息。 激发事件通知且指定的 Service Broker 收到结果时，将调用 `EVENTDATA`。 DDL 或登录触发器还支持在内部使用 `EVENTDATA`。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
EVENTDATA( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注  
只有直接在 DDL 或登录触发器内部引用 `EVENTDATA` 时，它才会返回数据。 如果其他例程调用 `EVENTDATA`，则它返回 null，即使 DDL 或登录触发器调用这些例程时也是如此。
  
当执行以下事务时，由 `EVENTDATA` 返回的数据无效

+ 显式调用 `EVENTDATA`
+ 隐式调用 `EVENTDATA`
+ 提交
+ 已回滚  
  
> [!CAUTION]  
>  `EVENTDATA` 返回 XML 数据，该数据将以 Unicode 的形式（其中每个字符包含 2 个字节）发送到客户端。 `EVENTDATA` 返回可表示这些 Unicode 码位的 XML：  
>   
>  `0x0009`  
>   
>  `0x000A`  
>   
>  `0x000D`  
>   
>  `>= 0x0020 && <= 0xD7FF`  
>   
>  `>= 0xE000 && <= 0xFFFD`  
>   
>  某些可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符和数据中显示的字符不能或不允许在 XML 中使用。 其码位没有显示在前面列表中的字符或数据将映射为问号 (?)。  
  
密码在 `CREATE LOGIN` 或 `ALTER LOGIN` 语句执行时不会显示。 这可以保护登录安全性。  
  
## <a name="schemas-returned"></a>返回的架构  
EVENTDATA 返回数据类型为 xml 的值。 默认情况下，所有事件的架构定义都安装在此目录中：[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Tools\Binn\schemas\sqlserver\2006\11\events\events.xsd。  
  
[Microsoft SQL Server XML 架构](https://go.microsoft.com/fwlink/?LinkID=31850)网页还具有事件架构。  
  
若要提取任何特定事件的架构，请搜索复杂类型 `EVENT_INSTANCE_<event_type>` 的架构。 例如，若要提取 `DROP_TABLE` 事件的架构，请搜索 `EVENT_INSTANCE_DROP_TABLE` 的架构。  
  
## <a name="examples"></a>示例  
  
### <a name="a-querying-event-data-in-a-ddl-trigger"></a>A. 在 DDL 触发器中查询事件数据  
此示例创建阻止创建新数据库表的 DDL 触发器。 对 `EVENTDATA` 生成的 XML 数据使用 XQuery 可捕获激发触发器的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 请参阅 [XQuery 语言参考 (SQL Server)](../../xquery/xquery-language-reference-sql-server.md) 以了解详细信息。  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的“以网格显示结果”来查询 `<TSQLCommand>` 元素时，命令文本中的分行符不会出现。 请改用“以文本格式显示结果”。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER safety   
ON DATABASE   
FOR CREATE_TABLE   
AS   
    PRINT 'CREATE TABLE Issued.'  
    SELECT EVENTDATA().value  
        ('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
   RAISERROR ('New tables cannot be created in this database.', 16, 1)   
   ROLLBACK  
;  
GO  
--Test the trigger.  
CREATE TABLE NewTable (Column1 INT);  
GO  
--Drop the trigger.  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
> [!NOTE]  
>  如果需要返回事件数据，使用 XQuery value() 方法而不是 query() 方法。 query() 方法可在输出中返回 XML 和以“and”符转义的回车符和换行符 (CR/LF) 实例，而 value() 方法无法在输出中呈现 CR/LF 实例。  
  
### <a name="b-creating-a-log-table-with-event-data-in-a-ddl-trigger"></a>B. 创建事件数据在 DDL 触发器中的日志表  
此示例创建用于存储有关所有数据库级事件的信息的表，并在表中填充 DDL 触发器。 对 `EVENTDATA` 生成的 XML 数据使用 XQuery 可捕获事件类型和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
```sql 
USE AdventureWorks2012;  
GO  
CREATE TABLE ddl_log (PostTime DATETIME, DB_User NVARCHAR(100), Event NVARCHAR(100), TSQL NVARCHAR(2000));  
GO  
CREATE TRIGGER log   
ON DATABASE   
FOR DDL_DATABASE_LEVEL_EVENTS   
AS  
DECLARE @data XML  
SET @data = EVENTDATA()  
INSERT ddl_log   
   (PostTime, DB_User, Event, TSQL)   
   VALUES   
   (GETDATE(),   
   CONVERT(NVARCHAR(100), CURRENT_USER),   
   @data.value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),   
   @data.value('(/EVENT_INSTANCE/TSQLCommand)[1]', 'NVARCHAR(2000)') ) ;  
GO  
--Test the trigger.  
CREATE TABLE TestTable (a INT);  
DROP TABLE TestTable ;  
GO  
SELECT * FROM ddl_log ;  
GO  
--Drop the trigger.  
DROP TRIGGER log  
ON DATABASE;  
GO  
--Drop table ddl_log.  
DROP TABLE ddl_log;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 EVENTDATA 函数](../../relational-databases/triggers/use-the-eventdata-function.md)   
 [DDL 触发器](../../relational-databases/triggers/ddl-triggers.md)   
 [事件通知](../../relational-databases/service-broker/event-notifications.md)   
 [登录触发器](../../relational-databases/triggers/logon-triggers.md)  
  
  
