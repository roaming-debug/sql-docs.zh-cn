---
title: 行集（OLE DB 驱动程序）
description: 了解支持使用者在 OLE DB Driver for SQL Server 的会话中创建行集的接口。 有关详细信息，请参阅本节中的文章。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- OLE DB Driver for SQL Server, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e41444c601eac8829ce502e46aa96b784df8b799
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746077"
---
# <a name="rowsets"></a>行集
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  行集是一组包含数据列的行。 行集是使所有 OLE DB 数据访问接口能够以表格形式公开结果集数据的中心对象。  
  
 使用者使用 IDBCreateSession::CreateSession 方法创建会话之后，该使用者可以对会话使用 IOpenRowset 或 IDBCreateCommand 接口创建行集    。 OLE DB Driver for SQL Server 支持这两个接口。 下面描述这两种方法。  
  
-   可调用 IOpenRowset::OpenRowset 方法来创建行集  。  
  
     这等同于通过单个表创建行集。 该方法打开并返回行集，其中包括单个基表中的所有行。 OpenRowset 的一个参数是表 ID，它标识从其创建该行集的表  。  
  
-   可调用 IDBCreateCommand::CreateCommand 方法来创建命令对象  。  
  
     命令对象执行提供程序所支持的命令。 通过适用于 SQL Server 的 OLE DB 驱动程序，使用者可指定任何 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句，例如 SELECT 语句或对存储过程的调用。 使用命令对象创建行集的步骤如下：  
  
    1.  使用者在会话中调用 IDBCreateCommand::CreateCommand 方法，以获得一个命令对象，并请求该命令对象的 ICommandText 接口   。 此 ICommandText 接口设置并检索实际的命令文本  。 使用者通过调用 ICommandText::SetCommandText 方法填充文本命令  。  
  
    2.  用户调用该命令的 ICommand::Execute 方法  。 执行命令时生成的行集对象包含该命令产生的结果集。  
  
 使用者可以使用 ICommandProperties 接口获得或设置 ICommand::Execute 接口执行命令后返回的行集的属性   。 最经常请求的属性是行集必须支持的接口。 除了接口以外，使用者还可以请求能够修改行集或接口行为的属性。  
  
 使用者用 IRowset::Release 方法释放行集  。 释放行集时，将释放由该行集的使用者持有的任何行控点。 释放行集时，不会释放取值函数。 即使存在 IAccessor 接口，仍必须释放它  。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [使用 IOpenRowset 创建行集](../../oledb/ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [使用 ICommand::Execute 创建行集](../../oledb/ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [行集属性和行为](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [行集和 SQL Server 游标](../../oledb/ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [提取行](../../oledb/ole-db-rowsets/fetching-rows.md)  
  
-   [使用 IRow 提取单行](../../oledb/ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [书签](../../oledb/ole-db-rowsets/bookmarks.md)  
  
-   [更新行集中的数据](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
