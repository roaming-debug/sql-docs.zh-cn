---
title: 提取行（OLE DB 驱动程序）| Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 中的使用者如何使用 IRowset 接口提取行、从行获取数据以及管理行。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- fetching rows
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- IRowset interface
- OLE DB Driver for SQL Server, fetching
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43f50c291902c0fa488683992282470ee2ad9374
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753027"
---
# <a name="fetching-rows"></a>提取行
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  IRowset 接口是基础行集接口  。 IRowset 接口提供了用于按顺序提取行、从这些行中获得数据以及管理行的方法  。 使用者使用 IRowset 中的这些方法执行所有基本行集操作  。 这包括提取和释放行以及获得列值。  
  
 使用者获得行集的接口指针时，第一步通常是使用 IRowsetInfo::GetProperties 方法确定行集的功能  。 这将返回行集所公开的接口的相关信息，以及未显示为非重复接口的行集的功能，例如最大活动行数和可以同时有挂起更新的行数。  
  
 使用者的下一步是确定行集中列的特征（即元数据）。 对于该步骤，它们对简单列信息使用 IColumnsInfo 方法，或对扩展列信息使用 IColumnsRowset 方法   。 GetColumnInfo 方法返回以下信息  ：  
  
-   结果集中的列数。  
  
-   DBCOLUMNINFO 结构的数组，每列一个。  
  
     结构的顺序是列在行集中出现的顺序。 每个 DBCOLUMNINFO 结构都包括列元数据，例如列名称、列的序号、列值的最大可能长度、列的数据类型、精度和长度。  
  
-   指向单个分配块中的所有字符串值的存储区的指针。  
  
 使用者通过元数据或基于生成该行集的文本命令来确定它需要哪些列。 它通过对 IColumnsInfo 返回的列信息进行排序，或通过 IColumnsRowset 返回的列元数据行集中的序号，确定所需列的序号   。  
  
 IColumnsInfo 和 IColumnsRowset 接口用于提取行集中有关列的信息   。 IColumnsInfo 接口返回一组有限的信息，而 IColumnsRowset 提供所有元数据   。  
  
> [!NOTE]  
>  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本 7.0 和更早版本中，IColumnsInfo::GetColumnsInfo 所返回的可选元数据列 DBCOLUMN_COMPUTEMODE 会返回 DBSTATUS_S_ISNULL（而不是用于描述该列是否为计算列的值），因为无法确定基础列是否为计算列  。  
  
 序号用于指定到列的绑定。 绑定是将使用者的结构元素与列进行关联的结构。 绑定可以是对列的数据值、长度和状态值的绑定。  
  
 在取值函数中可收集一组绑定。 此函数可通过 IAccessor::CreateAccessor 方法创建  。 取值函数可以包含多个绑定，以便可以在单个调用中检索或设置多个列的数据。 使用者可以创建多个取值函数，以便在应用程序的不同部分匹配不同的使用模式。 它可以在行集仍然存在时创建和释放取值函数。  
  
 若要从数据库提取行，使用者可调用某个方法，例如 IRowset::GetNextRows 或 IRowsetLocate::GetRowsAt   。 这些提取操作将行数据从服务器放入提供程序的行缓冲区中。 使用者不能直接访问提供程序的行缓冲区。 使用者使用 IRowset::GetData 从提供程序的缓冲区将数据复制到使用者缓冲区，并使用 IRowsetChange::SetData 将数据更改从使用者缓冲区复制到提供程序缓冲区   。  
  
 使用者调用 GetData 方法，并传递行的句柄、取值函数的句柄和使用者分配的缓冲区的指针  。 GetData 转换数据，并返回在用于创建取值函数的绑定中指定的列  。 使用者可以使用不同取值函数和缓冲区对某行多次调用 GetData，因此使用者可以获得相同数据的多个副本  。  
  
 可以采用多种方式处理来自变长列的数据。 首先，可以将这样的列绑定到使用者的结构的有限部分。 这将导致当数据的长度超过缓冲区的长度时发生截断。 通过检查状态是否为 DBSTATUS_S_TRUNCATED，使用者可以确定是否已发生截断。 返回的长度始终是真实的字节长度，因此使用者还可以确定有多少数据被截断。  
  
 使用者完成提取或更新行后，它将用 ReleaseRows 方法释放它们  。 这将释放行集中行的副本所占的资源，并为新行腾出空间。 然后，使用者可以重复其提取或创建行并访问其中数据的循环。  
  
 当使用者完成对行集的处理时，它将调用 IAccessor::ReleaseAccessor 方法释放所有取值函数  。 它将对行集所公开的所有接口调用 IUnknown::Release 方法，以释放行集  。 释放行集时，它将强制释放使用者可能持有的任何剩余的行或取值函数。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [下次提取位置](../../oledb/ole-db-rowsets/fetching-rows-next-fetch-position.md)  
  
## <a name="see-also"></a>另请参阅  
 [行集](../../oledb/ole-db-rowsets/rowsets.md)  
  
  
