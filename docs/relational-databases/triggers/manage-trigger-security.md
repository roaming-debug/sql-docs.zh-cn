---
description: 管理触发器安全性
title: 管理触发器安全性 | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], security
ms.assetid: e94720a8-a3a2-4364-b0a3-bbe86e3ce4d5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 931ba1487b8212c6c088b044df5e0b9fa30b5ef1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347596"
---
# <a name="manage-trigger-security"></a>管理触发器安全性

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

默认情况下，在调用触发器的用户的上下文中执行 DML 和 DDL 触发器。 触发器的调用方是执行使触发器运行的语句的用户。 例如，如果用户 **Mary** 执行可以使 DML 触发器 **DML_trigMary** 运行的 DELETE 语句，则 **DML_trigMary** 中的代码将在 **Mary** 的用户特权上下文中执行。 希望向数据库或服务器实例中引入恶意代码的用户可以使用此默认行为。 例如，用户 JohnDoe 创建以下 DDL 触发器：  

```sql
CREATE TRIGGER DDL_trigJohnDoe
ON DATABASE
FOR ALTER_TABLE
AS
SET NOCOUNT ON;

BEGIN TRY
  EXEC(N'
    USE [master];
    GRANT CONTROL SERVER TO [JohnDoe];
');
END TRY
BEGIN CATCH
  DECLARE @DoNothing INT;
END CATCH;
GO
```

此触发器的含义是：在有权执行 `GRANT CONTROL SERVER` 语句的用户（如 sysadmin 固定服务器角色的成员）执行 `ALTER TABLE` 语句时，为 JohnDoe 授予 `CONTROL SERVER` 权限。 换言之，虽然 JohnDoe 不能向自己授予 `CONTROL SERVER` 权限，但他启用了授予自己此权限的触发器代码以在提升特权下执行。 对于此类安全隐患，DML 和 DDL 触发器都处于打开状态。  
  
## <a name="trigger-security-best-practices"></a>保证触发器安全的最佳方法  
 可以采取下列措施阻止触发器代码在升级特权下执行：  
  
::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"

-   注意数据库和服务器实例中存在的 DML 和 DDL 触发器，方法是查询 [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) 和 [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md) 目录视图。 下面的查询将返回当前数据库中的所有 DML 触发器和数据库级别的 DDL 触发器，以及服务器实例中所有服务器级别的 DDL 触发器：  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers
    UNION ALL
    SELECT type, name, parent_class_desc FROM sys.server_triggers;
    ```  

   > [!NOTE]
   > 只有 sys.databases 可用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]，除非使用的是 [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]。

::: moniker-end

::: moniker range="=azuresqldb-current"

-   请注意数据库中是否存在 DML 和 DDL 触发器，具体方法是查询 [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) 目录视图。 下面的查询返回当前数据库中的所有 DML 和数据库级别 DDL 触发器：  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers;
    ```  
  
::: moniker-end

-   使用 [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) 禁用在升级特权下执行时可能会损害数据库或服务器完整性的触发器。 下面的语句可以禁用当前数据库中所有数据库级别的 DDL 触发器：  
  
    ```sql
    DISABLE TRIGGER ALL ON DATABASE;
    ```  
  
     下面的语句可以禁用服务器实例中所有服务器级别的 DDL 触发器：  
  
    ```sql
    DISABLE TRIGGER ALL ON ALL SERVER;
    ```  
  
     下面的语句可以禁用当前数据库中的所有 DML 触发器：  
  
    ```sql
    DECLARE @schema_name sysname, @trigger_name sysname, @object_name sysname;
    DECLARE @sql nvarchar(max);
    DECLARE trig_cur CURSOR FORWARD_ONLY READ_ONLY FOR
        SELECT SCHEMA_NAME(schema_id) AS schema_name,
            name AS trigger_name,
            OBJECT_NAME(parent_object_id) AS object_name
        FROM sys.objects WHERE type IN ('TR', 'TA');

    OPEN trig_cur;
    FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
  
    WHILE @@FETCH_STATUS = 0
    BEGIN
        SELECT @sql = N'DISABLE TRIGGER ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@trigger_name)
            + N' ON ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@object_name) + N'; ';
        EXEC (@sql);
        FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
    END;
    GO

    -- Verify triggers are disabled. Should return an empty result set.
    SELECT * FROM sys.triggers WHERE is_disabled = 0;
    GO

    CLOSE trig_cur;
    DEALLOCATE trig_cur;
    ```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DML 触发器](../../relational-databases/triggers/dml-triggers.md)   
 [DDL 触发器](../../relational-databases/triggers/ddl-triggers.md)  
 [登录触发器](../../relational-databases/triggers/logon-triggers.md)  
  
