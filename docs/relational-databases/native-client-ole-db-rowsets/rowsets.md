---
title: " (Native Client OLE DB 提供程序的行集) "
description: 使所有数据访问接口能够以表格形式公开结果集数据。 在 OLE DB 中，使用行集功能，这是一组包含数据列的行。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
ms.assetid: 5e7b3cbe-3670-4e18-8172-2226e0b6b142
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1af4116ce0318a4a88c66fc4560b4158b7501bc8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476068"
---
# <a name="rowsets-native-client-ole-db-provider"></a> (Native Client OLE DB 提供程序的行集) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  行集是一组包含数据列的行。 行集是使所有 OLE DB 数据访问接口能够以表格形式公开结果集数据的中心对象。  
  
 使用者使用 IDBCreateSession::CreateSession 方法创建会话之后，该使用者可以对会话使用 IOpenRowset 或 IDBCreateCommand 接口创建行集    。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持这两个接口。 下面描述这两种方法。  
  
-   可调用 IOpenRowset::OpenRowset 方法来创建行集  。  
  
     这等同于通过单个表创建行集。 该方法打开并返回行集，其中包括单个基表中的所有行。 OpenRowset 的一个参数是表 ID，它标识从其创建该行集的表  。  
  
-   可调用 IDBCreateCommand::CreateCommand 方法来创建命令对象  。  
  
     命令对象执行提供程序所支持的命令。 通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口，使用者可以指定任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，例如 SELECT 语句或对存储过程的调用。 使用命令对象创建行集的步骤如下：  
  
    1.  使用者在会话中调用 IDBCreateCommand::CreateCommand 方法，以获得一个命令对象，并请求该命令对象的 ICommandText 接口   。 此 ICommandText 接口设置并检索实际的命令文本  。 使用者通过调用 ICommandText::SetCommandText 方法填充文本命令  。  
  
    2.  用户调用该命令的 ICommand::Execute 方法  。 执行命令时生成的行集对象包含该命令产生的结果集。  
  
 使用者可以使用 ICommandProperties 接口获得或设置 ICommand::Execute 接口执行命令后返回的行集的属性   。 最经常请求的属性是行集必须支持的接口。 除了接口以外，使用者还可以请求能够修改行集或接口行为的属性。  
  
 使用者用 IRowset::Release 方法释放行集  。 释放行集时，将释放由该行集的使用者持有的任何行控点。 释放行集时，不会释放取值函数。 即使存在 IAccessor 接口，仍必须释放它  。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [使用 IOpenRowset 创建行集](../../relational-databases/native-client-ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [使用 ICommand::Execute 创建行集](../../relational-databases/native-client-ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [行集属性和行为](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [行集和 SQL Server 游标](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [提取行](../../relational-databases/native-client-ole-db-rowsets/fetching-rows.md)  
  
-   [使用 IRow 提取单行](../../relational-databases/native-client-ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [书签](../../relational-databases/native-client-ole-db-rowsets/bookmarks.md)  
  
-   [更新行集中的数据](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
