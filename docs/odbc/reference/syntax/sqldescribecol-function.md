---
description: SQLDescribeCol 函数
title: SQLDescribeCol 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeCol
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeCol
helpviewer_keywords:
- SQLDescribeCol function [ODBC]
ms.assetid: eddef353-83f3-4a3c-8f24-f9ed888890a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ceffc2b0db32ccce75c566c89ac3d69b535abe29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192082"
---
# <a name="sqldescribecol-function"></a>SQLDescribeCol 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 对于结果集中的一列， **SQLDescribeCol** 将返回结果描述符-列名称、类型、列大小、十进制数字和为 null 性。 此信息还可用于 IRD 的字段。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLDescribeCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLCHAR *      ColumnName,  
      SQLSMALLINT    BufferLength,  
      SQLSMALLINT *  NameLengthPtr,  
      SQLSMALLINT *  DataTypePtr,  
      SQLULEN *      ColumnSizePtr,  
      SQLSMALLINT *  DecimalDigitsPtr,  
      SQLSMALLINT *  NullablePtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 ColumnNumber   
 送按升序顺序排列的结果数据的列数，从1开始。 还可以将 *ColumnNumber* 参数设置为0来描述书签列。  
  
 *ColumnName*  
 输出指向以 null 结尾的缓冲区的指针，将在该缓冲区中返回列名称。 此值从 IRD 的 "SQL_DESC_NAME" 字段读取。 如果列未命名或无法确定列名，则驱动程序将返回空字符串。  
  
 如果 *ColumnName* 为 NULL， *NameLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的字符总数) 可在 *ColumnName* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送**ColumnName* 缓冲区的长度（以字符为限）。  
  
 *NameLengthPtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (排除 null 终止) 可在 ColumnName 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength*，则 ColumnName 中的列名称将被 \* 截断为 *BufferLength* 减去 null 终止字符的长度。  
  
 *DataTypePtr*  
 输出指向要在其中返回列的 SQL 数据类型的缓冲区的指针。 此值从 IRD 的 "SQL_DESC_CONCISE_TYPE" 字段读取。 这将是 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md)中的值之一，或特定于驱动程序的 sql 数据类型。 如果无法确定数据类型，则驱动程序将返回 SQL_UNKNOWN_TYPE。  
  
 ODBC 3 中的。在 *\* DataTypePtr* 中，分别为日期、时间或时间戳数据返回 *x*、SQL_TYPE_DATE、SQL_TYPE_TIME 或 SQL_TYPE_TIMESTAMP; 在 ODBC 2 中为。*返回 x*、SQL_DATE、SQL_TIME 或 SQL_TIMESTAMP。 当 ODBC 2 时，驱动程序管理器会执行所需的映射。*x* 应用程序正在使用 ODBC 3。*x* 驱动程序或 ODBC 3。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序。  
  
 如果 *ColumnNumber* 等于书签列) 的 (0，则将在 *\* DataTypePtr* 中为可变长度书签返回 SQL_BINARY。 如果 ODBC 3 使用书签，则返回 (SQL_INTEGER。使用 ODBC 2 的 *x* 应用程序。*x* 驱动程序或 ODBC 2。使用 ODBC 3 的 *x* 应用程序。*x* 驱动程序。 )   
  
 有关这些数据类型的详细信息，请参阅附录 D：数据类型中的 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) 。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。  
  
 *ColumnSizePtr*  
 输出指向缓冲区的指针，将在该缓冲区中返回数据源中列的大小 (以字符) 。 如果无法确定列的大小，则驱动程序将返回0。 有关列大小的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。  
  
 *DecimalDigitsPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回数据源中列的小数位数。 如果小数位数不能确定或不适用，则驱动程序将返回0。 有关十进制数字的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。  
  
 *NullablePtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回一个值，该值指示列是否允许 NULL 值。 此值从 IRD 的 "SQL_DESC_NULLABLE" 字段读取。 该值是下列值之一：  
  
 SQL_NO_NULLS：列不允许空值。  
  
 SQL_NULLABLE：列允许 NULL 值。  
  
 SQL_NULLABLE_UNKNOWN：驱动程序无法确定列是否允许 NULL 值。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLDescribeCol** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLDescribeCol** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *ColumnName* 不够大，无法返回整个列名称，因此列名被截断。 在 **NameLengthPtr* 中返回未截断列名称的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07005|预定义语句不是 *游标规范*|与 *StatementHandle* 关联的语句未返回结果集。 没有要描述的列。|  
|07009|描述符索引无效| (DM) 为参数 *ColumnNumber* 指定的值等于0，并 SQL_UB_OFF SQL_ATTR_USE_BOOKMARKS 语句选项。<br /><br /> 为参数 *ColumnNumber* 指定的值大于结果集中的列数。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配失败|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLDescribeCol** 时仍在执行此异步函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 在对语句句柄调用 **SQLPrepare**、 **SQLExecute** 或 Catalog 函数之前调用此函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *BufferLength* 指定的值小于0。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
 **SQLDescribeCol** 可以返回 **SQLPrepare** 或 **SQLExecute** 在 **SQLPrepare** 之后调用时可以返回的任何 **SQLSTATE，具体** 取决于数据源计算与语句句柄关联的 SQL 语句的时间。  
  
 出于性能方面的考虑，应用程序在执行语句之前不应调用 **SQLDescribeCol** 。  
  
## <a name="comments"></a>注释  
 应用程序通常会在调用 **SQLPrepare** 之后，或在对 **SQLExecute** 的关联调用之前或之后调用 **SQLDescribeCol** 。 应用程序还可以在调用 **SQLExecDirect** 后调用 **SQLDescribeCol** 。 有关详细信息，请参阅 [结果集元数据](../../../odbc/reference/develop-app/result-set-metadata.md)。  
  
 **SQLDescribeCol** 检索 **SELECT** 语句生成的列名称、类型和长度。 如果该列是表达式，则 **ColumnName* 为空字符串或驱动程序定义的名称。  
  
> [!NOTE]  
>  ODBC 支持 SQL_NULLABLE_UNKNOWN 作为扩展，即使开放组和 SQL 访问组调用级别接口规范未指定 **SQLDescribeCol** 的选项。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回有关结果集中的列的信息|[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|提取多行数据|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|返回结果集列的数目|[SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|准备要执行的语句|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
