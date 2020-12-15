---
title: 处理错误和消息 |Microsoft Docs
description: 了解应用程序调用 ODBC 函数时返回的诊断信息，包括成功或失败以及详细信息。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, about error handling
- errors [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], about messages
- ODBC error handling
- SQL_INVALID_HANDLE return code
- errors [ODBC], about error handling
- messages [ODBC]
ms.assetid: 74ea9630-e482-4a46-bb45-f5234f079b48
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 367142f4a605f95c81cbac34d4696a77f7a693ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438472"
---
# <a name="handling-errors-and-messages"></a>处理错误和消息
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当应用程序调用 ODBC 函数时，驱动程序执行该函数，并以两种方式返回诊断信息：返回代码指示 ODBC 函数总体成功或失败，诊断记录提供有关函数的详细信息。 诊断记录包含标题记录和状态记录。 即使该函数成功，也仍将至少返回一条诊断记录，即标题记录。  
  
 诊断信息用于在开发时捕获编程错误，例如硬编码的 SQL 语句中的无效句柄和语法错误。 此外，该信息还用于在运行时捕获运行时错误和警告，例如用户输入的 SQL 语句中的数据截断、规则冲突和语法错误。 程序逻辑通常基于返回代码。  
  
 例如，在应用程序调用 **SQLFetch** 来检索结果集中的行后，返回代码将指示是否已到达结果集的末尾 (SQL_NO_DATA) 、是否 (SQL_SUCCESS_WITH_INFO) 返回了任何信息性消息，或者 (SQL_ERROR) 发生错误。  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序返回 SQL_SUCCESS 以外的任何内容，则应用程序可以调用 **SQLGetDiagRec** 来检索任何信息或错误消息。 如果有多个消息，请使用 **SQLGetDiagRec** 向下和向下滚动消息集。  
  
 返回代码 SQL_INVALID_HANDLE 始终指示编程错误，并且决不能在运行时遇到。 所有其他返回代码提供运行时信息，但 SQL_ERROR 可以指示编程错误。  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于 C 的原始本机 API DB-Library 允许应用程序安装回调错误处理和消息处理函数，这些函数将返回错误或消息。 某些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（如 PRINT、RAISERROR、DBCC 和 SET）将其结果返回到 DB-Library 消息处理程序函数，而不是结果集。 但是，ODBC API 不具备这种回调功能。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序检测到返回的消息时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，它会将 ODBC 返回代码设置为 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR 并将消息作为一个或多个诊断记录返回。 因此，ODBC 应用程序必须仔细测试这些返回代码，并调用 **SQLGetDiagRec** 来检索消息数据。  
  
 有关跟踪错误的详细信息，请参阅[数据访问跟踪](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100))。 有关 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中添加的错误跟踪的增强功能的信息，请参阅[访问扩展事件日志中的诊断信息](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [处理生成消息的语句](../../relational-databases/native-client-odbc-error-messages/processing-statements-that-generate-messages.md)  
  
-   [诊断记录和字段](../../relational-databases/native-client-odbc-error-messages/diagnostic-records-and-fields.md)  
  
-   [本机错误号](../../relational-databases/native-client-odbc-error-messages/native-error-numbers.md)  
  
-   [SQLSTATE &#40;ODBC 错误代码&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md)  
  
-   [错误消息](../../relational-databases/native-client-odbc-error-messages/error-messages.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (ODBC)](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
