---
description: MSSQLSERVER_2516
title: MSSQLSERVER_2516 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2516 (Database Engine error)
ms.assetid: 79ed14b5-f53c-40c6-8334-8a083f732207
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d61eb6974d446f15b0658cef56a8f2d09922f67f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208451"
---
# <a name="mssqlserver_2516"></a>MSSQLSERVER_2516
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|2516|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC_REPAIR_DIFF_MAP_INVALIDATED|  
|消息正文|修复操作已经使数据库 NAME 的差异位图无效。 差异备份链断开。 必须首先执行完全数据库备份，才能执行差异备份。|  
  
## <a name="explanation"></a>说明  
在修复错误 MSSQLEngine_2515 时返回了此信息性消息。  
  
## <a name="user-action"></a>用户操作  
执行一次完整数据库备份。  
  
## <a name="see-also"></a>另请参阅  
[创建完整数据库备份 (SQL Server)](~/relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
