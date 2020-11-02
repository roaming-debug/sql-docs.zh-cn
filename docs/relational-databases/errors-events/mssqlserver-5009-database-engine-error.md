---
description: MSSQLSERVER_5009
title: MSSQLSERVER_5009
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 5009 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 1ca7fb52969d9ec08d8c80c48ec1325277a13fa7
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418695"
---
# <a name="mssqlserver_5009"></a>MSSQLSERVER_5009
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|5009|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|ALT_BADDISKS|
|消息正文|找不到或无法初始化语句中列出的一个或多个文件|
||

## <a name="explanation"></a>说明

此错误表示在 ALTER DATABASE 或 DBCC SHRINK* 命令中指定了无法解析的文件名或文件 ID。

请考虑以下情形：

- 你有一个使用完整或大容量日志恢复模式的 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。
- 你将一个名为 db_file1 的新数据文件添加到该数据库中。
- 你将 `db_file1` 文件的文件类型设置为数据。
- 你意识到你错误地指定了文件类型。
- 你删除 `db_file1` 文件，然后备份该数据库的事务日志。
- 你将一个名为 db_file1 的新日志文件添加到该数据库中。
- 你尝试使用 ALTER DATABASE 语句或使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 删除名为 db_file1 的日志文件。

在此情况下，你会收到如下所示的错误消息：

> 消息 5009，级别 16，状态 9，行 1 - 找不到或无法初始化语句中列出的一个或多个文件。

## <a name="possible-causes"></a>可能的原因

如果尝试删除的文件的逻辑名称在系统目录表中不是唯一的，则会发生此问题。 例如，如果删除了之前存在于数据库中的文件，则会发生此问题。

尝试删除具有相同逻辑名称的文件时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会尝试删除已删除的逻辑文件。 这会导致错误消息。

## <a name="user-action"></a>用户操作

若要解决此问题，请按照以下步骤操作。

> [!NOTE]
> 这些步骤会导致文件 ID 值被重用。

1. 使用 ALTER DATABASE 语句新建一个名称不同但数据类型相同的逻辑文件。 例如，将逻辑文件命名为 `different_remove_file_name` 而不是 `db_file1`，如以下示例所示：

    ```sql
    ALTER DATABASE [DBNAME] ADD FILE ( NAME = N'different_remove_file_name',
    FILENAME = N'D:\MSSQL.1\MSSQL\DATA\db_file1.ndf', SIZE = 1MB, MAXSIZE = 1MB)
    ```

    > [!NOTE]
    > 可使用任何文件名或任何文件路径。

1. 使用 ALTER DATABASE 语句删除在步骤 1 中创建的逻辑文件，如以下示例所示：

    ```sql
    ALTER DATABASE [DBNAME] REMOVE FILE [different_remove_file_name]
    ```

1. 创建数据库的事务日志备份。
1. 尝试再次删除名为 db_file1 的逻辑文件。
