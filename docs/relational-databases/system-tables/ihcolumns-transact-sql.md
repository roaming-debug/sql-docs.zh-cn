---
description: IHcolumns (Transact-SQL)
title: IHcolumns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHcolumns_TSQL
- IHcolumns
dev_langs:
- TSQL
helpviewer_keywords:
- IHcolumns system table
ms.assetid: 5bb027e5-5279-487b-9c33-5f402987253c
author: cawrites
ms.author: chadam
ms.openlocfilehash: e9d264dd2c3199527a763f5c2c8afce01dad17e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094813"
---
# <a name="ihcolumns-transact-sql"></a>IHcolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个已发布的列在 **IHcolumns** 系统表中各占一行。 该表用来定义非 SQL Server 发布服务器的列数据类型在发布时如何进行表示，它实际上是在非 SQL Server 数据库管理系统 (DBMS) 和 SQL Server 之间对数据类型进行映射。 此表存储在分发数据库中。  
  
## <a name="definition"></a>定义  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|column_id|**int**|标识已发布列。|  
|**publishercolumn_id**|**int**|将已发布的列与存储在 [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) 系统表中的列元数据相关联。|  
|name|**sysname**|指定列名。|  
|**article_id**|**int**|标识列所属的项目。|  
|**column_ordinal**|**int**|按顺序标识列。|  
|**mapped_type**|**tinyint**|订阅服务器中目标列的列数据类型。|  
|**mapped_length**|**bigint**|订阅服务器中列的长度。|  
|**mapped_prec**|**int**|订阅服务器中列的精度。|  
|**mapped_scale**|**int**|订阅服务器中列的小数位数。|  
|**mapped_nullable**|**bit**|指示订阅服务器上的列是否接受 NULL 值，其中 **1** 表示接受 null 值。|  
  
## <a name="see-also"></a>另请参阅  
 [异类数据库复制](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_articlecolumn (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;系统视图&#41; &#40;Transact-sql&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-sql&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
