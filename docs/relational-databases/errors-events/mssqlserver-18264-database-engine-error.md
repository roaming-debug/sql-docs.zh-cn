---
description: MSSQLSERVER_18264
title: MSSQLSERVER_18264 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18264 (Database Engine error)
ms.assetid: 3050fc56-2be5-43cf-916b-50a3ac5f89aa
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2a029255c0ae2d47f4f1033d9cf59e6f669383c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196541"
---
# <a name="mssqlserver_18264"></a>MSSQLSERVER_18264
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|Microsoft SQL Server|  
|事件 ID|18264|  
|事件源|MSSQLENGINE|  
|组件|SQLEngine|  
|符号名称|STRMIO_DBDUMP|  
|消息正文|数据库已备份。 数据库: %s，创建日期(时间): %s(%s)，转储的页数: %d，第一个 LSN: %s，最后一个 LSN: %s，转储设备数: %d，设备信息: (%s)。 这只是一条信息性消息。 不需要任何用户操作。|  
  
## <a name="explanation"></a>说明  
默认情况下，每个成功的备份操作都会将该信息性消息添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志和系统事件日志中。 如果非常频繁地备份事务日志，这些消息会迅速累积，从而产生一个巨大的错误日志，这样会使查找其他消息变得非常困难。  
  
## <a name="user-action"></a>用户操作  
可以通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 跟踪标志 **3226** 来禁止显示这些日志条目。 如果您频繁地运行日志备份，而且没有任何脚本依赖于这些条目，则启用该跟踪标志非常有用。  
  
有关使用跟踪标志的信息，请参阅 SQL Server 联机丛书。  
  
## <a name="see-also"></a>另请参阅  
[跟踪标志 (Transact-SQL)](~/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
  
