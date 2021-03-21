---
title: 使用适用于 SQL Server 的 OLE DB 驱动程序头文件和库文件 | Microsoft Docs
description: 了解如何在开发环境中使用 OLE DB Driver for SQL Server 头文件和库文件。
ms.custom: ''
ms.date: 02/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- header files [OLE DB Driver for SQL Server]
- MSOLEDBSQL, header files
- OLE DB, header files
- library files [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, header files
- data access [OLE DB Driver for SQL Server], header files
- data access [OLE DB Driver for SQL Server], library files
- OLE DB Driver for SQL Server, library files
- MSOLEDBSQL, library files
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9126ef09a59a04fe54c29050839de35afca84ca5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742947"
---
# <a name="using-the-ole-db-driver-for-sql-server-header-and-library-files"></a>使用适用于 SQL Server 的 OLE DB 驱动程序头文件和库文件
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  如果在安装过程中选择了“OLE DB Driver for SQL Server SDK”选项，则会安装 OLE DB Driver for SQL Server 头文件和库文件。 开发应用程序时，应将开发所需的所有文件复制到开发环境并进行安装，这一点非常重要。 有关安装和重新分发 OLE DB Driver for SQL Server 的详细信息，请参阅[安装 OLE DB Driver for SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md)。  
  
 OLE DB Driver for SQL Server 头文件和库文件安装在以下位置：  
  
 *%PROGRAM FILES%* \Microsoft SQL Server\Client SDK\OLEDB\182\SDK  
  
 OLE DB Driver for SQL Server 头文件 (msoledbsql.h) 可用于将 OLE DB Driver for SQL Server 数据访问功能添加到自定义应用程序。 在适用于 SQL Server 的 OLE DB 驱动程序头文件中，可找到利用 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的新功能所需的所有定义、特性、属性和接口。  
  
 除了 OLE DB Driver for SQL Server 头文件外，还存在 msoledbsql.lib 库文件，该文件是 [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md) 功能的导出库。  
  
 适用于 SQL Server 的 OLE DB 驱动程序头文件与 Microsoft 数据访问组件 (MDAC) 使用的 sqloledb.h 头文件向后兼容，但不包含 SQLOLEDB 的 CLSID（MDAC 附带的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] OLE DB 访问接口）或 XML 功能的符号（适用于 SQL Server 的 OLE DB 驱动程序不支持这些符号）。    
  
 使用 OLE DB Driver for SQL Server 的 OLE DB 应用程序只需引用 msoledbsql.h。 如果应用程序使用 MDAC (SQLOLEDB) 和适用于 SQL Server 的 OLE DB 驱动程序，则可同时引用 sqloledb.h 和 msoledbsql.h，但必须先引用 sqloledb.h。  
  
## <a name="using-the-ole-db-driver-for-sql-server-header-file"></a>使用 OLE DB Driver for SQL Server 头文件  
 若要使用 OLE DB Driver for SQL Server 头文件，必须在 C/C++ 编程代码中使用 include  语句。 以下几节介绍如何在 OLE DB 应用程序中执行此操作。  
  
> [!NOTE]  
>  OLE DB Driver for SQL Server 头文件和库文件只能使用 Visual Studio C++ 2012 或更高版本编译。  
  
### <a name="ole-db"></a>OLE DB  
 若要在 OLE DB 应用程序中使用 OLE DB Driver for SQL Server 头文件，请使用以下编程代码行：  
  
```    
include "msoledbsql.h";  
```  
  
> [!NOTE]  
>  如果应用程序的 sqloledb.h 具有用于 sqloledb.h 的 include  语句，msoledbsql.h 的 include  语句必须在该语句之后。  
  
 当通过 OLE DB Driver for SQL Server 创建与数据源的连接时，请将“MSOLEDBSQL”用作访问接口名称字符串。  

  
## <a name="component-names-and-properties-by-version"></a>基于版本的组件名称和属性  

|properties|适用于 SQL Server 的 OLE DB 驱动程序|MDAC|  
|--------|----------------------------|----|   
|OLE DB PROGID|MSOLEDBSQL|SQLOLEDB|  
|OLE DB 头文件名|msoledbsql.h|Sqloledb.h|  
|OLE DB 访问接口 DLL|msoledbsql.dll|Sqloledb.dll| 
  
  
## <a name="static-linking-and-bcp-functions"></a>静态链接和 BCP 函数  
 在某一应用程序使用 BCP 函数时，对于该应用程序十分需要特别注意的是，应在连接字符串中指定来自用于编译该应用程序的头文件和库随附的相同版本的驱动程序。  
  
 有关详细信息，请参阅[执行大容量复制操作](../../oledb/features/performing-bulk-copy-operations.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
  
  
