---
description: VIEWS (Transact-SQL)
title: Transact-sql)  (视图 |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- VIEWS_TSQL
- VIEWS
dev_langs:
- TSQL
helpviewer_keywords:
- VIEWS view
- INFORMATION_SCHEMA.VIEWS view
ms.assetid: 6119bc94-0b22-45d4-a34b-967afd810a9d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9ac17172022563318667954be53382f7ed22b484
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754067"
---
# <a name="views-transact-sql"></a>VIEWS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为当前数据库中的当前用户可访问的视图返回一行。  
  
 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称 **。**_view_name_。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|视图限定符。|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|包含该视图的架构名称。<br /><br /> **&#42;&#42; 重要 &#42;&#42;**  只是查找对象架构的可靠方法是查询 sys.databases 目录视图。|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|视图名。|  
|**VIEW_DEFINITION**|**nvarchar (** 4000 **)**|如果定义的长度大于 **nvarchar (** 4000 **)**，则此列为 NULL。 否则，该列是视图定义文本。|  
|**CHECK_OPTION**|**varchar (** 7 **)**|WITH CHECK OPTION 的类型。 如果最初的视图是使用 WITH CHECK OPTION 创建的，那么就为 CASCADE。 否则，返回 NONE。|  
|**IS_UPDATABLE**|**varchar (** 2 **)**|指定视图是否可更新。 始终返回 NO。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.views (Transact-SQL)](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)  
  
