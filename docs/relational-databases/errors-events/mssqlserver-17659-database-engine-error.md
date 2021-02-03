---
description: MSSQLSERVER_17659
title: MSSQLSERVER_17659
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17659 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 5e7baf4f821a7fd36ca4202bc5299ae4cc99ae65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196677"
---
# <a name="mssqlserver_17659"></a>MSSQLSERVER_17659
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|17659|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|DEMO_SYSCATUPDATE|
|消息正文|数据库 ID \%d 中的系统表 ID \%d 已直接更新，但可能未维护缓存一致性。 <br/> 应重新启动 SQL Server。|
||

## <a name="explanation"></a>说明

此错误表示已直接更新系统对象。 不支持手动更新系统表。 系统表只能由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎进行更新。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 检测到用户发起了对系统表的更改时，将引发错误 17659。 在这种情况下，如下所示的事件会记录在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志或事件查看器中的应用程序日志中。

> 日志名称：应用程序  
来源：MSSQLServer  
事件 ID：17659  
任务类别：服务器  
级别：信息  
说明：警告：数据库 ID \%d 中的系统表 ID %d 已直接更新，但可能未维护缓存一致性。 应重新启动 SQL Server。

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