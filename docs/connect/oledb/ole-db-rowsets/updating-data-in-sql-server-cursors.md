---
title: 更新游标中的数据（OLE DB 驱动程序）
description: 了解 OLE DB Driver for SQL Server 使用者如何使用 SQL Server 游标处理可修改行集中的请求。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- isolation levels [SQL Server]
- delayed update mode [OLE DB]
- immediate update mode [OLE DB]
- cursors [OLE DB]
- data updates [SQL Server], OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 53096897b248205f36dc0a479db0d6750ff4069f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755747"
---
# <a name="updating-data-in-sql-server-cursors"></a>更新 SQL Server 游标中的数据
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  当通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 游标提取和更新数据时，OLE DB Driver for SQL Server 使用者应用程序同样面临适用于任何其他客户端应用程序的注意事项和约束。  
  
 只有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 游标中的行才参与并发数据访问控制。 当使用者请求可修改的行集时，并发控制是通过 DBPROP_LOCKMODE 控制的。 若要修改并发访问控制的级别，使用者应在打开该行集之前设置 DBPROP_LOCKMODE 属性。  
  
 如果客户端应用程序设计使事务长时间保持打开状态，事务隔离级别在定位行时可能造成严重滞后。 默认情况下，适用于 SQL Server 的 OLE DB 驱动程序使用 DBPROPVAL_TI_READCOMMITTED 指定的已提交读隔离级别。 当行集并发为只读时，OLE DB Driver for SQL Server 支持脏读隔离。 因此，使用者可以在可修改行集中请求更高级别的隔离，但是不能成功请求任何更低级别。  
  
## <a name="immediate-and-delayed-update-modes"></a>立即更新模式和延迟更新模式  
 在立即更新模式中，对 IRowsetChange::SetData 的每次调用均导致与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之间发生一次往返。 如果使用者对一个行进行了多次更改，通过一个 SetData 调用提交所有更改将更有效  。  
  
 在延迟更新模式中，针对 IRowsetUpdate::Update 的 cRows 和 rghRows 参数中指示的每个行执行一次与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之间的往返    。  
  
 无论哪种模式，往返表示当未针对行集打开事务对象时的不同事务。  
  
 当使用 IRowsetUpdate::Update  时，OLE DB Driver for SQL Server 尝试处理指示的每个行。 因任何行的无效数据、长度或状态值引起的错误不能停止适用于 SQL Server 的 OLE DB 驱动程序处理。 可能修改参与更新的所有其他行，也可能不修改这些行。 当适用于 SQL Server 的 OLE DB 驱动程序返回 DB_S_ERRORSOCCURRED 时，使用者必须检查返回的 prgRowStatus 数组以便确定任何特定行是否失败  。  
  
 使用者不应假定行以任意特定顺序处理。 如果使用者要求按顺序处理基于多个行的数据修改，使用者应在应用程序逻辑中建立该顺序，并打开一个事务以包含该过程。  
  
## <a name="see-also"></a>另请参阅  
 [更新行集中的数据](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
  
