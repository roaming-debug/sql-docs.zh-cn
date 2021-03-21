---
title: 稀疏列支持 (OLE DB) | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 对稀疏列的支持，这些是为存储 null 值而优化的列。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12503fa6339dbf3f16a6564cab899b8802f8e3fb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755307"
---
# <a name="sparse-columns-support-ole-db"></a>稀疏列支持 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本主题提供有关 OLE DB Driver for SQL Server 对稀疏列支持的信息。 若要详细了解稀疏列，请参阅 [OLE DB Driver for SQL Server 中的稀疏列支持](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md)。 例如，请参阅[显示稀疏列的列和目录元数据 &#40;OLE DB&#41;](../../oledb/ole-db-how-to/display-column-and-catalog-metadata-for-sparse-columns-ole-db.md)。  
  
## <a name="ole-db-statement-metadata"></a>OLE DB 语句元数据  
 从 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 开始，可以使用新的 DBCOLUMNFLAGS 标记值 DBCOLUMNFLAGS_SS_ISCOLUMNSET。 应当为是 column_set 值的列设置该值  。 可以通过 IColumnsInfo::GetColumnsInfo 的 dwFlags  参数和 IColumnsRowset::GetColumnsRowset 返回的行集的 DBCOLUMN_FLAGS 列来检索 DBCOLUMNFLAGS 标志。  
  
## <a name="ole-db-catalog-metadata"></a>OLE DB 目录元数据  
 其他两个特定于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的列已添加到 DBSCHEMA_COLUMNS 中。  
  
|列名称|数据类型|值/注释|  
|-----------------|---------------|---------------------|  
|SS_IS_SPARSE|DBTYPE_BOOL|如果该列是稀疏列，它将具有值 VARIANT_TRUE；否则，为 VARIANT_FALSE。|  
|SS_IS_COLUMN_SET|DBTYPE_BOOL|如果该列是稀疏 column_set 列，则它具有值 VARIANT_TRUE；否则，为 VARIANT_FALSE  。|  
  
 还添加了其他两个架构行集。 这些行集具有与 DBSCHEMA_COLUMNS 相同的结构，但返回不同内容。 不管 column_set 成员身份是什么，DBSCHEMA_COLUMNS_EXTENDED 都将返回所有列  。 DBSCHEMA_SPARSE_COLUMN_SET 仅返回属于稀疏 column_set 的成员的列  。  
  
## <a name="ole-db-datatypecompatibility-behavior"></a>OLE DB DataTypeCompatibility 行为  
 在 DataTypeCompatibility=80  时（在连接字符串中）的行为与 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 客户端一致，如下所示：  
  
-   新架构行集是不可见的，并且在架构行集中没有它们的行。  
  
-   COLUMNS 行集中的新列是不可见的。  
  
-   不为 column_set 列设置 DBCOLUMNFLAGS_SS_ISCOLUMNSET  。  
  
-   为 column_set 列设置 DBCOMPUTEMODE_NOTCOMPUTED  。  
  
## <a name="ole-db-support-for-sparse-columns"></a>OLE DB 对稀疏列的支持  
 在 OLE DB Driver for SQL Server 中修改了以下 OLE DB 接口以支持稀疏列：  
  
|类型或成员函数|说明|  
|-----------------------------|-----------------|  
|IColumnsInfo::GetColumnsInfo|为 dwFlags 中的 column_set 列设置了新的 DBCOLUMNFLAGS 标记值 DBCOLUMNFLAGS_SS_ISCOLUMNSET   。<br /><br /> 为 column_set 列设置了 DBCOLUMNFLAGS_WRITE  。|  
|IColumsRowset::GetColumnsRowset|为 DBCOLUMN_FLAGS 中的 column_set 列设置了新的 DBCOLUMNFLAGS 标记值 DBCOLUMNFLAGS_SS_ISCOLUMNSET  。<br /><br /> 对于 column_set 列，将 DBCOLUMN_COMPUTEMODE 设置为 DBCOMPUTEMODE_DYNAMIC  。|  
|IDBSchemaRowset::GetSchemaRowset|DBSCHEMA_COLUMNS 返回两个新列：SS_IS_COLUMN_SET 和 SS_IS_SPARSE。<br /><br /> DBSCHEMA_COLUMNS 仅返回不属于 column_set 成员的列  。<br /><br /> 已添加两个新架构行集：不管 column_set 成员身份的稀疏性如何，DBSCHEMA_COLUMNS_EXTENDED 都将返回所有列  。 DBSCHEMA_SPARSE_COLUMN_SET 仅返回属于 column_set 成员的列  。 这些新行集具有与 DBSCHEMA_COLUMNS 相同的列和限制。|  
|IDBSchemaRowset::GetSchemas|IDBSchemaRowset::GetSchemas 将新行集 DBSCHEMA_COLUMNS_EXTENDED 和 DBSCHEMA_SPARSE_COLUMN_SET 的 GUID 包括在可用架构行集的列表中。|  
|ICommand::Execute|如果使用表中的 select **，它将返回不属于稀疏 column_set 成员的所有列，外加一个 XML 列（如果有），其中包含属于稀疏 column_set 成员的所有非 NULL 列的值\***    。|  
|IOpenRowset::OpenRowset|IOpenRowset::OpenRowset 通过同一表上的 select\* 查询返回与 ICommand::Execute 具有相同列的行集。|  
|ITableDefinition|对于稀疏列或 column_set 列，该接口没有更改  。 必须进行架构修改的应用程序必须直接执行正确的 [!INCLUDE[tsql](../../../includes/tsql-md.md)]。|  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
