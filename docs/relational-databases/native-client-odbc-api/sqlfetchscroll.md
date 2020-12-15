---
description: SQLFetchScroll
title: SQLFetchScroll |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLFetchScroll function
ms.assetid: 524a3985-a08d-4445-99e0-bb551a666615
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c87e00b8aab4879a9fa406cc5d2d60f1573f9fc7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465218"
---
# <a name="sqlfetchscroll"></a>SQLFetchScroll
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLFetchScroll** 将向应用程序返回一行数据。 使用 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)设置行集的大小。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序支持所有定义的提取说明 (例如 SQL_FETCH_RELATIVE) ，但具有以下限制：  
  
-   如果为语句定义了只进游标，则必须使用 SQL_FETCH_NEXT，尝试以任何其他方式执行提取都将导致返回错误。  
  
-   仅静态和键集驱动游标支持 SQL_FETCH_BOOKMARK。  
  
## <a name="sqlfetchscroll-support-for-enhanced-date-and-time-features"></a>SQLFetchScroll 对日期和时间增强功能的支持  
 日期/时间类型的结果列值按 [从 SQL 到 C 的转换](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-sql-to-c.md)中所述进行转换。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlfetchscroll-support-for-large-clr-udts"></a>SQLFetchScroll 对大型 CLR UDT 的支持  
 **SQLFetchScroll** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLFetchScroll 函数](../../odbc/reference/syntax/sqlfetchscroll-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
