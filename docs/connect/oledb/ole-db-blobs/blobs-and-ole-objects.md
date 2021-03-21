---
title: BLOB 和 OLE 对象（OLE DB 驱动程序）| Microsoft Docs
description: 了解 ISequentialStream 接口如何支持使用者访问作为 OLE DB Driver for SQL Server 中的二进制大型对象的 SQL Server 数据类型。
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- BLOBs
- storage object [OLE DB]
- OLE DB Driver for SQL Server, BLOBs
- large data, OLE objects
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80c1c1eff66c19b009dcfdd9164129734f6c4362
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749177"
---
# <a name="blobs-and-ole-objects"></a>BLOB 和 OLE 对象
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 公开 ISequentialStream 接口，以便支持使用者访问作为二进制大型对象 (BLOB) 的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ntext、text<a href="#text_note"><sup>1</sup></a>、image、varchar(max)、nvarchar(max)、varbinary(max) 和 xml 数据类型        。 通过对 ISequentialStream 执行 Read 方法，使用者可以用便于管理的方式成块检索大量数据   。

 <b id="text_note">[1]:</b>只有支持 UTF-8 的服务器才能使用 ISequentialStream 接口将 UTF-8 编码的数据插入旧版文本列。 如果不支持 UTF-8 的服务器尝试这么做，将导致驱动程序发布以下错误消息：“所选的列类型上不支持流式传输”。

 有关演示此功能的示例，请参阅[设置大型数据 (OLE DB)](../../oledb/ole-db-how-to/set-large-data-ole-db.md)。  
  
 当使用者在数据修改绑定的取值函数中提供接口指针时，适用于 SQL Server 的 OLE DB 驱动程序可以使用由使用者实现的 IStorage 接口。  
  
 对于大值数据类型，适用于 SQL Server 的 OLE DB 驱动程序检查 IRowset 和 DDL 接口中的类型大小假设。 具有 varchar、nvarchar 和 varbinary 数据类型且最大大小设置为无限制的列将通过架构行集和返回列数据类型的接口表示为 ISLONG。  
  
 适用于 SQL Server 的 OLE DB 驱动程序将 varchar(max)、varbinary(max) 和 nvarchar(max) 类型分别公开为 DBTYPE_STR、DBTYPE_BYTES 和 DBTYPE_WSTR  。  
  
 若要使用这些类型，应用程序可以使用以下选项：  
  
-   绑定为字节（DBTYPE_STR、DBTYPE_BYTES 和 DBTYPE_WSTR）。 如果缓冲区不够大，将发生截断，这与早期版本中的这些类型完全相同（但是现在可以使用更大的值）。  
  
-   绑定为字节并指定 DBTYPE_BYREF。  
  
-   绑定为 DBTYPE_IUNKNOWN 并使用流处理。  
  
 如果绑定为 DBTYPE_IUNKNOWN，则使用 ISequentialStream 流功能。 对于大型值数据类型，OLE DB Driver for SQL Server 支持将输出参数绑定为 DBTYPE_IUNKNOWN。 这是为了支持存储过程将这些数据类型作为返回值返回的情形，返回值将作为 DBTYPE_IUNKNOWN 返回给客户端。  
  
## <a name="storage-object-limitations"></a>存储对象限制  
  
-   OLE DB Driver for SQL Server 只能支持一个打开的存储对象。 尝试打开多个存储对象（以获取对多个 ISequentialStream 接口指针的引用）返回 DBSTATUS_E_CANTCREATE。  
  
-   在适用于 SQL Server 的 OLE DB 驱动程序中，DBPROP_BLOCKINGSTORAGEOBJECTS 只读属性的默认值为 VARIANT_TRUE。 因此，如果存储对象处于活动状态，某些方法（存储对象上的方法除外）将失败，并返回 E_UNEXPECTED。  
  
-   当创建引用由使用者实现的存储对象的行取值函数时，适用于 SQL Server 的 OLE DB 驱动程序必须了解该存储对象提供的数据长度。 使用者必须在用于创建取值函数的 DBBINDING 结构中绑定一个长度指示符。  
  
-   如果行包含多个大型数据值，且 DBPROP_ACCESSORDER 不为 DBPROPVAL_AO_RANDOM，使用者必须使用支持适用于 SQL Server 的 OLE DB 驱动程序游标的行集来检索行数据，或在检索其他行值之前处理所有大型数据值。 如果 DBPROP_ACCESSORDER 为 DBPROPVAL_AO_RANDOM，适用于 SQL Server 的 OLE DB 驱动程序将所有 xml 数据类型缓存为二进制大型对象 (BLOB)，以便以任意顺序进行访问。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [获取大型数据](../../oledb/ole-db-blobs/getting-large-data.md)  
  
-   [设置大型数据](../../oledb/ole-db-blobs/setting-large-data.md)  
  
-   [BLOB 输出参数的流支持](../../oledb/ole-db-blobs/streaming-support-for-blob-output-parameters.md)  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)        
 [使用大值类型](../../oledb/features/using-large-value-types.md)  
  
  
