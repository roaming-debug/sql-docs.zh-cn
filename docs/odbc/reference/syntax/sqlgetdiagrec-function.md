---
description: SQLGetDiagRec 函数
title: SQLGetDiagRec 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagRec
helpviewer_keywords:
- SQLGetDiagRec function [ODBC]
ms.assetid: ebdbac93-3d68-438f-8416-ef1f08e04269
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93b5b85d6c20a58d314dcb7f7c53f391c15a1491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194181"
---
# <a name="sqlgetdiagrec-function"></a>SQLGetDiagRec 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetDiagRec** 返回包含错误、警告和状态信息的诊断记录的多个字段的当前值。 不同于 **SQLGetDiagField**，后者每次调用返回一个诊断字段， **SQLGetDiagRec** 将返回诊断记录的几个常用字段，包括 SQLSTATE、本机错误代码和诊断消息文本。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetDiagRec(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLCHAR *       SQLState,  
     SQLINTEGER *    NativeErrorPtr,  
     SQLCHAR *       MessageText,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   TextLengthPtr);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送一个句柄类型标识符，描述需要诊断的句柄的类型。 必须是下列选项之一：  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN 句柄仅由驱动程序管理器和驱动程序使用。 应用程序不应使用此句柄类型。 有关 SQL_HANDLE_DBC_INFO_TOKEN 的详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)。  
  
 *Handle*  
 送 *HandleType* 指示的类型的诊断数据结构的句柄。 如果 SQL_HANDLE_ENV *HandleType* ，则 *句柄* 可以是共享或非共享环境句柄。  
  
 *RecNumber*  
 送指示应用程序从中查找信息的状态记录。 状态记录从1开始编号。  
  
 *SQLState*  
 输出指向缓冲区的指针，将在此缓冲区中返回五个字符的 SQLSTATE 代码 (并终止诊断记录 *RecNumber* 的 NULL) 。 前两个字符指示类;接下来的三个指示子类。 此信息包含在 SQL_DIAG_SQLSTATE 诊断字段中。 有关详细信息，请参阅 [SQLSTATEs](../../../odbc/reference/develop-app/sqlstates.md)。  
  
 *NativeErrorPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回特定于数据源的本机错误代码。 此信息包含在 SQL_DIAG_NATIVE 诊断字段中。  
  
 *MessageText*  
 输出指向要返回诊断消息文本字符串的缓冲区的指针。 此信息包含在 SQL_DIAG_MESSAGE_TEXT 诊断字段中。 有关字符串的格式，请参阅 [诊断消息](../../../odbc/reference/develop-app/diagnostic-messages.md)。  
  
 如果 *MessageText* 为 NULL，则 *TextLengthPtr* 仍返回 (字符数据的 null 终止字符以外的字符总数) 可在 *MessageText* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送**MessageText* 缓冲区的长度（以字符为限）。 诊断消息文本没有最大长度。  
  
 *TextLengthPtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 null 终止字符) 可在 *\* MessageText* 中返回的字符总数 (。 如果可返回的字符数大于 *BufferLength*，则 *\* MessageText* 中的诊断消息文本将截断为 *BufferLength* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 **SQLGetDiagRec** 不会为自身发布诊断记录。 它使用以下返回值报告其自身执行的结果：  
  
-   SQL_SUCCESS：函数已成功返回诊断信息。  
  
-   SQL_SUCCESS_WITH_INFO： \* *MessageText* 缓冲区太小，无法容纳请求的诊断消息。 未生成任何诊断记录。 若要确定是否发生了截断，应用程序必须将 *BufferLength* 与可用的实际字节数（写入 **StringLengthPtr*）进行比较。  
  
-   SQL_INVALID_HANDLE： *HandleType* 和 *句柄* 指示的句柄不是有效的句柄。  
  
-   SQL_ERROR：发生以下情况之一：  
  
    -   *RecNumber* 为负数或0。  
  
    -   *BufferLength* 小于零。  
  
    -   使用异步通知时，句柄上的异步操作不完整。  
  
-   SQL_NO_DATA： *RecNumber* 大于在 handle 中指定的句柄所存在的诊断记录数 *。* 如果没有适用于 *句柄* 的诊断记录，该函数还将为任何正 *RecNumber* 返回 SQL_NO_DATA。  
  
## <a name="comments"></a>注释  
 当对 ODBC 函数的上一次调用返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，应用程序通常会调用 **SQLGetDiagRec** 。 但是，由于任何 ODBC 函数在每次调用时都可以发布零个或多个诊断记录，因此应用程序可以在任何 ODBC 函数调用之后调用 **SQLGetDiagRec** 。 应用程序可以多次调用 **SQLGetDiagRec** ，以返回诊断数据结构中的部分或全部记录。 ODBC 对可以存储在任何一次的诊断记录数没有限制。  
  
 **SQLGetDiagRec** 不能用于从诊断数据结构的标头返回字段。  (*RecNumber* 参数必须大于0。 ) 应用程序应调用 **SQLGetDiagField** 以实现此目的。  
  
 **SQLGetDiagRec** 仅检索与 *handle* 参数中指定的句柄关联的诊断信息。 如果应用程序调用另一个 ODBC 函数（ **SQLGetDiagRec**、 **SQLGetDiagField** 或 **SQLError** 除外），则对相同句柄上先前调用的任何诊断信息都将丢失。  
  
 应用程序可以通过循环（递增 *RecNumber*）来扫描所有诊断记录，只要 **SQLGetDiagRec** 返回 SQL_SUCCESS。 对 **SQLGetDiagRec** 的调用与标头和记录字段不相同。 应用程序可以在以后再次调用 **SQLGetDiagRec** 来检索记录中的字段，只要在过渡中调用了除 **SQLGetDiagRec**、 **SQLGetDiagField** 或 **SQLError** 以外的其他函数。 应用程序还可以通过调用 **SQLGetDiagField** 检索 "SQL_DIAG_NUMBER" 字段的值，然后调用多次 **SQLGetDiagRec** 来检索可用诊断记录总数的计数。  
  
 有关诊断数据结构字段的说明，请参阅 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)。 有关详细信息，请参阅 [使用 SQLGetDiagRec 和 SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) 和 [实现 SQLGetDiagRec 和 SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md)。  
  
 如果调用的 API 不是异步执行的 API，则将生成 HY010 "函数序列错误"。 但是，在异步操作完成之前，不能检索错误记录。  
  
## <a name="handletype-argument"></a>HandleType 参数  
 每个句柄类型都可以有与之关联的诊断信息。 *HandleType* 参数表示 *handle* 参数的句柄类型。  
  
 不能为环境、连接、语句和描述符句柄返回某些标头和记录字段。 对于其字段不适用的句柄，将在 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)中的 "标头字段" 和 "记录字段" 部分中指明。  
  
 如果 *HandleType* SQL_HANDLE_SENV （表示共享环境句柄），则对 **SQLGetDiagRec** 的调用将返回 SQL_INVALID_HANDLE。 但是，如果 *HandleType* SQL_HANDLE_ENV，则 *句柄* 可以是共享或非共享环境句柄。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|获取诊断标头的诊断记录或字段的字段|[SQLGetDiagField 函数](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [示例 ODBC 程序](../../../odbc/reference/sample-odbc-program.md)
