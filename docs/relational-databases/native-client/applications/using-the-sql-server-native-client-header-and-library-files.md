---
title: 标头和库文件
description: 了解如何使用 SQL Server Native Client 的标头和库文件来开发应用程序。 将所需文件复制到开发环境。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- header files [SQL Server Native Client]
- SQLNCLI, header files
- OLE DB, header files
- library files [SQL Server Native Client]
- SQL Server Native Client, header files
- data access [SQL Server Native Client], header files
- SQL Server Native Client ODBC driver,header files
- data access [SQL Server Native Client], library files
- SQL Server Native Client, library files
- ODBC applications, header files
- SQLNCLI, library files
ms.assetid: 69889a98-7740-4667-aecd-adfc0b37f6f0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6aea3d0f81f03c5850902b18d5cf2511d66d4ddf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463408"
---
# <a name="using-the-sql-server-native-client-header-and-library-files"></a>使用 SQL Server Native Client 头文件和库文件
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件和库文件随 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 一起安装。 开发应用程序时，应将开发所需的所有文件复制到开发环境并进行安装，这一点非常重要。 有关安装和重新分发 Native Client 的详细信息 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，请参阅 [安装 SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件和库文件安装在以下位置：  
  
 *% PROGRAM FILES%* \Microsoft SQL Server\110\SDK  
  
 可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件 (sqlncli.h) 向自定义应用程序添加 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 数据访问功能。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件包含利用 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的新功能所需的所有定义、特性、属性和接口。  
  
 除 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件之外，还存在一个 sqlncli11.lib 库文件，该文件是 ODBC 的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大容量复制程序 (BCP) 功能的导出库。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件与用于 Microsoft 数据访问组件 (MDAC) 的 sqloledb.h 和 odbcss.h 头文件向后兼容，但不包含 SQLOLEDB 的 CLSID（MDAC 附带的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] OLE DB 访问接口）或 XML 功能的符号（[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 不支持这些符号）。  
  
 ODBC 应用程序不能在同一程序中引用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件 (sqlncli.h) 和 odbcss.h。 即使您不使用 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的任何功能，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件仍将替换旧的 odbcss.h 运行。  
  
 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口的 OLE DB 应用程序只需引用 sqlncli.h。 如果应用程序使用 MDAC (SQLOLEDB) 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口，则可以同时引用 sqloledb.h 和 sqlncli.h，但必须先引用 sqloledb.h。  
  
## <a name="using-the-sql-server-native-client-header-file"></a>使用 SQL Server Native Client 头文件  
 若要使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件，必须在 C/c + + 编程代码中使用 **include** 语句。 以下部分说明如何在 OLE DB 和 ODBC 应用程序中使用该头文件。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件和库文件只能使用 Visual Studio C++ 2002 或更高版本编译。  
  
### <a name="ole-db"></a>OLE DB  
 若要在 OLE DB 应用程序中使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件，请使用以下编程代码行：  
  
```  
#define _SQLNCLI_OLEDB_  
include "sqlncli.h";  
```  
  
> [!NOTE]  
>  如果应用程序同时使用 OLE DB 和 ODBC API，应省略上面显示的第一行代码。 此外，如果应用程序具有 sqloledb 的 **include** 语句，则 sqlncli.msi 的 **include** 语句必须位于它后面。  
  
 当通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 创建与数据源的连接时，请将“SQLNCLI11”用作访问接口名称字符串。  
  
### <a name="odbc"></a>ODBC  
 若要在 ODBC 应用程序中使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 头文件，请使用以下编程代码行：  
  
```  
#define _SQLNCLI_ODBC_  
include "sqlncli.h";  
```  
  
> [!NOTE]  
>  如果应用程序使用 OLE DB 和 ODBC Api，则应该省略上面显示的第一行代码。 此外，如果应用程序的 odbcss.h 具有 `#include` 语句，则应删除该语句。  
  
 当通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 创建与数据源的连接时，请将“SQL Server Native Client 11.0”用作驱动程序名称字符串。  
  
## <a name="component-names-and-properties-by-version"></a>基于版本的组件名称和属性  
  
|Property|SQL Server Native Client<br /><br /> SQL Server 2005|SQL Server Native Client 10.0<br /><br /> SQL Server 2008|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]|MDAC|  
|--------------|--------------------------------------------------|-------------------------------------------------------|---------------------------------------------------------------|----------|  
|ODBC 驱动程序名称|SQL Native Client|SQL Server Native Client 10.0|SQL Server Native Client 11.0|SQL Server|  
|ODBC 头文件名|Sqlncli.h|Sqlncli.h|Sqlncli.h|Odbcss.h|  
|ODBC 驱动程序 DLL|Sqlncli.dll|Sqlncl10.dll|Sqlncl11.dll|sqlsrv32.dll|  
|BCP API 的 ODBC 库文件|Sqlncli.lib|Sqlncli10.lib|Sqlncli11.lib|Odbcbcp.lib|  
|BCP API 的 ODBC DLL|Sqlncli.dll|Sqlncli10.dll|Sqlncli11.dll|Odbcbcp.dll|  
|OLE DB PROGID|SQLNCLI|SQLNCLI10|SQLNCLI11|SQLOLEDB|  
|OLE DB 头文件名|Sqlncli.h|Sqlncli.h|Sqlncli.h|Sqloledb.h|  
|OLE DB 访问接口 DLL|Sqlncli.dll|Sqlncli10.dll|Sqlncli11.dll|Sqloledb.dll|  
  
 sqlncli.h 通过 SQLNCLI_VER 宏支持多个版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client。 默认情况下，SQLNCLI_VER 默认为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 的最新版本。 若要生成使用 sqlncli10.dll 而非 sqlncli11.dll 的应用程序，请将 SQLNCLI_VER 设置为 10。  
  
## <a name="static-linking-and-bcp-functions"></a>静态链接和 BCP 函数  
 在某一应用程序使用 BCP 函数时，对于该应用程序十分需要特别注意的是，应在连接字符串中指定来自用于编译该应用程序的头文件和库随附的相同版本的驱动程序。  
  
 例如，如果您使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 编译某一应用程序，并且关联的库文件 (sqlncli11.lib) 和头文件 (sqlncli.h) 来自 \Program Files\Microsoft SQL Server\110\SDK，则应确保在连接字符串中指定（以 ODBC 为例）“DRIVER={SQL Server Native Client 11.0}”。  
  
 有关详细信息，请参阅[执行大容量复制操作](../../../relational-databases/native-client/features/performing-bulk-copy-operations.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用 SQL Server Native Client 生成应用程序](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
