---
description: TABLES (Transact-SQL)
title: 表 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- TABLES_TSQL
- TABLES
dev_langs:
- TSQL
helpviewer_keywords:
- TABLES view
- INFORMATION_SCHEMA.TABLES view
ms.assetid: 723a9e63-8f6e-4d6e-b570-468cfaf03201
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96ac7ae6f29bd9babf353b15a74b608de6a1e4fd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754107"
---
# <a name="tables-transact-sql"></a>TABLES (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为当前用户有权访问的当前数据库中的每个表或视图返回一行。  
  
 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|表限定符。|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|包含该表的架构的名称。<br /><br /> <strong> \* \* 重要 \* 信息 \* ：</strong>查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。 INFORMATION_SCHEMA 视图可能不完整，因为它们并未针对所有新功能进行更新。|  
|**TABLE_NAME**|**sysname**|表名或视图名。|  
|**TABLE_TYPE**|**varchar (** 10 **)**|表的类型。 可以是 VIEW 或 BASE TABLE。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)  
  
