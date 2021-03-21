---
title: 向 SQL Server 表中添加列（OLE DB 驱动程序）|Microsoft Docs
description: 了解 ITableDefinition::AddColumn 方法如何允许使用者向 OLE DB Driver for SQL Server 的 SQL Server 表中添加列。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- columns [OLE DB]
- AddColumn function
- OLE DB Driver for SQL Server, columns
- adding columns
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bb04db6e712c7c5fcd33d92f51b09ae44de770d9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749137"
---
# <a name="adding-a-column-to-a-sql-server-table"></a>向 SQL Server 表添加列
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 公开 ITableDefinition::AddColumn  函数。 利用此函数，使用者便可向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表中添加列。  
  
 向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表添加列时，OLE DB Driver for SQL Server 的使用者将受到如下约束：  
  
-   如果 DBPROP_COL_AUTOINCREMENT 为 VARIANT_TRUE，则 DBPROP_COL_NULLABLE 必须为 VARIANT_FALSE。  
  
-   如果相应列是使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] timestamp  数据类型定义的，则 DBPROP_COL_NULLABLE 必须为 VARIANT_FALSE。  
  
-   对于任何其他列定义，DBPROP_COL_NULLABLE 都必须为 VARIANT_TRUE。  
  
 在 pTableID 参数的 uName 联合的 pwszName 成员中，使用者将表名指定为 Unicode 字符串    。 pTableID 的 eKind 成员必须是 DBKIND_NAME   。  
  
 在 uName 联合（位于 DBCOLUMNDESC 参数 pColumnDesc 的 dbcid 成员中）的 pwszName 成员中，将新列的名称指定为 Unicode 字符串     。 eKind 成员必须为 DBKIND_NAME  。  
  
## <a name="see-also"></a>另请参阅  
 [表和索引](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md)  
  
  
