---
title: 删除 SQL Server 表（OLE DB 驱动程序）| Microsoft Docs
description: 了解如何使用 OLE DB Driver for SQL Server 中的 ITableDefinition::DropTable 函数来从数据库中删除 SQL Server 表。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- deleting tables
- OLE DB Driver for SQL Server, tables
- removing tables
- dropping tables
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1527835952024f2a10807f98749db85a8244f3a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754247"
---
# <a name="dropping-a-sql-server-table"></a>删除 SQL Server 表
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 公开 ITableDefinition::DropTable 函数，以从数据库中删除   表[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  
  
 在 pTableID 参数的 uName 联合的 pwszName 成员中，将表名指定为 Unicode 字符串    。 pTableID 的 eKind 成员必须是 DBKIND_NAME   。  
  
## <a name="see-also"></a>另请参阅  
 [表和索引](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)  
  
  
