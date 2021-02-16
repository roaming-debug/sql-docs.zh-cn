---
description: MSSQLSERVER_3859
title: MSSQLSERVER_3859
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3859 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 7fff2944c0fdb3fc6d80326841b9950d3591f12e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348802"
---
# <a name="mssqlserver_3859"></a>MSSQLSERVER_3859
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|3859|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|DBCC_CHECKCAT_DIRECT_UPDATE|
|消息正文|警告：数据库 ID \%d 中的系统目录已直接更新，最近的更新时间为 %S_DATE|
||

## <a name="explanation"></a>说明

此错误表示用户发起了对系统表的更改。 不支持手动更新系统表。 系统表只能由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎进行更新。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 检测到用户发起了对系统表的更改，则将在出现下列两种情况时引发错误 3859：

- 方案 1

    启动包含已手动更新的系统表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库时，如下所示的事件会记录在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志或事件查看器中的应用程序日志中：

    > 日志名称：应用程序  
    来源：MSSQLSERVER 事件 ID：3859  
    任务类别：服务器  
    级别：信息  
    说明：警告：数据库 ID \%d 中的系统目录已直接更新，最近的更新时间为 date_time  

- 方案 2  

    手动更新系统表后，如果执行 `DBCC_CHECKDB` 命令，将返回以下警告消息：

    > “database_name”的 DBCC 结果。  
    消息 8992，级别 16，状态 1，行 1  
    请检查目录消息 3859，状态 1：警告：数据库 ID \%d 中的系统目录已直接更新，最近的更新时间为 date_time。  
    CHECKDB 在数据库“db_name”中未发现任何分配错误和一致性错误。  
    DBCC 执行完毕。 如果 DBCC 输出了错误消息，请与系统管理员联系。

## <a name="user-action"></a>用户操作

若要解决此问题，请使用下列方法之一。

- 方法 1

    如果具有数据库的干净备份，请从该备份还原数据库。  
    > [!NOTE]
    > 仅当备份在元数据中保持一致时，此方法才有效。  

- 方法 2  

    如果无法从备份还原数据库，请将数据和对象导出到新的数据库中。 然后，将手动更新的数据库的内容传输到新数据库中。 请注意，不能使用 DBCC CHECKDB 命令中的 REPAIR 选项来修复系统目录中的不一致问题。 结果就是，该命令无法修复元数据损坏，因此它不提供任何建议的修复级别。

    > [!NOTE]
    > 可通过系统目录视图查看系统表中的数据。

## <a name="more-information"></a>详细信息

有关详细信息，请参阅：[系统基表](../system-tables/system-base-tables.md)。