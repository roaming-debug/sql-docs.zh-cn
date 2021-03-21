---
description: 诊断记录和字段
title: 诊断记录和字段 |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- header records [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], diagnostic records
- ODBC error handling, diagnostic records
- SQLGetDiagField function
- diagnostic records [ODBC]
- errors [ODBC], diagnostic records
- fields [ODBC]
- status information [ODBC]
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2b0cbc0573f733a1fd7f0c225160e90f9dd206ac
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737677"
---
# <a name="diagnostic-records-and-fields"></a>诊断记录和字段
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  诊断记录与 ODBC 环境、连接、语句或描述符句柄关联。 当任何 ODBC 函数产生的返回代码不是 SQL_SUCCESS 或 SQL_INVALID_HANDLE 时，该函数调用的句柄就有相关联的诊断记录，其中包含信息性或错误性消息。 这些记录会一直保留，直到要通过该句柄调用其他函数，那时，记录就会被丢弃。 与一个句柄任一次调用相关联的诊断记录数目不受限制。  
  
 有两种类型的诊断记录：标题和状态。 标题记录为记录 0；状态记录为记录 1 及更大数字。 诊断记录包含适用于标题记录和状态记录的不同字段。 ODBC 组件还可以定义自己的诊断记录字段。  
  
 标题记录中的字段包含有关函数执行的一般信息，包括返回代码、行计数、状态记录数和所执行的语句的类型。 除非 ODBC 函数返回 SQL_INVALID_HANDLE，否则，总是会创建标题记录。 有关标头记录中字段的完整列表，请参阅 [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md)。  
  
 状态记录中的字段包含关于 ODBC 驱动程序管理器、驱动程序或数据源返回的特定错误或警告的信息，包括 SQLSTATE、本机错误号、诊断消息、列号和行号。 仅当函数返回 SQL_ERROR, SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_NEED_DATA 或 SQL_STILL_EXECUTING 时，才会创建状态记录。 有关状态记录中字段的完整列表，请参阅 **SQLGetDiagField**。  
  
 **SQLGetDiagRec** 检索单个诊断记录及其 ODBC SQLSTATE、本机错误号和诊断消息字段。 此功能类似于 ODBC 2。_x_**SQLError** 函数。 ODBC 3 中最简单的错误处理函数。*x* 重复调用 **SQLGetDiagRec** ，并将 *RecNumber* 参数设置为1，并按1递增 *RecNumber* ，直到 **SQLGetDiagRec** 返回 SQL_NO_DATA。 这与 ODBC 2 等效。*x* 应用程序调用 **SQLError** ，直到返回 SQL_NO_DATA_FOUND。  
  
 ODBC 3。*x* 比 ODBC 2 支持更多的诊断信息。*x*。 此信息存储在使用 **SQLGetDiagField** 检索的诊断记录的其他字段中。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序具有特定于驱动程序的诊断字段，可以通过 **SQLGetDiagField** 进行检索。 这些特定于驱动程序的字段的标签在 sqlncli.h 中定义。 使用这些标签可以检索与每条诊断记录关联的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 状态、严重级别、服务器名称、过程名称和行号。 此外，sqlncli.msi 包含驱动程序用于标识 Transact-sql 语句的代码定义（如果应用程序调用 *DiagIdentifier* 设置为 SQL_DIAG_DYNAMIC_FUNCTION_CODE 的 **SQLGetDiagField** ）。  
  
 ODBC 驱动程序管理器使用它从底层驱动程序缓存的错误信息来处理 **SQLGetDiagField** 。 在成功连接之前，ODBC 驱动程序管理器不会缓存特定于驱动程序的诊断字段。 如果在成功完成连接之前调用以获取特定于驱动程序的诊断字段， **SQLGetDiagField** 将返回 SQL_ERROR。 如果 ODBC 连接函数返回 SQL_SUCCESS_WITH_INFO，则该连接函数特定于驱动程序的诊断字段尚不可用。 只有在连接函数之后执行了另一个 ODBC 函数调用之后，才能开始为特定于驱动程序的诊断字段调用 **SQLGetDiagField** 。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用 **SQLGetDiagRec** 返回的信息，可以有效地使用 Native Client ODBC 驱动程序报告的大多数错误进行诊断。 不过，在有些情况下，特定于驱动程序的诊断字段返回的信息对于诊断错误非常重要。 当使用 Native Client ODBC 驱动程序为应用程序编写 ODBC 错误处理程序时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，最好也使用 **SQLGetDiagField** 来检索至少 SQL_DIAG_SS_MSGSTATE 和 SQL_DIAG_SS_SEVERITY 特定于驱动程序的字段。 如果特定错误可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代码的多个位置引发，SQL_DIAG_SS_MSGSTATE 会为 Microsoft 支持工程师专门指示错误的引发位置，有时，这有助于诊断问题。  
  
## <a name="see-also"></a>另请参阅  
 [处理错误和消息](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
