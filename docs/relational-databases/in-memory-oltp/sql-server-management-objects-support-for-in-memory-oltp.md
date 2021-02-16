---
title: SQL Server 管理对象支持 - 内存中 OLTP
description: 了解 SQL Server 管理对象 (SMO) 中支持内存中 OLTP 的项。 查看 Microsoft.SqlServer.Management.Smo 命名空间中的类型和成员。
ms.custom: seo-dt-2019
ms.date: 08/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2b67292d-6d8e-4016-9063-a97461ffe57a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fcacc0f64b33bedaeda87ef7a7a7b6363d1359a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353650"
---
# <a name="sql-server-management-objects-support-for-in-memory-oltp"></a>对内存中 OLTP 的 SQL Server 管理对象支持
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
本主题介绍 SQL Server 管理对象 (SMO) 中支持内存中 OLTP 的项。  

## <a name="smo-types-and-members"></a>SMO 的类型和成员

以下类型和成员位于命名空间 Microsoft.SqlServer.Management.Smo 中，且它们支持内存中 OLTP：

- **<xref:Microsoft.SqlServer.Management.Smo.DurabilityType>** （枚举）
- FileGroup. **<xref:Microsoft.SqlServer.Management.Smo.FileGroup.FileGroupType%2A>** （属性）
- FileGroup. **<xref:Microsoft.SqlServer.Management.Smo.FileGroup.%23ctor%2A>** （构造函数）
- **<xref:Microsoft.SqlServer.Management.Smo.FileGroupType>** （枚举）
- Index. **<xref:Microsoft.SqlServer.Management.Smo.Index.BucketCount%2A>** （属性）
- IndexType. **<xref:Microsoft.SqlServer.Management.Smo.IndexType.NonClusteredHashIndex>** （枚举成员）
- Index. **<xref:Microsoft.SqlServer.Management.Smo.Index.IsMemoryOptimized%2A>** （属性）
- Server. **<xref:Microsoft.SqlServer.Management.Smo.Server.IsXTPSupported%2A>** （属性）
- StoredProcedure. **<xref:Microsoft.SqlServer.Management.Smo.StoredProcedure.IsNativelyCompiled%2A>** （属性）
- StoredProcedure. **<xref:Microsoft.SqlServer.Management.Smo.StoredProcedure.IsSchemaBound%2A>** （属性）
- Table. **<xref:Microsoft.SqlServer.Management.Smo.Table.Durability%2A>** （属性）
- Table. **<xref:Microsoft.SqlServer.Management.Smo.Table.IsMemoryOptimized%2A>** （属性）
- UserDefinedTableType. **<xref:Microsoft.SqlServer.Management.Smo.UserDefinedTableType.IsMemoryOptimized%2A>** （属性）

## <a name="c-code-example"></a>C# 代码示例

#### <a name="assemblies-referenced-by-the-compiled-code-example"></a>由编译的代码示例引用的程序集

- Microsoft.SqlServer.ConnectionInfo.dll
- Microsoft.SqlServer.Management.Sdk.Sfc.dll
- Microsoft.SqlServer.Smo.dll
- Microsoft.SqlServer.SqlEnum.dll

#### <a name="actions-taken-in-the-code-example"></a>代码示例中执行的操作

1. 使用内存优化文件组和内存优化文件创建数据库。  
2. 使用主键、非聚集索引和非聚集哈希索引创建持久内存优化表。  
3. 创建列和索引。  
4. 创建用户定义的内存优化表类型。  
5. 创建本机编译的存储过程。

#### <a name="source-code"></a>源代码
  
```csharp
using Microsoft.SqlServer.Management.Smo;  
using System;  
  
public class A {  
   static void Main(string[] args) {  
      Server server = new Server("(local)");  
  
      // Create a database with memory-optimized filegroup and memory-optimized file.
      Database db = new Database(server, "MemoryOptimizedDatabase");  
      db.Create();  
      FileGroup fg = new FileGroup(
         db,
         "memOptFilegroup",
         FileGroupType.MemoryOptimizedDataFileGroup);  
      db.FileGroups.Add(fg);  
      fg.Create();  
      // Change this path if needed.
      DataFile file = new DataFile(
         fg,
         "memOptFile",
         @"C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\MSSQLmemOptFileName");  
      file.Create();  
  
      // Create a durable memory-optimized table with primary key, nonclustered index and nonclustered hash index.
      // Define the table as memory optimized and set the durability.
      Table table = new Table(db, "memOptTable");  
      table.IsMemoryOptimized = true;  
      table.Durability = DurabilityType.SchemaAndData;  
  
      // Create columns.
      Column col1 = new Column(table, "col1", DataType.Int);  
      col1.Nullable = false;  
      table.Columns.Add(col1);  
      Column col2 = new Column(table, "col2", DataType.Float);  
      col2.Nullable = false;  
      table.Columns.Add(col2);  
      Column col3 = new Column(table, "col3", DataType.Decimal(2, 10));  
      col3.Nullable = false;  
      table.Columns.Add(col3);  
  
      // Create indexes.
      Index pk = new Index(table, "PK_memOptTable");  
      pk.IndexType = IndexType.NonClusteredIndex;  
      pk.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      pk.IndexedColumns.Add(new IndexedColumn(pk, col1.Name));  
      table.Indexes.Add(pk);  
  
      Index ixNonClustered = new Index(table, "ix_nonClustered");  
      ixNonClustered.IndexType = IndexType.NonClusteredIndex;  
      ixNonClustered.IndexKeyType = IndexKeyType.None;  
      ixNonClustered.IndexedColumns.Add(
         new IndexedColumn(ixNonClustered, col2.Name));  
      table.Indexes.Add(ixNonClustered);  
  
      Index ixNonClusteredHash = new Index(table, "ix_nonClustered_Hash");  
      ixNonClusteredHash.IndexType = IndexType.NonClusteredHashIndex;  
      ixNonClusteredHash.IndexKeyType = IndexKeyType.None;  
      ixNonClusteredHash.BucketCount = 1024;  
      ixNonClusteredHash.IndexedColumns.Add(
         new IndexedColumn(ixNonClusteredHash, col3.Name));  
      table.Indexes.Add(ixNonClusteredHash);  
  
      table.Create();  
  
      // Create a user-defined memory-optimized table type.
      UserDefinedTableType uDTT = new UserDefinedTableType(db, "memOptUDTT");  
      uDTT.IsMemoryOptimized = true;  
  
      // Add columns.
      Column udTTCol1 = new Column(uDTT, "udtCol1", DataType.Int);  
      udTTCol1.Nullable = false;  
      uDTT.Columns.Add(udTTCol1);  
      Column udTTCol2 = new Column(uDTT, "udtCol2", DataType.Float);  
      udTTCol2.Nullable = false;  
      uDTT.Columns.Add(udTTCol2);  
      Column udTTCol3 = new Column(uDTT, "udtCol3", DataType.Decimal(2, 10));  
      udTTCol3.Nullable = false;  
      uDTT.Columns.Add(udTTCol3);  
  
      // Add index.
      Index ix = new Index(uDTT, "IX_UDT");  
      ix.IndexType = IndexType.NonClusteredHashIndex;  
      ix.BucketCount = 1024;  
      ix.IndexKeyType = IndexKeyType.DriPrimaryKey;  
      ix.IndexedColumns.Add(new IndexedColumn(ix, udTTCol1.Name));  
      uDTT.Indexes.Add(ix);  
  
      uDTT.Create();  
  
      // Create a natively compiled stored procedure.
      StoredProcedure sProc = new StoredProcedure(db, "nCSProc");  
      sProc.TextMode = false;  
      sProc.TextBody = "--Type body here";  
      sProc.IsNativelyCompiled = true;  
      sProc.IsSchemaBound = true;  
      sProc.ExecutionContext = ExecutionContext.Owner;  
      sProc.Create();  
   }  
}  
```  
  
## <a name="see-also"></a>另请参阅  

- [SQL Server 对内存中 OLTP 的支持](./transact-sql-support-for-in-memory-oltp.md)
- [SMO 概述](../server-management-objects-smo/overview-smo.md)