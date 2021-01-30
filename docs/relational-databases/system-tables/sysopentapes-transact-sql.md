---
description: sysopentapes (Transact-SQL)
title: sysopentapes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysopentapes
- sysopentapes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], sysopentapes system table
- sysopentapes system table
ms.assetid: c066ca9b-9cfd-46b1-90a3-5c8dc9e7b6ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 133bbf06a4c25ba050287e83dcfd9005594483f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195401"
---
# <a name="sysopentapes-transact-sql"></a>sysopentapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  当前打开的每个磁带设备在表中占一行。 此视图存储在 **master** 数据库中。  
  
> [!IMPORTANT]  
>  包含此系统表作为一个视图是为了保持向后兼容性。 请改用 [&#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) 动态管理视图中的 sys.dm_io_backup_tapes。  
  
> [!NOTE]  
>  不能删除 **sysopentapes** 视图。  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**openTape**|**nvarchar (64)**|打开的磁带设备的物理文件名。 有关打开和释放磁带设备的详细信息，请参阅 [BACKUP &#40;transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md) 和 [RESTORE &#40;transact-sql&#41;](../../t-sql/statements/restore-statements-transact-sql.md)。|  
  
## <a name="permissions"></a>权限  
 用户需要对该服务器具有 VIEW SERVER STATE 权限。  
  
  
