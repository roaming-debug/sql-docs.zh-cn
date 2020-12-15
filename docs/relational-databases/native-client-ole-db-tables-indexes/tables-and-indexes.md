---
description: SQL Server Native Client 中的表和索引
title: " (Native Client OLE DB 提供程序的表和索引) "
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, indexes
- OLE DB, tables
- ITableDefinition interface
- tables [OLE DB]
- IIndexDefinition interface
- SQL Server Native Client OLE DB provider, tables
- SQL Server Native Client OLE DB provider, indexes
- indexes [OLE DB]
ms.assetid: 4217c6d8-8cd2-43dc-b36f-3cfd8a58fabc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c02f27bd339b4777b679c4f79364ab851130b3d6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463428"
---
# <a name="tables-and-indexes-in-sql-server-native-client"></a>SQL Server Native Client 中的表和索引
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序公开 **IIndexDefinition** 和 **ITableDefinition** 接口，以便使用者可以创建、更改和删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表和索引。 表和索引定义是否有效取决于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本。  
  
 创建或删除表和索引的功能取决于使用者应用程序用户的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问权限。 删除表的功能还可以通过是否存在声明性引用完整性约束或其他因素进行进一步限制。  
  
 面向的大多数应用程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 都使用 sql-dmo，而不是使用这些 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序接口。 SQL-DMO 是支持所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理功能的 OLE 自动化对象的集合。 面向多个 OLE DB 访问接口的应用程序使用多种 OLE DB 访问接口支持的通用 OLE DB 接口。  
  
 在特定于访问接口的 DBPROPSET_SQLSERVERCOLUMN 属性集中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 定义了以下属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_COL_COLLATIONNAME|类型：VT_BSTR<br /><br /> R/W：写<br /><br /> 默认值：Null<br /><br /> 说明：该属性只能在 ITableDefinition 中使用  。 该属性中指定的字符串可在创建 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)<br /><br /> 语句时使用。|  
  
## <a name="in-this-section"></a>本节内容  
  
-   [创建 SQL Server 表](../../relational-databases/native-client-ole-db-tables-indexes/creating-sql-server-tables.md)  
  
-   [向 SQL Server 表添加列](../../relational-databases/native-client-ole-db-tables-indexes/adding-a-column-to-a-sql-server-table.md)  
  
-   [从 SQL Server 表中删除列](../../relational-databases/native-client-ole-db-tables-indexes/removing-a-column-from-a-sql-server-table.md)  
  
-   [删除 SQL Server 表](../../relational-databases/native-client-ole-db-tables-indexes/dropping-a-sql-server-table.md)  
  
-   [创建 SQL Server 索引](../../relational-databases/native-client-ole-db-tables-indexes/creating-sql-server-indexes.md)  
  
-   [删除 SQL Server 索引](../../relational-databases/native-client-ole-db-tables-indexes/dropping-a-sql-server-index.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)   
 [DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)  
  
  
