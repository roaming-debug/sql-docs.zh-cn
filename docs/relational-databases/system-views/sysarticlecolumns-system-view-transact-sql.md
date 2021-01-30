---
description: sysarticlecolumns（系统视图）(Transact-SQL)
title: sysarticlecolumns (系统视图)  (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticlecolumns
- sysarticlecolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticlecolumns view
ms.assetid: a8dd8d13-c827-45c4-87ba-802725301382
author: stevestein
ms.author: sstein
ms.openlocfilehash: 1de338795532eb470db25a4510f75dc3d6bf4239
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160346"
---
# <a name="sysarticlecolumns-system-view-transact-sql"></a>sysarticlecolumns（系统视图）(Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Sysarticlecolumns** 视图显示有关已发布项目中的列的其他信息。 此视图存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|标识项目。|  
|**colid**|**int**|标识项目中的列。|  
|**is_udt**|**int**|表示列是否为用户定义数据类型 (UDT) 列。 值为 **1** 指示 UDT 列。|  
|**is_xml**|**int**|如果列为 **xml** 列，则为。 值 **1** 指示 **xml** 列。|  
|**is_max**|**int**|如果列为大值数据类型列 (**varchar (max)**、 **nvarchar (max)** 或 **varbinary (max)**) 。 值 **1** 表示大值列。|  
  
## <a name="see-also"></a>另请参阅  
 [sp_articlecolumn (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-sql&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
