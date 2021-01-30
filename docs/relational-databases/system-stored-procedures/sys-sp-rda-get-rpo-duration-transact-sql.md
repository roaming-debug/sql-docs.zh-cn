---
title: sys.sp_rda_get_rpo_duration (Transact-sql) |Microsoft Docs
description: 使用 sys.sp_rda_get_rpo_duration 获取已迁移数据的小时数，SQL Server 保留在临时表中以完全还原远程 Azure 数据库。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_get_rpo_duration
- sys.sp_rda_get_rpo_duration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_get_rpo_duration stored procedure
ms.assetid: 35882067-3072-47ff-9024-ca453c0f49a7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5c84b580f2a51ad9507db2fbece327a3ca71608
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211795"
---
# <a name="syssp_rda_get_rpo_duration-transact-sql"></a>sys.sp_rda_get_rpo_duration (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  获取 SQL Server 保留在临时表中的已迁移数据的小时数，以帮助确保远程 Azure 数据库完整还原（如果需要进行时间点还原）。 
  
  有关详细信息，请参阅 [Stretch Database 通过暂时保留已迁移行来降低 Azure 数据的数据丢失风险](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md#stretchRPO)。  
    
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>语法    
    
```    
    
sp_rda_get_rpo_duration @durationinhours output    
    
```    
    
## <a name="output-parameter"></a>输出参数    
 *\@durationinhours*    
  SQL Server 为当前已启用延伸的数据库保留的已迁移数据)  (非 null 整数值的小时数。    
    
## <a name="permissions"></a>权限    
 需要 db_owner 权限。    
    
## <a name="remarks"></a>备注    
 通过运行 [&#40;transact-sql&#41;sys.sp_rda_set_rpo_duration ](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md)来更改值。    
    
## <a name="see-also"></a>另请参阅    
 [sys.sp_rda_set_rpo_duration &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md)     
 [还原已启用延伸的数据库 (Stretch Database) ](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)    
 [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)    
    
  
