---
title: 执行大容量复制操作
description: 了解如何使用 OLE DB Driver for SQL Server 执行大容量复制操作，以及它如何实现将数据快速传输到数据库。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- bulk copy [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], bulk copy operations
- OLE DB Driver for SQL Server, bulk copy operations
- MSOLEDBSQL, bulk copy operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 13ddae176474396d5bbb718051069b67f60c0a8c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751187"
---
# <a name="performing-bulk-copy-operations"></a>执行大容量复制操作
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大容量复制功能支持将大量数据传输到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表或视图中，或从其中传出。 也可以通过指定 SELECT 语句将数据传出。 可以在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和操作系统的数据文件（比如 ASCII 文件）之间移动数据。 数据文件可以有不同格式；定义格式是为了对格式文件中的数据进行大容量复制。 也可以选择将数据加载到程序变量中，然后使用大容量复制函数和方法将数据传输到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  
  
 有关演示此功能的示例应用程序，请参阅[使用 IRowsetFastLoad 大容量复制数据 (OLE DB)](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md)。  
  
 应用程序通常按以下方式之一使用大容量复制：  
  
-   从表、视图或 Transact-SQL 语句的结果集，将数据大容量复制到以与该表或视图相同的格式存储数据的数据文件中。  
  
     这称为本机模式数据文件。  
  
-   从表、视图或 Transact-SQL 语句的结果集，将数据大容量复制到以不同于该表或视图的格式存储数据的数据文件中。  
  
     这种情况下，将创建一个单独的格式文件，用于定义每个列存储在数据文件中时的特征（数据类型、位置、长度、终止符等等）。 如果所有列均转换为字符格式，则所产生的文件称为字符模式的数据文件。  
  
-   从数据文件大容量复制到表或视图中。  
  
     如果需要，则使用格式文件确定数据文件的布局。  
  
-   将数据加载到程序变量中，然后使用大容量复制函数将数据导入表或视图中，以便一次一行执行大容量复制。  
  
 大容量复制函数使用的数据文件不必由另一个大容量复制程序创建。 任何其他系统都可以按照大容量复制的定义生成数据文件和格式文件；然后，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大容量复制程序可以使用这些文件将数据导入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中。 例如，可以将数据从电子表格导出到以制表符分隔的文件中，然后生成描述该制表符分隔文件的格式文件，之后使用大容量复制程序将此数据快速导入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中。 还可以将大容量复制所生成的数据文件导入其他应用程序中。 例如，可以使用大容量复制函数将数据从表或视图导出到以制表符分隔的文件中，然后可以将该文件加载到电子表格中。  
  
 编写应用程序以使用大容量复制函数的程序员应当遵从常规规则，以获得良好的大容量复制性能。 有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中对大容量复制操作的支持的详细信息，请参阅[批量导入和导出数据 (SQL Server)](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 CLR 用户定义类型 (UDT) 必须绑定为二进制数据。 即使格式文件指定将 SQLCHAR 作为目标 UDT 列的数据类型，BCP 实用工具仍会将数据视为二进制。  
  
 不要对大容量复制操作使用 SET FMTONLY OFF。 SET FMTONLY OFF 可能导致大容量复制操作失败，或导致意外的结果。  
  
## <a name="ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序 
 OLE DB Driver for SQL Server 实现两个方法来对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库执行大容量复制操作。 第一个方法涉及使用 [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) 接口进行基于内存的大容量复制操作；第二个方法涉及使用 [IBCPSession](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md) 接口进行基于文件的大容量复制操作。  
  
### <a name="using-memory-based-bulk-copy-operations"></a>使用基于内存的大容量复制操作  
 适用于 SQL Server 的 OLE DB 驱动程序实现了 IRowsetFastLoad 接口，以公开对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 基于内存的大容量复制操作的支持  。 IRowsetFastLoad 接口实现了 [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md) 和 [IRowsetFastLoad::InsertRow](../../oledb/ole-db-interfaces/irowsetfastload-insertrow-ole-db.md) 方法  。  
  
#### <a name="enabling-a-session-for-irowsetfastload"></a>为 IRowsetFastLoad 启用会话  
 通过将特定于 OLE DB Driver for SQL Server 的数据源属性 SSPROP_ENABLEFASTLOAD 设置为 VARIANT_TRUE，使用者将其对大容量复制的需要通知 OLE DB Driver for SQL Server。 通过对数据源设置该属性，使用者创建 OLE DB Driver for SQL Server 会话。 新会话允许使用者访问 IRowsetFastLoad 接口  。  
  
> [!NOTE]  
>  如果 IDataInitialize 接口用于初始化数据源，则需要在 IOpenRowset::OpenRowset 方法的 rgPropertySets 参数中设置 SSPROP_IRowsetFastLoad 属性；否则，对 OpenRowset 方法的调用将返回 E_NOINTERFACE     。  
  
 启用大容量复制会话将会约束 OLE DB Driver for SQL Server 在会话中对接口的支持。 启用大容量复制的会话仅公开以下接口：  
  
-   **IDBSchemaRowset**  
  
-   **IGetDataSource**  
  
-   **IOpenRowset**  
  
-   **ISupportErrorInfo**  
  
-   **ITransactionJoin**  
  
 若要禁止创建启用大容量复制的行集，并使适用于 SQL Server 的 OLE DB 驱动程序会话恢复到标准处理，请将 SSPROP_ENABLEFASTLOAD 重置为 VARIANT_FALSE。  
  
#### <a name="irowsetfastload-rowsets"></a>IRowsetFastLoad 行集  
 适用于 SQL Server 的 OLE DB 驱动程序大容量复制行集是只写的，但它们公开的接口允许使用者确定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表的结构。 启用大容量复制的 OLE DB Driver for SQL Server 行集公开了以下接口：  
  
-   **IAccessor**  
  
-   **IColumnsInfo**  
  
-   **IColumnsRowset**  
  
-   **IConvertType**  
  
-   **IRowsetFastLoad**  
  
-   **IRowsetInfo**  
  
-   **ISupportErrorInfo**  
  
 特定于访问接口的属性 SSPROP_FASTLOADOPTIONS、SSPROP_FASTLOADKEEPNULLS 和 SSPROP_FASTLOADKEEPIDENTITY 控制适用于 SQL Server 的 OLE DB 驱动程序大容量复制行集的行为。 属性是在 rgPropertySets  IOpenRowset  参数成员的 rgProperties  成员中指定的。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_FASTLOADKEEPIDENTITY|列：否<br /><br /> R/W：读取/写入<br /><br /> 键入：VT_BOOL<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明:保留使用者提供的标识值。<br /><br /> VARIANT_FALSE：[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表中的标识列的值由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 生成。 OLE DB Driver for SQL Server 将忽略该列绑定的任何值。<br /><br /> VARIANT_TRUE：使用者绑定可提供 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识列的值的取值函数。 在接受 NULL 的列上，标识属性不可用，因此使用者为每个 IRowsetFastLoad::Insert 调用提供一个唯一的值  。|  
|SSPROP_FASTLOADKEEPNULLS|列：否<br /><br /> R/W：读取/写入<br /><br /> 键入：VT_BOOL<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明:对于具有 DEFAULT 约束的列，将保持 NULL。 仅影响接受 NULL 并应用了 DEFAULT 约束的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 列。<br /><br /> VARIANT_FALSE：当适用于 SQL Server 的 OLE DB 驱动程序使用者插入一个该列包含 NULL 的行时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 插入该列的默认值。<br /><br /> VARIANT_TRUE：当适用于 SQL Server 的 OLE DB 驱动程序使用者插入一个该列包含 NULL 的行时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 插入该列的 NULL。|  
|SSPROP_FASTLOADOPTIONS|列：否<br /><br /> R/W：读取/写入<br /><br /> 键入：VT_BSTR<br /><br /> 默认值：无<br /><br /> 说明:此属性与 bcp  实用工具的 -h  “hint  [,...n  ]”选项相同。 以下字符串可以在将数据大容量复制到表中时作为选项使用。<br /><br /> **ORDER**(*column*[**ASC** | **DESC**][,...*n*])：数据文件中数据的排序顺序。 如果按照表的聚集索引对加载的数据文件进行排序，将会提高大容量复制性能。<br /><br /> **ROWS_PER_BATCH** = *bb*：每批数据的行数（即 *bb*）。 服务器根据 *bb* 值优化大容量加载。 默认情况下，ROWS_PER_BATCH 是未知的  。<br /><br /> **KILOBYTES_PER_BATCH** = *cc*：每批数据的千字节 (KB) 数（即 cc）。 默认情况下，KILOBYTES_PER_BATCH 是未知的  。<br /><br /> **TABLOCK**：在大容量复制操作期间，将获得表级锁定。 由于仅在大容量复制操作期间持有锁定会减少对表的锁定争用，因此该选项极大地提高了性能。 如果表没有索引并且指定了 TABLOCK，则该表可以同时由多个客户端加载  。 默认情况下，锁定行为由表选项 table lock on bulk load 确定  。<br /><br /> **CHECK_CONSTRAINTS**：在大容量复制操作期间，将检查对 table_name  的任何约束。 默认情况下，忽略约束。<br /><br /> FIRE_TRIGGER：[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 对触发器使用行版本控制，并将行版本存储在 tempdb 的版本存储中   。 因此，即使是在启用触发器时，也可以进行大容量记录优化。 在启用触发器的情况下对有很多行的批数据执行大容量导入之前，可能需要增加 tempdb 的大小  。|  
  
### <a name="using-file-based-bulk-copy-operations"></a>使用基于文件的大容量复制操作  
 适用于 SQL Server 的 OLE DB 驱动程序实现了 IBCPSession 接口，以公开对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 基于文件的大容量复制操作的支持  。 IBCPSession 接口实现了 [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md)、[IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md)、[IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md)、[IBCPSession::BCPDone](../../oledb/ole-db-interfaces/ibcpsession-bcpdone-ole-db.md)、[IBCPSession::BCPExec](../../oledb/ole-db-interfaces/ibcpsession-bcpexec-ole-db.md)、[IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md)、[IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) 和 [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md) 方法  。  
  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [数据源属性 (OLE DB)](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)   
 [批量导入和导出数据 (SQL Server)](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)   
 [IBCPSession (OLE DB)](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   

