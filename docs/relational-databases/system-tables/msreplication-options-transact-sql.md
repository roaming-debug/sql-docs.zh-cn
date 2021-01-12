---
description: MSreplication_options (Transact-SQL)
title: MSreplication_options (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_options
- MSreplication_options_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_options system table
ms.assetid: 23cf10d7-8bc1-4368-b5eb-e5576421e776
author: cawrites
ms.author: chadam
ms.openlocfilehash: 263436f5bd3ff691191d74b29c7bced426ba4599
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098135"
---
# <a name="msreplication_options-transact-sql"></a>MSreplication_options (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSreplication_options** 表存储复制在内部使用的元数据。 该表存储在 **master** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**optname**|**sysname**|仅限内部使用。|  
|**value**|**bit**|仅限内部使用。|  
|**major_version**|**int**|仅限内部使用。|  
|**minor_version**|**int**|仅限内部使用。|  
|**revision**|**int**|仅限内部使用。|  
|**install_failures**|**int**|仅限内部使用。|  
  
## <a name="see-also"></a>另请参阅  
 [复制表 (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
