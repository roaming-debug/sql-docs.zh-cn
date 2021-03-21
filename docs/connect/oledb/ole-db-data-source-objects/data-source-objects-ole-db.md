---
title: 数据源对象（OLE DB 驱动程序）| Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 使用者如何为提供程序创建数据源对象实例。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], data source objects
- OLE DB Driver for SQL Server, data source objects
- MSOLEDBSQL, data source objects
- OLE DB Driver for SQL Server, data source objects
- OLE DB data source objects [OLE DB Driver for SQL Server]
- data source objects [OLE DB]
- CLSID
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 53208b567de362b388e5fb2183c67dccdabb2d2d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735206"
---
# <a name="data-source-objects-ole-db"></a>数据源对象 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序将数据源一词用于 OLE DB 接口集，这些接口用于建立指向某一数据存储的链接，例如 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 创建访问接口的数据源对象的实例是 OLE DB Driver for SQL Server 使用者的第一个任务。  
  
 每个 OLE DB 访问接口都为自身声明一个类标识符 (CLSID)。 适用于 OLE DB Driver for SQL Server 的 CLSID 是 C/C++ GUID CLSID_MSOLEDBSQL（符号 MSOLEDBSQL_CLSID 将解析为所引用的 msoledbsql.h 文件中的正确 progid）。 通过 CLSID，使用者使用 OLE CoCreateInstance 函数生成数据源对象的实例  。  
  
 OLE DB Driver for SQL Server 是进程内服务器。 适用于 SQL Server 的 OLE DB 驱动程序对象的实例使用 CLSCTX_INPROC_SERVER 宏创建，以便指示可执行上下文。  
  
 适用于 SQL Server 的 OLE DB 驱动程序数据源对象公开 OLE DB 初始化接口，使用者可使用这些接口连接到现有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库。  
  
 通过 OLE DB Driver for SQL Server 建立的每个连接都自动设置以下选项：  
  
-   SET ANSI_WARNINGS ON  
  
-   SET ANSI_NULLS ON  
  
-   SET ANSI_PADDING ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET QUOTED_IDENTIFIER ON  
  
-   SET CONCAT_OF_NULL_YIELDS_NULL ON  
  
 此示例使用类标识符宏来创建适用于 SQL Server 的 OLE DB 驱动程序数据源对象并获取对其 IDBInitialize 接口的引用  。  
  
```  
IDBInitialize*   pIDBInitialize;  
HRESULT          hr;  
  
hr = CoCreateInstance(CLSID_MSOLEDBSQL, NULL, CLSCTX_INPROC_SERVER,  
    IID_IDBInitialize, (void**) &pIDBInitialize);  
  
if (SUCCEEDED(hr))  
{  
    //  Perform necessary processing with the interface.  
    pIDBInitialize->Uninitialize();  
    pIDBInitialize->Release();  
}  
else  
{  
    // Display error from CoCreateInstance.  
}  
```  
  
 在成功创建适用于 SQL Server 的 OLE DB 驱动程序数据源对象的实例后，使用者应用程序可通过初始化数据源和创建会话来继续。 OLE DB 会话提供允许数据访问和操作的接口。  
  
 适用于 SQL Server 的 OLE DB 驱动程序使与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的指定实例的首次连接成为成功的数据源初始化的一部分。 只要保持对数据源初始化接口的引用，或者在调用 IDBInitialize::Uninitialize 方法前，这一连接会一直保持  。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [数据源属性 (OLE DB)](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)  
  
-   [数据源信息属性](../../oledb/ole-db-data-source-objects/data-source-information-properties.md)  
  
-   [初始化和授权属性](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)  
  
-   [会话](../../oledb/ole-db-data-source-objects/sessions.md)  
  
-   [会话属性 - 适用于 SQL Server 的 OLE DB 驱动程序](../../oledb/ole-db-data-source-objects/session-properties-oledb-driver-for-sql-server.md)  
  
-   [持久化数据源对象](../../oledb/ole-db-data-source-objects/persisted-data-source-objects.md)  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
