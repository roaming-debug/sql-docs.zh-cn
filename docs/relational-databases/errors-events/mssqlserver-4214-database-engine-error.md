---
description: MSSQLSERVER_
title: MSSQLSERVER_4214
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 4214 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 0a04ec00e94dca9a4d940a3b0182123f0716d472
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418697"
---
# <a name="mssqlserver_4214"></a>MSSQLSERVER_4214
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|4214|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|DMPX_NODBBACKUP|
|消息正文|无法执行 BACKUP LOG，因为当前没有数据库备份|
||

## <a name="explanation"></a>说明

在执行完整的数据库备份之前尝试事务日志备份时，会引发此错误。 在尝试备份数据库的事务日志之前，必须执行完整的数据库备份。 此外，你会在错误日志中收到以下消息：

> \<Datetime> 备份    错误：3041，严重性：16，状态：1。  
\<Datetime>  备份     备份未能完成命令 BACKUP LOG \<db_name>。 有关详细消息，请查看备份应用程序日志。

## <a name="user-action"></a>用户操作

在尝试备份数据库的事务日志之前，请执行完整的数据库备份。

## <a name="more-information"></a>详细信息

有关详细信息，请参阅：[备份和还原 SQL Server 数据库](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)。