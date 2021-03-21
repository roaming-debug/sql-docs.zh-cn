---
title: 适用于 SQL Server 的 OLE DB 驱动程序的组件 | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 的组件，其中包括包含驱动程序功能的库、其他库和头文件。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], components
- components [OLE DB Driver for SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b4d4ea136dbc216f758bed78caff5d84382caa45
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751277"
---
# <a name="components-of-ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序的组件
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 包含以下组件：  

|组件|说明|  
|---------------|-----------------|  
|msoledbsql.dll|包含 OLE DB Driver for SQL Server 所有功能的动态链接库 (DLL) 文件。|  
|msoledbsqlr.rll|OLE DB Driver for SQL Server 库的附带资源文件。|   
|msoledbsql.h|包含使用 OLE DB Driver for SQL Server 所需的所有新定义的 OLE DB Driver for SQL Server 头文件。 该头文件取代了 sqloledb.h 头文件。<br /><br /> 注意：只要先定义了 sqloledb.h，就可以在同一个程序中引用 msoledbsql.h 和 sqloledb.h。|  
|msoledbsql.lib|直接调用属于 OLE DB Driver for SQL Server 的一部分的 [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md) 函数所需的库文件。<br /><br /> 注意：如果确实要在编程代码中引用 msoledbsql.lib 文件，需要确保 msoledbsql.dll 文件位于你的系统路径和使用你的应用程序的用户系统路径中。|  

## <a name="see-also"></a>另请参阅  
 [使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
