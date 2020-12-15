---
description: SQLGetTypeInfo
title: SQLGetTypeInfo |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetTypeInfo function
ms.assetid: 13b982c3-ae03-4155-bc0d-e225050703ce
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9bea9cc9285d99c5632dc443a135008ff5f3799
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485059"
---
# <a name="sqlgettypeinfo"></a>SQLGetTypeInfo
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序在 **SQLGetTypeInfo** 的结果集中报告额外的列 USERTYPE。 USERTYPE 报告 DB-Library 数据类型定义，这对需要将现有 DB-Library 应用程序移植到 ODBC 的开发人员很有用。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将标识视为属性，而 ODBC 将它视为数据类型。 若要解决此不匹配， **SQLGetTypeInfo** 返回数据类型： **intidentity**、 **smallintidentity**、 **tinyintidentity**、 **decimalidentity** 和 **numericidentity**。 **SQLGetTypeInfo** 结果集列 AUTO_UNIQUE_VALUE 报告这些数据类型的值为 TRUE。  
  
 对于 **varchar**、 **nvarchar** 和 **varbinary**， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序会分别针对 COLUMN_SIZE 值继续报告8000、4000和8000，即使它实际上是不受限制的。 这是为了确保向后兼容性。  
  
 对于 **xml** 数据类型， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序报告 SQL_SS_LENGTH_UNLIMITED COLUMN_SIZE 以表示不受限制。  
  
## <a name="sqlgettypeinfo-and-table-valued-parameters"></a>SQLGetTypeInfo 和表值参数  
 表值参数的表类型实际上是一个元类型，即用于定义其他类型的类型。 因此，无需通过 SQLGetTypeInfo 公开。 应用程序必须使用 SQLTables （而不是 SQLGetTypeInfo）来检索用于表值参数的表类型的元数据。  
  
 有关检索表值参数的元数据的详细信息，请参阅 [影响 Table-Valued 参数的语句属性](../../relational-databases/native-client-odbc-table-valued-parameters/statement-attributes-that-affect-table-valued-parameters.md)。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlgettypeinfo-support-for-enhanced-date-and-time-features"></a>SQLGetTypeInfo 对日期和时间增强功能的支持  
 有关为日期/时间类型返回的值，请参阅 [目录元数据](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md)。  
  
 有关更多常规信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlgettypeinfo-support-for-large-clr-udts"></a>SQLGetTypeInfo 对大型 CLR UDT 的支持  
 **SQLGetTypeInfo** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLGetTypeInfo 函数](../../odbc/reference/syntax/sqlgettypeinfo-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
