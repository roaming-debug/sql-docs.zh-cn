---
description: DROP DATABASE (Transact-SQL)
title: DROP DATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP DATABASE
- DROP_DATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], deleting
- removing databases
- database snapshots [SQL Server], removing
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- DROP DATABASE statement
- database removal [SQL Server], DROP DATABASE statement
ms.assetid: 477396a9-92dc-43c9-9b97-42c8728ede8e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25fb14a69ee138195ae163f9ee90a82b0e90e3dd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095783"
---
# <a name="drop-database-transact-sql"></a>DROP DATABASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除一个或多个用户数据库或数据库快照。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
-- SQL Server Syntax
DROP DATABASE [ IF EXISTS ] { database_name | database_snapshot_name } [ ,...n ] [;]
```

```syntaxsql
-- Azure SQL Database, Azure Synapse Analytics and Analytics Platform System Syntax
DROP DATABASE database_name [;]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

IF EXISTS
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到[当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)）。

只有在数据库已存在时才对其进行有条件地删除。

database_name 指定要删除的数据库的名称。 若要显示数据库列表，请使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图。

database_snapshot_name
适用于：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

指定要删除的数据库快照的名称。

## <a name="general-remarks"></a>一般备注

无论数据库处于下列哪种状态，都可将其删除：脱机状态、只读状态或可疑状态等。 若要显示数据库的当前状态，请使用 **sys.databases** 目录视图。

只有通过还原备份才能重新创建已删除的数据库。 数据库快照无法备份，因此也就无法还原。

删除数据库时，应备份 [master 数据库](../../relational-databases/databases/master-database.md)。

执行数据库删除操作会从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除数据库，并删除该数据库使用的物理磁盘文件。 执行删除操作时，如果数据库或它的任意一个文件处于脱机状态，则不会删除磁盘文件。 可使用 Windows 资源管理器手动删除这些文件。 若要从当前服务器删除数据库，但不从文件系统删除文件，请使用 [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)。

> [!WARNING]
> 删除具有与其关联的 FILE_SNAPSHOT 备份的数据库将成功，但不会删除具有关联快照的数据库文件，以免使引用这些数据库文件的备份失效。 将截断该文件，但不会以物理方式将其删除，以保持 FILE_SNAPSHOT 备份不受影响。 有关详细信息，请参阅[使用 Microsoft Azure Blob 存储服务执行 SQL Server 备份和还原](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)。 适用于：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)。

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

执行数据库快照删除操作会从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中删除数据库快照，并删除该快照使用的物理 NTFS 文件系统稀疏文件。 有关数据库快照如何使用稀疏文件的信息，请参阅[数据库快照](../../relational-databases/databases/database-snapshots-sql-server.md)。 删除数据库快照将清除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计划缓存。 清除计划缓存将导致对所有后续执行计划进行重新编译，并可能导致查询性能暂时性地突然降低。 对于计划缓存中每个已清除的缓存存储区，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志包含以下信息性消息：“由于某些数据库维护或重新配置操作，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 经历了 '%s' 缓存存储区(计划缓存的一部分)的 %d 次刷新”。 每隔五分钟，只要缓存在这段时间间隔内得到刷新，此消息就记录一次。

## <a name="interoperability"></a>互操作性

### <a name="sql-server"></a>SQL Server

若要删除针对事务复制发布的数据库，或者删除针对合并复制发布或订阅的数据库，必须首先从数据库中删除复制。 如果数据库损坏或无法首先删除复制，或这两种情况同时存在，那么在多数情况下仍然可以删除数据库，方法是使用 ALTER DATABASE 将数据库设置为脱机，然后将其删除。

如果数据库涉及日志传送操作，请在删除数据库之前取消日志传送操作。 有关详细信息，请参阅 [关于日志传送](../../database-engine/log-shipping/about-log-shipping-sql-server.md)。

## <a name="limitations-and-restrictions"></a>限制和局限

无法删除[系统数据库](../../relational-databases/databases/system-databases.md)。

DROP DATABASE 语句必须在自动提交模式下运行，并且不允许在显式或隐式事务中使用。 自动提交模式是默认的事务管理模式。

不能删除当前正在使用的数据库。 这表示数据库正处于打开状态，以供用户读写。 从数据库中删除用户的一种方式是使用 ALTER DATABASE 将数据库设置为 SINGLE_USER。

> [!WARNING]
> 这并不是失败的证明方法，因为任何线程的第一个连续连接都将收到 SINGLE_USER 线程，导致连接失败。 SQL Server 不提供删除低负荷数据库的内置方法。

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

在删除数据库之前，必须将该数据库上的所有数据库快照都删除。

删除为 Stretch Database 启用的数据库不会删除远程数据。 若要删除远程数据，必须手动删除。

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

您必须连接到 master 数据库来删除数据库。

 DROP DATABASE 语句必须是 SQL 批处理中的唯一语句，您一次只能删除一个数据库。

### [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

您必须连接到 master 数据库来删除数据库。

DROP DATABASE 语句必须是 SQL 批处理中的唯一语句，您一次只能删除一个数据库。

## <a name="permissions"></a>权限

### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

要求对数据库具有 CONTROL 权限或者 ALTER ANY DATABASE 权限，或者在 db_owner 固定数据库角色中具有成员身份。

### [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

只有服务器级别主体登录名（由预配过程创建）或 dbmanager 数据库角色的成员可以删除数据库。

### [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

要求对数据库具有 CONTROL 权限或者 ALTER ANY DATABASE 权限，或者在 db_owner 固定数据库角色中具有成员身份。

## <a name="examples"></a>示例

### <a name="a-dropping-a-single-database"></a>A. 删除单个数据库

以下示例删除 `Sales` 数据库。

```sql
DROP DATABASE Sales;
```

### <a name="b-dropping-multiple-databases"></a>B. 删除多个数据库

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

以下示例删除每个列出的数据库。

```sql
DROP DATABASE Sales, NewSales;
```

### <a name="c-dropping-a-database-snapshot"></a>C. 删除数据库快照

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

以下示例在不影响源数据库的情况下，删除名为 `sales_snapshot0600` 的数据库快照。

```sql
DROP DATABASE sales_snapshot0600;
```

## <a name="see-also"></a>另请参阅

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
