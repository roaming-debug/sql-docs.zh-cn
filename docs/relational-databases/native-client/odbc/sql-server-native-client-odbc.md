---
title: ODBC
description: SQL Server 通过使用 SQL Server Native Client ODBC 驱动程序作为与 SQL Server 通信的 C 和 c + + 应用程序的本机 API 来支持 ODBC。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLNCLI, ODBC
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- data access [SQL Server Native Client], ODBC
- SQL Server Native Client ODBC driver
- ODBC
- SQL Server Native Client, ODBC
- ODBC, about SQL Server Native Client ODBC driver
ms.assetid: 811d5ba3-a2b8-48c0-adbc-8c91f041f458
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aa462e10c92e94582fb127667127a3f7c50a98ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467618"
---
# <a name="sql-server-native-client-odbc"></a>SQL Server Native Client (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 是应用程序编程接口 (API) 的标准定义，可用于访问关系型数据库或索引的顺序访问方法 (ISAM) 数据库中的数据。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序支持 ODBC，并将其作为可用于编写与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的 C 和 C++ 应用程序的本机 API 之一。  
  
 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序编写的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 程序通过 C 函数调用与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 进行通信。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序中实现了特定于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 ODBC 函数版本。 驱动程序将 SQL 语句传递给 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 并将语句的结果返回给应用程序。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序符合 Microsoft Win32 ODBC 3.51 规范。 驱动程序按照 ODBC 3.51 规范中定义的方式支持使用 ODBC 早期版本编写的应用程序。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [数据源名称和 64 位操作系统](../../../relational-databases/native-client/odbc/data-source-names-and-64-bit-operating-systems.md)  
  
-   [创建 SQL Server Native Client ODBC 驱动程序应用程序](../../../relational-databases/native-client/odbc/creating-a-driver-application.md)  
  
-   [与 SQL Server &#40;ODBC&#41;通信 ](../../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
-   [&#40;ODBC&#41;执行查询 ](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
-   [&#40;ODBC&#41;处理结果 ](../../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
-   [使用游标 &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
-   [&#40;ODBC&#41;执行事务 ](./performing-transactions-in-odbc.md)  
  
-   [处理错误和消息](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
-   [运行存储过程](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
-   [使用目录函数](../../../relational-databases/native-client/odbc/using-catalog-functions.md)  
  
-   [&#40;ODBC&#41;执行大容量复制操作 ](../../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
-   [管理 Text 和 Image 列](../../../relational-databases/native-client-odbc-text-image-columns/managing-text-and-image-columns.md)  
  
-   [ODBC 驱动程序性能事件探查](../../../relational-databases/native-client/odbc/profiling-odbc-driver-performance.md)  
  
-   [ODBC&#41;&#40;表值参数 ](../../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
-   [ODBC&#41;&#40;的日期和时间改进 ](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
  
-   [&#40;ODBC&#41;的大型 CLR User-Defined 类型 ](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)  
  
-   [FILESTREAM 支持 &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/filestream-support-odbc.md)  
  
-   [客户端连接中的服务主体名称 (SPN) (ODBC)](../../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md)  
  
-   [稀疏列支持 &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)  
  
-   [SQL Server Native Client &#40;ODBC&#41; 引用]()  
  
-   [ODBC 操作指南主题](../../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 编程](../../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [安装 SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
