---
title: 使用 IMultipleResults 处理多个结果集 | Microsoft Docs
description: 了解使用者如何使用 IMultipleResults 接口处理执行 OLE DB Driver for SQL Server 命令所返回的结果。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- multiple rowsets
- rowsets [OLE DB], multiple
- IMultipleResults interface
- multiple-rowset results
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5c9c73e56b257251b59dbfb611ce15058d78602
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735405"
---
# <a name="using-imultipleresults-to-process-multiple-result-sets"></a>使用 IMultipleResults 处理多个结果集
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  使用者使用 IMultipleResults 接口处理执行适用于 SQL Server 的 OLE DB 驱动程序命令所返回的结果  。 当适用于 SQL Server 的 OLE DB 驱动程序提交要执行的命令后，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会执行该语句并返回任意结果。  
  
 客户端必须处理命令执行返回的所有结果。 由于适用于 SQL Server 的 OLE DB 驱动程序命令执行可以生成多个行集对象作为结果，因此，使用 IMultipleResults 接口可以确保应用程序数据检索完成由客户端启动的往返  。  
  
 以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句生成多个行集，某些行集包含 OrderDetails 表的行数据，某些行集包含 COMPUTE BY 子句的结果  ：  
  
```  
SELECT OrderID, FullPrice = (UnitPrice * Quantity), Discount,  
    Discounted = UnitPrice * (1 - Discount) * Quantity  
FROM OrderDetails  
ORDER BY OrderID  
COMPUTE  
    SUM(UnitPrice * Quantity), SUM(UnitPrice * (1 - Discount) * Quantity)  
    BY OrderID  
```  
  
 如果使用者执行包含该文本的命令并请求行集作为返回的结果接口，则只返回第一组行。 使用者可以处理返回的行集中的所有行。 但是，如果将 DBPROP_MULTIPLECONNECTIONS 数据源属性设置为 VARIANT_FALSE，并且未在连接上启用 MARS，则无法针对会话对象执行任何其他命令（适用于 SQL Server 的 OLE DB 驱动程序将不会创建其他连接），直到取消该命令为止。 如果在连接上启用了 MARS，当 DBPROP_MULTIPLECONNECTIONS 为 VARIANT_FALSE 时，适用于 SQL Server 的 OLE DB 驱动程序返回 DB_E_OBJECTOPEN 错误，如果存在活动事务，则返回 E_FAIL。  
  
 如果使用流式输出参数，并且应用程序在调用 IMultipleResults::GetResults 获取下一结果集之前尚未使用完所有已返回的输出参数值，则适用于 SQL Server 的 OLE DB 驱动程序也将返回 DB_E_OBJECTOPEN  。 如果未启用 MARS 并且连接正忙于运行的命令不生成行集或者生成的行集不是服务器游标，同时 DBPROP_MULTIPLECONNECTIONS 数据源属性设置为 VARIANT_TRUE，那么适用于 SQL Server 的 OLE DB 驱动程序将创建其他连接来支持并发命令对象，除非某个事务处于活动状态（在此情况下，将返回错误）。 事务和锁定是在每个连接的基础上通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 来管理的。 如果生成第二个连接，该独立连接上的命令不能共享锁定。 应通过对其他命令所请求的行保持锁定，小心确保一个命令不会阻塞另一个命令。 如果启用 MARS，连接上可以有多个活动命令；如果使用显式事务，所有命令都共享一个公用事务。  
  
 使用者可以使用 [ISSAbort::Abort](../../oledb/ole-db-interfaces/issabort-abort-ole-db.md) 或释放命令对象和派生行集所持有的所有引用来取消命令。  
  
 通过在所有实例中使用 IMultipleResults，使用者能够获取命令执行生成的所有行集，并正确确定何时取消命令执行和释放会话对象以供其他命令使用  。  
  
> [!NOTE]  
>  当使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 游标时，命令执行将创建游标。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 返回游标创建是成功还是失败；因此，与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的往返将在命令执行返回时完成。 随后，每个 GetNextRows 调用成为一个往返  。 这样，可以存在多个活动命令对象，每个对象分别处理作为服务器游标提取结果的行集。 有关详细信息，请参阅[行集和 SQL Server 游标](../../oledb/ole-db-rowsets/rowsets-and-sql-server-cursors.md)。  
  
## <a name="see-also"></a>另请参阅  
 [命令](../../oledb/ole-db-commands/commands.md)  
  
  
