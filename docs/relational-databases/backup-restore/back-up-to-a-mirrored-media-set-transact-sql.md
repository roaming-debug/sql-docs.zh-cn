---
title: 备份到镜像媒体集 (Transact-SQL) | Microsoft Docs
description: 本文介绍在备份 SQL Server 数据库时如何使用 Transact-SQL BACKUP 语句指定镜像媒体集。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 5fc43a5d-dfd6-4c53-a4ef-3c8da23ccc81
author: cawrites
ms.author: chadam
ms.openlocfilehash: ee85e21230a8a178bcc916b05ad8de7811a826bd
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129373"
---
# <a name="back-up-to-a-mirrored-media-set-transact-sql"></a>备份镜像媒体集 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍在备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库时如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句指定镜像媒体集。 在 BACKUP 语句中的 TO 子句中指定第一个镜像。 然后，在它自己的 MIRROR TO 子句中指定每个镜像。 TO 和 MIRROR TO 子句必须指定相同数量和类型的备份设备。  
  
## <a name="example"></a>示例  
 下例创建上一图中所述的镜像介质集并将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库同时备份到两个镜像。  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'  
WITH  
    FORMAT,  
    MEDIANAME = 'AdventureWorks2012Set1';  
GO  
```  
  
## <a name="related-tasks"></a>Related Tasks  
 **从镜像备份还原**  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [镜像备份媒体集 (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)  
  
  
