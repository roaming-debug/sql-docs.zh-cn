---
description: 使用 EVENTDATA 函数
title: 使用 EVENTDATA 函数 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- EVENTDATA function
- DDL triggers, EVENTDATA function
ms.assetid: 675b8320-9c73-4526-bd2f-91ba42c1b604
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5b12fb3c0f29d4f9ef4137c277760593225b5e2e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97426401"
---
# <a name="use-the-eventdata-function"></a>使用 EVENTDATA 函数
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  使用 EVENTDATA 函数，可以捕获有关激发 DDL 触发器的事件的信息。 此函数返回 **xml** 值。 XML 架构包括下列信息：  
  
-   事件时间。  
  
-   在执行触发器时，连接的系统进程 ID (SPID)。  
  
-   激发触发器的事件类型。  
  
 根据事件类型，该架构还包括其他信息，例如事件在其中发生的数据库、发生事件的相关对象以及事件的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 有关详细信息，请参阅 [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md)。  
  
 例如，将在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库中创建以下 DDL 触发器：  
  
```  
CREATE TRIGGER safety   
ON DATABASE   
FOR CREATE_TABLE   
AS   
    PRINT 'CREATE TABLE Issued.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
   RAISERROR ('New tables cannot be created in this database.', 16, 1)   
   ROLLBACK  
;  
```  
  
 然后运行以下 `CREATE TABLE` 语句：  
  
 `CREATE TABLE NewTable (Column1 int);`  
  
 DDL 触发器中的 `EVENTDATA()` 语句将捕获不允许使用的 `CREATE TABLE` 语句文本。 通过对 EVENTDATA 生成的 xml 数据使用 XQuery 语句以及检索 \<CommandText> 元素来实现此操作。 有关详细信息，请参阅 [XQuery 语言参考 (SQL Server)](../../xquery/xquery-language-reference-sql-server.md)。  
  
> [!CAUTION]  
>  EVENTDATA 将捕获 CREATE_SCHEMA 事件的数据以及相应 CREATE SCHEMA 定义的 <schema_element>（如果存在）。 此外，EVENTDATA 将 <schema_element> 定义识别为单独的事件。 因此，针对 CREATE_SCHEMA 事件和由 CREATE_SCHEMA 定义的 <schema_element> 表示的事件创建的 DDL 触发器可能两次返回相同的事件数据，如 `TSQLCommand` 数据。 例如，针对 CREATE_SCHEMA 事件和 CREATE_TABLE 事件创建的 DDL 触发器，将运行下列批处理：  
>   
>  `CREATE SCHEMA s`  
>   
>  `CREATE TABLE t1 (col1 int)`  
>   
>  如果应用程序检索 CREATE_TABLE 事件的 `TSQLCommand` 数据，则注意，此数据可能出现两次：一次是在 CREATE_SCHEMA 事件发生时，另一次是在 CREATE_TABLE 事件发生时。 应避免同时针对 CREATE_SCHEMA 事件和任何相应 CREATE SCHEMA 定义的 <schema_element> 文本创建 DDL 触发器，或者将逻辑置于应用程序中，这样，就不会将同一事件处理两次了。  
  
## <a name="alter-table-and-alter-database-events"></a>ALTER TABLE 和 ALTER DATABASE 事件  
 ALTER_TABLE 和 ALTER_DATABASE 事件的事件数据还包括以下内容：受 DDL 语句影响的其他对象的名称和类型以及对这些对象执行的操作。 ALTER_TABLE 事件数据包括以下内容：受 ALTER TABLE 语句影响的列、约束或触发器的名称以及对受影响的对象执行的操作（创建、更改、删除、启用或禁用）。 ALTER_DATABASE 事件数据包括以下内容：受 ALTER DATABASE 语句影响的任何文件或文件组的名称以及对受影响的对象执行的操作（创建、更改或删除）。  
  
 例如，在 AdventureWorks 示例数据库中创建以下 DDL 触发器：  
  
```  
CREATE TRIGGER ColumnChanges  
ON DATABASE   
FOR ALTER_TABLE  
AS  
-- Detect whether a column was created/altered/dropped.  
SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]', 'nvarchar(max)')  
RAISERROR ('Table schema cannot be modified in this database.', 16, 1);  
ROLLBACK;  
```  
  
 然后，执行以下违反约束的 ALTER TABLE 语句：  
  
```  
ALTER TABLE Person.Address ALTER COLUMN ModifiedDate date;   
```  
  
 DDL 触发器中的 EVENTDATA() 语句将捕获不允许使用的 `ALTER TABLE` 语句的文本。  
  
## <a name="example"></a>示例  
 您可以使用 EVENTDATA 函数来创建事件日志。 在下面的示例中，创建了一个表来存储事件信息。 然后，针对当任何数据库级 DDL 事件发生时，使用以下信息填充表的当前数据库创建 DDL 触发器：  
  
-   事件时间（使用 GETDATE 函数）。  
  
-   其会话上发生事件的数据库用户（使用 CURRENT_USER 函数）。  
  
-   事件类型。  
  
-   包括事件的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
 此外，通过对 EVENTDATA 生成的 **xml** 数据使用 XQuery 来捕获最后两项。  
  
```  
USE AdventureWorks2012;  
GO  
CREATE TABLE ddl_log (PostTime datetime, DB_User nvarchar(100), Event nvarchar(100), TSQL nvarchar(2000));  
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
   CONVERT(nvarchar(100), CURRENT_USER),   
   @data.value('(/EVENT_INSTANCE/EventType)[1]', 'nvarchar(100)'),   
   @data.value('(/EVENT_INSTANCE/TSQLCommand)[1]', 'nvarchar(2000)') ) ;  
GO  
--Test the trigger  
CREATE TABLE TestTable (a int)  
DROP TABLE TestTable ;  
GO  
SELECT * FROM ddl_log ;  
GO  
```  
  
> [!NOTE]  
>  如果需要返回事件数据，我们建议使用 XQuery **value()** 方法而不是 **query()** 方法。 **query()** 方法可在输出中返回 XML 和以“and”符转义的回车符和换行符 (CRLF) 实例，而 **value()** 方法无法在输出中呈现 CRLF 实例。  
  
 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库还提供了类似的 DDL 触发器示例。 若要获得示例，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]找到 Database Triggers 文件夹。 此文件夹位于 **数据库的** Programmability [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 文件夹下。 右键单击“ddlDatabaseTriggerLog”并选择“将数据库触发器脚本编写为”。 默认情况下，DDL 触发器 ddlDatabaseTriggerLog 处于禁用状态。  
  
## <a name="see-also"></a>另请参阅  
 [DDL 事件](../../relational-databases/triggers/ddl-events.md)   
 [DDL 事件组](../../relational-databases/triggers/ddl-event-groups.md)  
  
  
