---
title: 关于
description: 了解 SQL Server Native Client (SNAC) 的功能。 SQL Server Native Client 指 ODBC，SQL Server 的 OLE DB 驱动程序。
ms.date: 04/14/2017
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: native-client
ms.topic: conceptual
ms.assetid: e4d4fe39-0090-42a7-8405-6378370d11cb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b65b009d5dc88dc9d5a0cd5cc6f5592c8160c8bb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467498"
---
# <a name="sql-server-native-client"></a>SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SNAC 或 SQL Server Native Client 是一种可互换使用的术语，用于引用 SQL Server 的 ODBC 和 OLE DB 驱动程序。

> [!IMPORTANT] 
> SQL Server Native Client (SQLNCLI.MSI) 保持不推荐使用，不建议用于新的开发工作。 相反，请使用新的 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其将使用最新的服务器功能进行更新。

> [!NOTE]
> 有关详细信息和下载 SNAC 或 ODBC 驱动程序的详细信息，请参阅 [博客文章 SNAC 的生命周期](/archive/blogs/sqlreleaseservices/snac-lifecycle-explained)。
> 有关 SQL Server 的 ODBC 驱动程序的详细信息，请参阅 [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)。  

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]随一起发布的 Native client 功能的信息 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ，SQL Server native client 的最新可用版本：

-   [SQL Server Native Client 对 LocalDB 的支持](../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)  

-   [元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)  

-   [SQL Server Native Client 11.0 中的 UTF-16 支持](../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)  

-   [对高可用性、灾难恢复的 SQL Server Native Client 支持](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  

-   [访问扩展事件日志中的诊断信息](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client 中的 ODBC 支持在 Windows 7 SDK 中添加到标准 ODBC 的三个功能：  

-   异步执行与连接相关的操作。 有关详细信息，请参阅 [异步执行](../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)。  

-   C 数据类型扩展能力。 有关详细信息，请参阅 [ODBC 中的 C 数据类型](../../odbc/reference/develop-app/c-data-types-in-odbc.md)。  

     若要在 Native Client 中支持此功能 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，如果应用程序使用 ODBC 3.8，SQLGetDescField 可以返回 (的 **时间** 类型 **SQL_C_SS_TIME2**) 或 **SQL_C_SS_TIMESTAMPOFFSET** (，而不是 **)**。 有关详细信息，请参阅 [对 ODBC 日期和时间改进的数据类型支持](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md)。  

-   多次使用小型缓冲区调用 **SQLGetData** ，以检索大型参数值。 有关详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  

 下列主题描述 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Native Client 行为更改。  

-   调用 **ICommandWithParameters：： SetParameterInfo** 时，传递给 *pwszName* 参数的值必须是有效的标识符。 有关详细信息，请参阅 [ICommandWithParameters](../../relational-databases/native-client-ole-db-interfaces/icommandwithparameters.md)。  

-   **SQLDescribeParam** 将一致地返回符合 ODBC 规范的值。 有关详细信息，请参阅 [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md)。  

-   [处理字符转换时 ODBC 驱动程序行为的变化](../../relational-databases/native-client/features/odbc-driver-behavior-change-when-handling-character-conversions.md)  

## <a name="see-also"></a>请参阅  
[安装 SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
 [SQL Server Native Client 功能](../../relational-databases/native-client/features/sql-server-native-client-features.md)