---
description: sys.column_type_usages (Transact-SQL)
title: sys.column_type_usages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- column_type_usages
- sys.column_type_usages_TSQL
- column_type_usages_TSQL
- sys.column_type_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_type_usages catalog view
ms.assetid: 1ead375e-f662-4837-903f-8947496c51e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 328d98a5b8690cd92da0d130736e5254d2e94630
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190861"
---
# <a name="syscolumn_type_usages-transact-sql"></a>sys.column_type_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  具有用户定义类型的每一列在表中对应一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此列所属对象的 ID。|  
|column_id|**int**|列的 ID。 在对象中是唯一的。|  
|**user_type_id**|**int**|用户定义类型的 ID。<br /><br /> 若要返回类型的名称，请在此列上联接到 [sys.databases](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) 目录视图。|  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的标量类型目录视图 ](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
