---
description: 更新 SQL Server Native Client 中行集中的数据
title: " (Native Client OLE DB 提供程序更新行集中的数据) "
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- rowsets [OLE DB], updating data
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets, updating data
- SQL Server Native Client OLE DB provider, data updates
- data updates [SQL Server], OLE DB
ms.assetid: 37769b1c-c480-419a-8c54-5cc420bf73db
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d3639e0fd6a72df7ea748cc0a2aa4daf79776f9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476048"
---
# <a name="updating-data-in-rowsets-in-sql-server-native-client"></a>更新 SQL Server Native Client 中行集中的数据
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当使用者更新包含数据的可修改行集时，Native Client OLE DB 提供程序更新数据。 当使用者请求支持 IRowsetChange 或 IRowsetUpdate 接口时，将创建一个可修改的行集   。  
  
 所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序可修改的行集使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标来支持行集。 行集属性 DBPROP_LOCKMODE 更改游标中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并发控制行为，并确定在可更新行集中提取行集的行和生成数据完整性错误的行为。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持更新前后的行同步。  
  
> [!NOTE]  
>  可使用 IRowChange::SetColumns 设置行对象的一个或多个指定列的值。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [更新 SQL Server 游标中的数据](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-sql-server-cursors.md)  
  
-   [重新同步行](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-rowsets-resynchronizing-rows.md)  
  
## <a name="see-also"></a>另请参阅  
 [行集](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
