---
description: sys.partition_schemes (Transact-SQL)
title: sys.partition_schemes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- partition_schemes_TSQL
- partition_schemes
- sys.partition_schemes_TSQL
- sys.partition_schemes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.partition_schemes catalog view
ms.assetid: ed557fd5-12b0-4cef-9e4f-440b02e99d1f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8502a019a21bae64583736d5ed3c940c42fbdef9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203792"
---
# <a name="syspartition_schemes-transact-sql"></a>sys.partition_schemes (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  对于作为分区方案的每个数据空间，都包含一个对应的行， **type** = PS。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||[&#40;transact-sql&#41;继承 sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)中的列。|  
|**function_id**|**int**|方案中使用的分区函数的 ID。|  
  
 有关此视图所继承的列的列表，请参阅 [sys.data_spaces &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
