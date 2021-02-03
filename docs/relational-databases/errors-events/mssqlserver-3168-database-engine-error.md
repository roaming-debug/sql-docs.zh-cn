---
description: MSSQLSERVER_3168
title: MSSQLSERVER_3168 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3168 (Database Engine error)
ms.assetid: 991111d9-1eb3-43e9-9333-a75a775c3200
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9e57fa84cf49932c0a550290c11f267d6a055921
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194209"
---
# <a name="mssqlserver_3168"></a>MSSQLSERVER_3168
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|3168|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LDDB_SYSTEMWRONGVER|  
|消息正文|无法还原设备 %ls 上的系统数据库备份，因为创建该数据库的服务器版本 (%ls) 与此服务器 (%ls) 的版本不同。|  
  
## <a name="explanation"></a>说明  
不能在与原来执行备份的服务器内部版本不同的内部版本上还原系统数据库（**master**、**model** 或 **msdb**）的备份。  
  
> [!NOTE]  
> 安装 Service Pack 或修补程序会更改服务器内部版本号，并且服务器内部版本始终是递增的。  
  
### <a name="possible-causes"></a>可能的原因  
系统数据库的数据库架构在各服务器内部版本之间可能会发生更改。 为确保架构更改不会产生不一致，RESTORE 语句会将备份文件的服务器内部版本号与尝试在其上还原备份的服务器内部版本号进行比较。 如果内部版本不相同，则该语句会发出 3168 错误消息，从而导致还原操作异常终止。  
  
可能发生此错误的情况包括：  
  
-   用户试图在服务器 A 上使用在服务器 B 上执行的备份来还原系统数据库。服务器 A 和 B 的服务器内部版本不同。 例如，服务器 A 可能是原始发布版本的内部版本，而服务器 B 可能是 Service Pack 1 (SP1) 内部版本。  
  
-   用户试图从在同一服务器上执行的备份还原系统数据库。 但是，执行备份时服务器运行的是不同的内部版本。 即执行备份后对服务器进行了升级。  
  
## <a name="user-action"></a>用户操作  
在此情况下的还原过程相当复杂，应在不得已的情况下才使用。 有关详细信息，请参阅“[无法将系统数据库备份还原到不同内部版本的 SQL Server](https://support.microsoft.com/kb/264474)”。  
  
## <a name="see-also"></a>另请参阅  
[对还原系统数据库的限制 &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md#limitations-on-restoring-system-databases)  
  
