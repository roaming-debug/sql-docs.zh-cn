---
description: DDL 触发器
title: DDL 触发器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- DDL triggers, about DDL triggers
ms.assetid: 1a4a6564-9820-4a14-9305-2c0e9ea37454
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2c06441c50e4fe85f17dc556074beb461d8c2e51
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403176"
---
# <a name="ddl-triggers"></a>DDL 触发器
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  DDL 触发器将激发，以响应各种数据定义语言 (DDL) 事件。 这些事件主要与以关键字 CREATE、ALTER、DROP、GRANT、DENY、REVOKE 或 UPDATE STATISTICS 开头的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句对应。 执行 DDL 式操作的系统存储过程也可以激发 DDL 触发器。  
  
 如果要执行以下操作，请使用 DDL 触发器：  
  
-   防止对数据库架构进行某些更改。  
  
-   希望数据库中发生某种情况以响应数据库架构的更改。  
  
-   记录数据库架构的更改或事件。  
  
> [!IMPORTANT]  
>  测试您的 DDL 触发器以确定它们是否响应运行的系统存储过程。 例如，CREATE TYPE 语句和 **sp_addtype** 存储过程都将激发针对 CREATE_TYPE 事件创建的 DDL 触发器。  
  
## <a name="types-of-ddl-triggers"></a>DDL 触发器的类型  
 ### <a name="transact-sql-ddl-trigger"></a>Transact-SQL DDL 触发器  
 用于执行一个或多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句以响应服务器范围或数据库范围事件的一种特殊类型的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程。 例如，如果执行某个语句（如 ALTER SERVER CONFIGURATION）或者使用 DROP TABLE 删除某个表，则激发 DDL 触发器。  
  
 ### <a name="clr-ddl-trigger"></a>CLR DDL 触发器  
 CLR 触发器将执行在托管代码（在 .NET Framework 中创建并在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中上载的程序集的成员）中编写的方法，而不用执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]存储过程。  
  
 仅在运行触发 DDL 触发器的 DDL 语句后，DDL 触发器才会激发。 DDL 触发器无法作为 INSTEAD OF 触发器使用。 对于影响局部或全局临时表和存储过程的事件，不会触发 DDL 触发器。  
  
 DDL 触发器不会创建特殊的 **inserted** 和 **deleted** 表。  
  
 可以使用 EVENTDATA 函数捕获有关激发 DDL 触发器的事件以及触发器导致的后续更改的信息。  
  
 为每个 DDL 事件创建多个触发器。  
  
 与 DML 触发器不同，DDL 触发器的作用域不是架构。 因此，不能将 OBJECT_ID、OBJECT_NAME、OBJECTPROPERTY 和 OBJECTPROPERTYEX 之类的函数用于查询有关 DDL 触发器的元数据。 请改用目录视图。  
  
 服务器范围的 DDL 触发器显示在 SQL Server Management Studio 对象资源管理器的“触发器”文件夹中。 此文件夹位于 **“服务器对象”** 文件夹下。 数据库范围的 DDL 触发器显示在“数据库触发器”文件夹中。 此文件夹位于相应数据库的 **“可编程性”** 文件夹下。  
  
> [!IMPORTANT]  
>  触发器内部的恶意代码可以在升级后的权限下运行。 有关如何帮助减少此威胁的详细信息，请参阅 [管理触发器安全](../../relational-databases/triggers/manage-trigger-security.md)。  
  
## <a name="ddl-trigger-scope"></a>DDL 触发器作用域  
 在响应当前数据库或服务器上处理的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 事件时，可以触发 DDL 触发器。 触发器的作用域取决于事件。 例如，每当数据库中或服务器实例上发生 CREATE_TABLE 事件时，都会激发为响应 CREATE_TABLE 事件创建的 DDL 触发器。 仅当服务器实例上发生 CREATE_LOGIN 事件时，才能激发为响应 CREATE_LOGIN 事件创建的 DDL 触发器。  
  
 在下面的示例中，每当数据库中发生 `safety` 或 `DROP_TABLE` 事件时，都会激发 DDL 触发器 `ALTER_TABLE` 。  
  
```  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
```  
  
 在下面的示例中，如果当前服务器实例上发生任何 `CREATE_DATABASE` 事件，DDL 触发器将输出消息。 此示例使用 `EVENTDATA` 函数检索相应的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的文本。 有关如何将 EVENTDATA 与 DDL 触发器配合使用的详细信息，请参阅 [使用 EVENTDATA 函数](../../relational-databases/triggers/use-the-eventdata-function.md)。  
  
```  
IF EXISTS (SELECT * FROM sys.server_triggers  
    WHERE name = 'ddl_trig_database')  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
CREATE TRIGGER ddl_trig_database   
ON ALL SERVER   
FOR CREATE_DATABASE   
AS   
    PRINT 'Database Created.'  
    SELECT EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
GO  
DROP TRIGGER ddl_trig_database  
ON ALL SERVER;  
GO  
  
```  
  
 本主题后面的“选择触发 DDL 触发器的特定 DDL 语句”一部分中提供了一些链接，通过这些链接可以找到将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句映射到为它们指定的作用域的列表。  
  
 数据库范围内的 DDL 触发器都作为对象存储在创建它们的数据库中。 可以在 **master** 数据库中创建 DDL 触发器，这些触发器的行为与在用户设计的数据库中创建的 DDL 触发器的行为类似。 可以通过查询 **sys.triggers** 目录视图获取有关 DDL 触发器的信息。 可以在创建触发器的数据库上下文中或通过指定数据库名称作为标识符（例如 **master.sys.triggers** ），查询 **sys.triggers**。  
  
 服务器范围内的 DDL 触发器作为对象存储在 **master** 数据库中。 然而，可以通过在任何数据库上下文中查询 **sys.server_triggers** 目录视图，获取有关服务器范围内的 DDL 触发器的信息。  
  
## <a name="specifying-a-transact-sql-statement-or-group-of-statements"></a>指定 Transact-SQL 语句或语句组  
  
### <a name="selecting-a-particular-ddl-statement-to-fire-a-ddl-trigger"></a>选择触发 DDL 触发器的特定 DDL 语句  
 可以安排在运行一个或多个特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句后触发 DDL 触发器。 在前面的示例中，在发生任何 `safety` 或 `DROP_TABLE` 事件后触发 `ALTER_TABLE` 触发器。 有关可指定激发 DDL 触发器的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的列表，请参阅 [DDL 事件](../../relational-databases/triggers/ddl-events.md)。  
  
### <a name="selecting-a-predefined-group-of-ddl-statements-to-fire-a-ddl-trigger"></a>选择触发 DDL 触发器的一组预定义的 DDL 语句  
 可以在执行属于一组预定义的相似事件的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 事件后激发 DDL 触发器。 例如，如果希望在运行 CREATE TABLE、ALTER TABLE 或 DROP TABLE 语句后触发 DDL 触发器，则可以在 CREATE TRIGGER 语句中指定 FOR DDL_TABLE_EVENTS。 运行 CREATE TRIGGER 后，事件组涵盖的事件都添加到 **sys.trigger_events** 目录视图中。  
  
 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中，如果对事件组创建触发器，则 **sys.trigger_events** 不包括有关该事件组的信息，而 **sys.trigger_events** 仅包括有关该组涵盖的个别事件的信息。 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中， **sys.trigger_events** 保留有关创建触发器的事件组的元数据，以及有关该事件组涵盖的个别事件的元数据。 因此，对 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中事件组所涵盖的事件的更改并不适用于在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中对这些事件组创建的 DDL 触发器。  
  
 有关可用于 DDL 触发器的预定义 DDL 语句组、事件组所涵盖的特定语句以及可以对这些事件组进行编程的作用域的列表，请参阅 [DDL Event Groups](../../relational-databases/triggers/ddl-event-groups.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务|主题|  
|----------|-----------|  
|说明如何创建、修改、删除或禁用 DDL 触发器。|[实现 DDL 触发器](../../relational-databases/triggers/implement-ddl-triggers.md)|  
|说明如何创建 CLR DDL 触发器。|[创建 CLR 触发器](../../relational-databases/triggers/create-clr-triggers.md)|  
|说明如何返回有关 DDL 触发器的信息。|[获取有关 DDL 触发器的信息](../../relational-databases/triggers/get-information-about-ddl-triggers.md)|  
|说明如何返回有关使用 EVENTDATA 函数激发 DDL 触发器的事件的信息。|[使用 EVENTDATA 函数](../../relational-databases/triggers/use-the-eventdata-function.md)|  
|说明如何管理触发器安全性。|[管理触发器安全性](../../relational-databases/triggers/manage-trigger-security.md)|  
  
## <a name="see-also"></a>另请参阅  
 [DML 触发器](../../relational-databases/triggers/dml-triggers.md)   
 [登录触发器](../../relational-databases/triggers/logon-triggers.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
 [事件数据 &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
