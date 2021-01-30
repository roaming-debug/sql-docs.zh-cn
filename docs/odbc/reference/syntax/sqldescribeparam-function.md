---
description: SQLDescribeParam 函数
title: SQLDescribeParam 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeParam
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeParam
helpviewer_keywords:
- SQLDescribeParam function [ODBC]
ms.assetid: 1f5b63c4-2f3e-44da-b155-876405302281
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8459ca7dd493ba8ab316f832951170f87a77664
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192034"
---
# <a name="sqldescribeparam-function"></a>SQLDescribeParam 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLDescribeParam** 返回与已准备的 SQL 语句相关联的参数标记的说明。 此信息也可用于 IPD 的字段。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLDescribeParam(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT *   DataTypePtr,  
      SQLULEN *       ParameterSizePtr,  
      SQLSMALLINT *   DecimalDigitsPtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *ParameterNumber*  
 送参数标记编号按递增参数顺序排序，从1开始。  
  
 *DataTypePtr*  
 输出指向要在其中返回参数的 SQL 数据类型的缓冲区的指针。 此值从 IPD 的 SQL_DESC_CONCISE_TYPE 记录 "字段中读取。 这将是 "附录 D：数据类型" 的 " [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) " 部分或特定于驱动程序的 sql 数据类型的值之一。  
  
 ODBC 3 中的。在 *\* DataTypePtr* 中，将分别为日期、时间或时间戳数据返回 *x*、SQL_TYPE_DATE、SQL_TYPE_TIME 或 SQL_TYPE_TIMESTAMP; 在 ODBC 2 中为。将返回 x、SQL_DATE、SQL_TIME 或 SQL_TIMESTAMP。 当 ODBC 2 时，驱动程序管理器会执行所需的映射。*x* 应用程序正在使用 ODBC 3。*x* 驱动程序或 ODBC 3。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序。  
  
 如果 *ColumnNumber* 等于书签列) 的 (0，则将在 *\* DataTypePtr* 中为可变长度书签返回 SQL_BINARY。 如果 ODBC 3 使用书签，则返回 (SQL_INTEGER。使用 ODBC 2 的 *x* 应用程序。*x* 驱动程序或 ODBC 2。使用 ODBC 3 的 *x* 应用程序。*x* 驱动程序。 )   
  
 有关详细信息，请参阅附录 D：数据类型中的 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) 。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。  
  
 *ParameterSizePtr*  
 输出指向缓冲区的指针，该缓冲区用于返回数据源定义的相应参数标记的列或表达式的大小（以字符为字符）。 有关列大小的详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。  
  
 *DecimalDigitsPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回数据源定义的相应参数的列或表达式的小数位数。 有关十进制数字的详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。  
  
 *NullablePtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回一个值，该值指示参数是否允许 NULL 值。 此值从 IPD 的 SQL_DESC_NULLABLE 字段中读取。 下列类型作之一：  
  
-   SQL_NO_NULLS：该参数不允许 NULL 值 (这是) 的默认值。  
  
-   SQL_NULLABLE：参数允许空值。  
  
-   SQL_NULLABLE_UNKNOWN：驱动程序无法确定参数是否允许空值。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLDescribeParam** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLDescribeParam** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07009|描述符索引无效| (DM) 为参数 *ParameterNumber* 指定的值小于1。<br /><br /> 为参数 *ParameterNumber* 指定的值大于关联的 SQL 语句中参数的数目。<br /><br /> 参数标记是非 DML 语句的一部分。<br /><br /> 参数标记是 **选择** 列表的一部分。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|21S01|插入值列表与列列表不匹配|**INSERT** 语句中的参数数目与语句中指定的表中的列数不匹配。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 调用 *StatementHandle* 的 **SQLPrepare** 或 **SQLExecDirect** 之前调用了函数。<br /><br />  (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLDescribeParam** 函数时，此异步函数仍在执行。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 参数标记按递增参数顺序编号，从1开始，按照它们在 SQL 语句中出现的顺序进行编号。  
  
 **SQLDescribeParam** 不会返回 SQL 语句中参数 (输入、输入/输出或输出) 的类型。 在对过程的调用中，SQL 语句中的所有参数都是输入参数。 若要在对过程的调用中确定每个参数的类型，应用程序将调用 **SQLProcedureColumns**。  
  
 有关详细信息，请参阅 [描述参数](../../../odbc/reference/develop-app/describing-parameters.md)。  
  
## <a name="code-example"></a>代码示例  
 以下示例将提示用户输入 SQL 语句，然后准备该语句。 接下来，它调用 **SQLNumParams** 以确定语句是否包含任何参数。 如果该语句包含参数，则它会调用 **SQLDescribeParam** 来描述这些参数，并 **SQLBindParameter** 将其绑定。 最后，它会提示用户输入任何参数的值，然后执行该语句。  
  
```cpp  
SQLCHAR       Statement[100];  
SQLSMALLINT   NumParams, i, DataType, DecimalDigits, Nullable;  
SQLUINTEGER   ParamSize;  
SQLHSTMT      hstmt;  
  
// Prompt the user for an SQL statement and prepare it.  
GetSQLStatement(Statement);  
SQLPrepare(hstmt, Statement, SQL_NTS);  
  
// Check to see if there are any parameters. If so, process them.  
SQLNumParams(hstmt, &NumParams);  
if (NumParams) {  
   // Allocate memory for three arrays. The first holds pointers to buffers in which  
   // each parameter value will be stored in character form. The second contains the  
   // length of each buffer. The third contains the length/indicator value for each  
   // parameter.  
   SQLPOINTER * PtrArray = (SQLPOINTER *) malloc(NumParams * sizeof(SQLPOINTER));  
   SQLINTEGER * BufferLenArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
   SQLINTEGER * LenOrIndArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
  
   for (i = 0; i < NumParams; i++) {  
   // Describe the parameter.  
   SQLDescribeParam(hstmt, i + 1, &DataType, &ParamSize, &DecimalDigits, &Nullable);  
  
   // Call a helper function to allocate a buffer in which to store the parameter  
   // value in character form. The function determines the size of the buffer from  
   // the SQL data type and parameter size returned by SQLDescribeParam and returns  
   // a pointer to the buffer and the length of the buffer.  
   AllocParamBuffer(DataType, ParamSize, &PtrArray[i], &BufferLenArray[i]);  
  
   // Bind the memory to the parameter. Assume that we only have input parameters.  
   SQLBindParameter(hstmt, i + 1, SQL_PARAM_INPUT, SQL_C_CHAR, DataType, ParamSize,  
         DecimalDigits, PtrArray[i], BufferLenArray[i],  
         &LenOrIndArray[i]);  
  
   // Prompt the user for the value of the parameter and store it in the memory  
   // allocated earlier. For simplicity, this function does not check the value  
   // against the information returned by SQLDescribeParam. Instead, the driver does  
   // this when the statement is executed.  
   GetParamValue(PtrArray[i], BufferLenArray[i], &LenOrIndArray[i]);  
   }  
}  
  
// Execute the statement.  
SQLExecute(hstmt);  
  
// Process the statement further, such as retrieving results (if any) and closing the  
// cursor (if any). Code not shown.  
  
// Free the memory allocated for each parameter and the memory allocated for the arrays  
// of pointers, buffer lengths, and length/indicator values.  
for (i = 0; i < NumParams; i++) free(PtrArray[i]);  
free(PtrArray);  
free(BufferLenArray);  
free(LenOrIndArray);  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|准备要执行的语句|[SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
