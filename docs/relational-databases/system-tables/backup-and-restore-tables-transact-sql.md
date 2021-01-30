---
description: 备份和还原表 (Transact-SQL)
title: 备份和还原表 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- system tables [SQL Server], backup tables
- backup system tables [SQL Server]
- system tables [SQL Server], restore tables
- restore system tables [SQL Server]
ms.assetid: aa615add-54e6-40f5-8b55-3728b26884ee
author: cawrites
ms.author: chadam
ms.openlocfilehash: fe70ff306185ed57f0881cddd0a48dd19636dae0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180405"
---
# <a name="backup-and-restore-tables-transact-sql"></a>备份和还原表 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节中的主题介绍可存储数据库备份和还原操作所用信息的系统表。  
  
## <a name="in-this-section"></a>本节内容  
 [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
 数据库的每个数据文件或日志文件在表中占一行。  
  
 [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
 备份时数据库中的每个文件组在表中占一行。  
  
 [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
 每个介质簇在表中占一行。  
  
 [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
 每个备份介质集在表中占一行。  
  
 [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
 每个备份集在表中占一行。  
  
 [logmarkhistory](../../relational-databases/system-tables/logmarkhistory-transact-sql.md)  
 每个已提交的标记事务在表中占一行。  
  
 [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
 每个已还原的文件在表中占一行。 其中包括按文件组名称间接还原的文件。  
  
 [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
 每个已还原的文件组在表中占一行。  
  
 [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
 每个还原操作在表中占一行。  
  
 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md)  
 由于发生 824 错误而失败的每个页面在表中占一行（限制为 1,000 行）。  
  
 [sysopentapes](../../relational-databases/system-tables/sysopentapes-transact-sql.md)  
 当前打开的每个磁带设备在表中占一行。  
  
  
