---
title: sys.sp_rda_reconcile_indexes (Transact-sql) |Microsoft Docs
description: 了解 sys.sp_rda_reconcile_indexes。 请参阅如何使用此 Transact-sql 存储过程将架构任务排队以便协调远程表上的索引。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sp_rda_reconcile_indexes
- sp_rda_reconcile_indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_indexes stored procedure
ms.assetid: 96b31ab9-bf84-46d6-9990-81f5c51f885a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28152ad8d8ed10eff6f0576a6fd9f508f0e8fc98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211733"
---
# <a name="syssp_rda_reconcile_indexes-transact-sql"></a>sys.sp_rda_reconcile_indexes (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  将架构任务排队以协调远程表上的索引。 此任务成功完成后，远程表与启用了本地 Stretch 的表中存在相同的索引。  
  
 如果在调用 **sp_rda_reconcile_indexes** 时有另一个排队等待协调索引的任务，则此存储过程不会将重复任务排队。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_rda_reconcile_indexes [@objname = ] 'objname'  
  
```  
  
## <a name="arguments"></a>参数  
 [ @objname =] *"objname"*  
 要对其对索引进行协调的已启用延伸的表的限定或非限定名称。 仅当指定限定对象时，才需要使用引号。  
  
## <a name="return-code-values"></a>返回代码值  
 0 (成功) 或 >0 (故障)   
  
## <a name="see-also"></a>另请参阅  
 [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)  
  
  
