---
description: MSmerge_errorlineage (Transact-SQL)
title: MSmerge_errorlineage (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_errorlineage_TSQL
- MSmerge_errorlineage
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_errorlineage system table
ms.assetid: 3bcbd328-c958-4cd4-a573-3c35539fa919
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9dcf7f8db0a5d02a64657e00046667b07d1e29e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171682"
---
# <a name="msmerge_errorlineage-transact-sql"></a>MSmerge_errorlineage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSmerge_errorlineage** 表包含已在订阅服务器上删除的行，但其删除不会传播到发布服务器。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|分配给为合并复制发布的表的整数值。 对应于 **sysmergearticles** 表中的昵称字段。|  
|**rowguid**|**uniqueidentifier**|行标识符。|  
|**衍生**|**varbinary (501)**|存储订阅服务器和发布服务器对行进行更新的历史记录列表。 用于检测和解决冲突情况。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
