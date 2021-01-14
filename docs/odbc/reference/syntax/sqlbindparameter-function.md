---
description: SQLBindParameter 函数
title: SQLBindParameter 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLBindParameter
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindParameter
helpviewer_keywords:
- SQLBindParameter function [ODBC]
ms.assetid: 38349d4b-be03-46f9-9d6a-e50dd144e225
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44b8b59722b790685b7fad47300039913d7294ae
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170149"
---
# <a name="sqlbindparameter-function"></a>SQLBindParameter 函数

**度**  
 引入的版本： ODBC 2.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLBindParameter** 将缓冲区绑定到 SQL 语句中的参数标记。 **SQLBindParameter** 支持绑定到 unicode C 数据类型，即使基础驱动程序不支持 unicode 数据。  
  
> [!NOTE]  
>  此函数取代了 ODBC 1.0 函数 **SQLSetParam**。 有关详细信息，请参阅 "注释"。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN SQLBindParameter(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT     InputOutputType,  
      SQLSMALLINT     ValueType,  
      SQLSMALLINT     ParameterType,  
      SQLULEN         ColumnSize,  
      SQLSMALLINT     DecimalDigits,  
      SQLPOINTER      ParameterValuePtr,  
      SQLLEN          BufferLength,  
      SQLLEN *        StrLen_or_IndPtr);  
```
  
## <a name="arguments"></a>参数

 *StatementHandle*  
 送语句句柄。  
  
 *ParameterNumber*  
 送参数编号，按递增参数顺序排序，从1开始。  
  
 InputOutputType   
 送参数的类型。 有关详细信息，请参阅 "备注" 中的 "*InputOutputType* 参数"。  
  
 *ValueType*  
 送参数的 C 数据类型。 有关详细信息，请参阅 "备注" 中的 "*ValueType* 参数"。  
  
 *ParameterType*  
 送参数的 SQL 数据类型。 有关详细信息，请参阅 "备注" 中的 "*ParameterType* 参数"。  
  
 ColumnSize   
 送相应参数标记的列或表达式的大小。 有关详细信息，请参阅 "备注" 中的 "*ColumnSize* 参数"。  
  
 如果你的应用程序将在64位 Windows 操作系统上运行，请参阅 [ODBC 64 位信息](../../../odbc/reference/odbc-64-bit-information.md)。  
  
 DecimalDigits   
 送相应参数标记的列或表达式的十进制数字。 有关列大小的详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。  
  
 *ParameterValuePtr*  
 [延迟输入]指向参数数据缓冲区的指针。 有关详细信息，请参阅 "备注" 中的 "*ParameterValuePtr* 参数"。  
  
 *BufferLength*  
 [输入/输出] *ParameterValuePtr* 缓冲区的长度（以字节为单位）。 有关详细信息，请参阅 "备注" 中的 "*BufferLength* 参数"。  
  
 如果你的应用程序将在64位操作系统上运行，请参阅 [ODBC 64 位信息](../../../odbc/reference/odbc-64-bit-information.md)。  
  
 *StrLen_or_IndPtr*  
 [延迟输入]指向参数长度的缓冲区的指针。 有关详细信息，请参阅 "备注" 中的 "*StrLen_or_IndPtr* 参数"。  
  
## <a name="returns"></a>返回

 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断

 当 **SQLBindParameter** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLBindParameter** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  

|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|*ValueType* 参数标识的数据类型无法转换为 *ParameterType* 参数标识的数据类型。 请注意，此错误可能由 **SQLExecDirect**、 **SQLExecute** 或 **SQLPutData** 在执行时返回，而不是由 **SQLBindParameter** 返回。|  
|07009|描述符索引无效| (DM) 为参数 *ParameterNumber* 指定的值小于1。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 **MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY003|应用程序缓冲区类型无效|参数 *ValueType* 指定的值不是有效的 C 数据类型或 SQL_C_DEFAULT。|  
|HY004|SQL 数据类型无效|为参数 *ParameterType* 指定的值既不是有效的 ODBC sql 数据类型标识符，也不是驱动程序支持的特定于驱动程序的 SQL 数据类型标识符。|  
|HY009|参数值无效| (DM) 参数 *ParameterValuePtr* 为 null 指针，则参数 *StrLen_or_IndPtr* 为 null 指针，并且未 SQL_PARAM_OUTPUT 参数 *InputOutputType* 。<br /><br />  (DM) SQL_PARAM_OUTPUT，其中参数 *ParameterValuePtr* 为 null 指针，C 类型为 char 或 Binary，BufferLength (*cbValueMax*) 大于0。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLBindParameter** 时仍在执行此异步函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY021|描述符信息不一致|在一致性检查过程中检查的描述符信息不一致。  (参阅 **SQLSetDescField** 中的 "一致性检查" 部分。 ) <br /><br /> 为参数 *DecimalDigits* 指定的值超出了由 *ParameterType* 参数指定的 SQL 数据类型列的数据源所支持的值的范围。|  
|HY090|字符串或缓冲区长度无效| (DM) *BufferLength* 中的值小于0。  (在 **SQLSetDescField** 中查看 SQL_DESC_DATA_PTR 字段的说明 ) |  
|HY104|无效的精度或小数位数值|为参数 *ColumnSize* 或 *DecimalDigits* 指定的值超出了由 *ParameterType* 参数指定的 SQL 数据类型列的数据源所支持的值的范围。|  
|HY105|无效的参数类型| (DM) 为参数 *InputOutputType* 指定的值无效。  (参阅 "注释"。) |  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持通过为参数 *ValueType* 指定的值和为参数 *ParameterType* 指定的驱动程序特定值的组合指定的转换。<br /><br /> 为参数 *ParameterType* 指定的值为驱动程序支持的 odbc 版本的有效 odbc SQL 数据类型标识符，但驱动程序或数据源不支持此标识符。<br /><br /> 驱动程序仅支持 ODBC 2。*x* 和参数 *ValueType* 为以下其中一项：<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> 以及附录 D：数据类型中 [c 数据类型](../../../odbc/reference/appendixes/c-data-types.md) 中列出的所有时间间隔 c 数据类型。<br /><br /> 驱动程序仅支持3.50 之前的 ODBC 版本，并 SQL_C_GUID 参数 *ValueType* 。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释

 应用程序调用 **SQLBindParameter** 来绑定 SQL 语句中的每个参数标记。 绑定始终有效，直到应用程序再次调用 **SQLBindParameter** ，将 **SQLFreeStmt** 与 SQL_RESET_PARAMS 选项一起调用，或调用 **SQLSetDescField** 将 APD 的 SQL_DESC_COUNT 标头字段设置为0。  
  
 有关参数的详细信息，请参阅 [语句参数](../../../odbc/reference/develop-app/statement-parameters.md)。 有关参数数据类型和参数标记的详细信息，请参阅附录 C： SQL 语法中的 [参数数据类型](../../../odbc/reference/appendixes/parameter-data-types.md) 和 [参数标记](../../../odbc/reference/appendixes/parameter-markers.md) 。  
  
## <a name="parameternumber-argument"></a>ParameterNumber 参数  
 如果对 **SQLBindParameter** 的调用中的 *ParameterNumber* 大于 SQL_DESC_COUNT 的值，则调用 **SQLSetDescField** 以将 SQL_DESC_COUNT 的值增加到 *ParameterNumber*。  
  
## <a name="inputoutputtype-argument"></a>InputOutputType 参数  
 *InputOutputType* 参数指定参数的类型。 此参数设置 IPD 的 SQL_DESC_PARAMETER_TYPE 字段。 不调用过程的 SQL 语句中的所有参数（如 **INSERT** 语句）都是 *输入**参数*。 过程调用中的参数可以是输入、输入/输出或输出参数。  (应用程序调用 **SQLProcedureColumns** 来确定过程调用中参数的类型;不能确定其类型的参数被认为是输入参数。 )   
  
 InputOutputType 参数为以下其中一个值  ：  
  
-   SQL_PARAM_INPUT。 参数在不调用过程的 SQL 语句（如 **INSERT** 语句）中标记参数，或在过程中标记输入参数。 例如， **插入到员工值中的参数 (?,?,? )** 是输入参数，而 **{call AddEmp (?,?,? ) }** 中的参数可以为，但不一定是输入参数。  
  
     执行语句时，驱动程序将参数的数据发送到数据源;\* *ParameterValuePtr* 缓冲区必须包含有效的输入值，或者 **StrLen_or_IndPtr* 缓冲区必须包含 SQL_LEN_DATA_AT_EXEC 宏 SQL_NULL_DATA、SQL_DATA_AT_EXEC 或结果。  
  
     如果应用程序不能在过程调用中确定参数的类型，则会将 *InputOutputType* 设置为 SQL_PARAM_INPUT;如果数据源返回参数的值，则驱动程序会将其丢弃。  
  
-   SQL_PARAM_INPUT_OUTPUT。 参数标记过程中的输入/输出参数。 例如， **{Call GetEmpDept (？ ) }** 中的参数是一个输入/输出参数，该参数接受雇员的姓名，并返回该雇员所在部门的姓名。  
  
     执行语句时，驱动程序将参数的数据发送到数据源;\* *ParameterValuePtr* 缓冲区必须包含有效的输入值，或者 \* *StrLen_or_IndPtr* 缓冲区必须包含 SQL_LEN_DATA_AT_EXEC 宏的 SQL_NULL_DATA、SQL_DATA_AT_EXEC 或结果。 执行语句之后，驱动程序将参数的数据返回到应用程序;如果数据源不返回输入/输出参数的值，则驱动程序将 **StrLen_or_IndPtr* 缓冲区设置为 SQL_NULL_DATA。  
  
    > [!NOTE]  
    >  当 ODBC 1.0 应用程序在 ODBC 2.0 驱动程序中调用 **SQLSetParam** 时，驱动程序管理器会将其转换为对 **SQLBindParameter** 的调用，其中 *InputOutputType* 参数设置为 SQL_PARAM_INPUT_OUTPUT。  
  
-   SQL_PARAM_OUTPUT。 参数在过程中标记过程或 output 参数的返回值;在这两种情况下，它们称为 *output 参数*。 例如， **{？ = Call GetNextEmpID}** 中的参数是一个输出参数，该参数返回下一个雇员 ID。  
  
     执行语句之后，驱动程序将参数的数据返回到应用程序，除非 *ParameterValuePtr* 和 *StrLen_or_IndPtr* 参数都是 null 指针，在这种情况下，驱动程序会丢弃输出值。 如果数据源不返回 output 参数的值，则驱动程序将 **StrLen_or_IndPtr* 缓冲区设置为 SQL_NULL_DATA。  
  
-   SQL_PARAM_INPUT_OUTPUT_STREAM。 指示应对输入/输出参数进行流式处理。 **SQLGetData** 可以读取部分中的参数值。 由于将在调用 **SQLGetData** 时确定缓冲区长度，因此忽略 *BufferLength* 。 *StrLen_or_IndPtr* 缓冲区的值必须包含 SQL_NULL_DATA、SQL_DEFAULT_PARAM、SQL_DATA_AT_EXEC 或 SQL_LEN_DATA_AT_EXEC 宏的结果。 如果参数将在输出中进行流式处理，则必须将参数作为执行时数据 (DAE) 参数绑定为输入。 *ParameterValuePtr* 可以是将由 **SQLParamData** 返回的任何非 null 指针值作为用户定义的标记，其值与 *ParameterValuePtr* 一起传递给输入和输出。 有关详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
-   SQL_PARAM_OUTPUT_STREAM。 对于 OUTPUT 参数，与 SQL_PARAM_INPUT_OUTPUT_STREAM 相同。 *输入时将忽略 *StrLen_or_IndPtr* 。  
  
 下表列出了 *InputOutputType* 和 **StrLen_or_IndPtr* 的不同组合：  
  
|InputOutputType |**StrLen_or_IndPtr*|业务成效|ParameterValuePtr 上的注释|  
|-----------------------|----------------------------|-------------|---------------------------------|  
|SQL_PARAM_INPUT|SQL_LEN_DATA_AT_EXEC (*长度*) 或 SQL_DATA_AT_EXEC|部分输入|*ParameterValuePtr* 可以是将由 **SQLParamData** 返回的任何指针值，它是其值与 *ParameterValuePtr* 一起传递的用户定义的标记。|  
|SQL_PARAM_INPUT|不 SQL_LEN_DATA_AT_EXEC (*长度*) 或 SQL_DATA_AT_EXEC|输入绑定缓冲区|*ParameterValuePtr* 是输入缓冲区的地址。|  
|SQL_PARAM_OUTPUT|在输入时忽略。|输出绑定缓冲区|*ParameterValuePtr* 是输出缓冲区的地址。|  
|SQL_PARAM_OUTPUT_STREAM|在输入时忽略。|流式处理输出|*ParameterValuePtr* 可以是任何指针值， **SQLParamData** 将其作为其值与 *ParameterValuePtr* 一起传递的用户定义标记返回。|  
|SQL_PARAM_INPUT_OUTPUT|SQL_LEN_DATA_AT_EXEC (*长度*) 或 SQL_DATA_AT_EXEC|部分输入和输出绑定缓冲区|*ParameterValuePtr* 是输出缓冲区的地址， **SQLParamData** 将返回其值与 *ParameterValuePtr* 一起传递的用户定义标记。|  
|SQL_PARAM_INPUT_OUTPUT|不 SQL_LEN_DATA_AT_EXEC (*长度*) 或 SQL_DATA_AT_EXEC|输入绑定缓冲区和输出绑定缓冲区|*ParameterValuePtr* 是共享的输入/输出缓冲区的地址。|
|SQL_PARAM_INPUT_OUTPUT_STREAM|SQL_LEN_DATA_AT_EXEC (*长度*) 或 SQL_DATA_AT_EXEC|部分输入和流式输出|*ParameterValuePtr* 可以是任何非 null 指针值， **SQLParamData** 将它作为用户定义的标记返回，其值与 *ParameterValuePtr* 一起传递输入和输出。|  
  
> [!NOTE]  
>  当应用程序将输出或输入输出参数绑定为流式处理时，驱动程序必须决定允许哪些 SQL 类型。 对于无效的 SQL 类型，驱动程序管理器不会生成错误。  
  
## <a name="valuetype-argument"></a>ValueType 参数

 *ValueType* 参数指定参数的 C 数据类型。 此参数设置 APD 的 SQL_DESC_TYPE、SQL_DESC_CONCISE_TYPE 和 SQL_DESC_DATETIME_INTERVAL_CODE 字段。 这必须是附录 D：数据类型的 [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md) 部分中的值之一。  
  
 如果 *ValueType* 参数是 interval 数据类型之一，则将 APD 的 *ParameterNumber* 记录的 "SQL_DESC_TYPE" 字段设置为 "SQL_INTERVAL，将 APD 的" SQL_DESC_CONCISE_TYPE "字段设置为" 简洁间隔 "数据类型，并将 *ParameterNumber* 记录的" SQL_DESC_DATETIME_INTERVAL_CODE "字段设置为特定" 时间间隔 "数据类型的子代码。  (参见 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。 ) 默认间隔前导精度 (2) 和默认间隔秒精度 (6) ，如在 APD 的 "SQL_DESC_DATETIME_INTERVAL_PRECISION" 和 "SQL_DESC_PRECISION" 字段中分别设置用于数据。 如果任何一个默认精度不合适，则应用程序应通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 显式设置描述符字段。  
  
 如果 *ValueType* 参数是日期时间数据类型之一，则将 APD 的 *ParameterNumber* 记录的 "SQL_DESC_TYPE" 字段设置为 "SQL_DATETIME，将 APD 的 *ParameterNumber* 记录的" SQL_DESC_CONCISE_TYPE "字段设置为简明日期时间 C 数据类型，将" *ParameterNumber* "记录的" SQL_DESC_DATETIME_INTERVAL_CODE "字段设置为特定 datetime 数据类型的子代码。  (参见 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。 )   
  
 如果 *ValueType* 参数是 SQL_C_NUMERIC 的数据类型，则用于数据的默认精度 (为驱动程序定义) ，默认小数位数 (0) 在 APD 的 SQL_DESC_PRECISION 和 SQL_DESC_SCALE 字段中设置。 如果默认的精度或小数位数不合适，应用程序应该通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 显式设置描述符字段。  
  
 SQL_C_DEFAULT 指定将从 *ParameterType* 指定的 SQL 数据类型的默认 C 数据类型传输参数值。  
  
 还可以指定扩展的 C 数据类型。 有关详细信息，请参阅 [ODBC 中的 C 数据类型](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)。  
  
 有关详细信息，请参阅附录 D：数据类型中的 [默认 c 数据类型](../../../odbc/reference/appendixes/default-c-data-types.md)、 [将数据从 C 转换为 sql 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)和 [将数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) 。  
  
## <a name="parametertype-argument"></a>ParameterType 参数

 此值必须是 "附录 D：数据类型" 的 " [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) " 部分中列出的值之一，或者必须是驱动程序特定的值。 此参数设置 IPD 的 SQL_DESC_TYPE、SQL_DESC_CONCISE_TYPE 和 SQL_DESC_DATETIME_INTERVAL_CODE 字段。  
  
 如果 *ParameterType* 参数是日期时间标识符之一，则 ipd 的 SQL_DESC_TYPE 字段设置为 SQL_DATETIME，ipd 的 SQL_DESC_CONCISE_TYPE 字段设置为简明日期时间 SQL 数据类型，SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为相应的 datetime 子代码值。  
  
 如果 *ParameterType* 是某个间隔标识符，则 ipd 的 "SQL_DESC_TYPE" 字段将设置为 SQL_INTERVAL，ipd 的 "SQL_DESC_CONCISE_TYPE" 字段设置为 "简洁 SQL interval" 数据类型，而 ipd 的 "SQL_DESC_DATETIME_INTERVAL_CODE" 字段设置为相应的间隔子代码。 IPD 的 "SQL_DESC_DATETIME_INTERVAL_PRECISION" 字段设置为间隔前导精度，并且 SQL_DESC_PRECISION 字段设置为间隔秒精度（如果适用）。 如果 SQL_DESC_DATETIME_INTERVAL_PRECISION 或 SQL_DESC_PRECISION 的默认值不合适，则应用程序应通过调用 **SQLSetDescField** 进行显式设置。 有关这些字段的详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)。  
  
 如果 *ValueType* 参数是 SQL_NUMERIC 的数据类型，则用于数据的默认精度 (为驱动程序定义) ，默认小数位数 (0) （在 IPD 的 SQL_DESC_PRECISION 和 SQL_DESC_SCALE 字段中设置）。 如果默认的精度或小数位数不合适，应用程序应该通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 显式设置描述符字段。  
  
 有关如何转换数据的信息，请参阅附录 D：数据类型中的将 [数据从 C 转换为 Sql 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) 和 [将数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) 。  
  
## <a name="columnsize-argument"></a>ColumnSize 参数

 *ColumnSize* 参数指定对应于参数标记的列或表达式的大小、该数据的长度或两者。 此参数设置 IPD 的不同字段，具体取决于 *ParameterType* 参数) 的 SQL 数据类型 (。 以下规则适用于此映射：  
  
-   如果 *ParameterType* 是 SQL_CHAR、SQL_VARCHAR、SQL_LONGVARCHAR、SQL_BINARY、SQL_VARBINARY、SQL_LONGVARBINARY 或简洁的 SQL 日期时间或间隔数据类型之一，则 IPD 的 SQL_DESC_LENGTH 字段将设置为 *ColumnSize* 的值。  (有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 部分。 )   
  
-   如果 *ParameterType* 是 SQL_DECIMAL、SQL_NUMERIC、SQL_FLOAT、SQL_REAL 或 SQL_DOUBLE，则 IPD 的 SQL_DESC_PRECISION 字段将设置为 *ColumnSize* 的值。  
  
-   对于其他数据类型，将忽略 *ColumnSize* 参数。  
  
 有关详细信息，请参阅 "*StrLen_or_IndPtr* 参数" 中的 "传递参数值" 和 SQL_DATA_AT_EXEC。  
  
## <a name="decimaldigits-argument"></a>DecimalDigits 参数

 如果 *ParameterType* SQL_TYPE_TIME，SQL_TYPE_TIMESTAMP，SQL_INTERVAL_SECOND，SQL_INTERVAL_DAY_TO_SECOND，SQL_INTERVAL_HOUR_TO_SECOND 或 SQL_INTERVAL_MINUTE_TO_SECOND，则 IPD 的 SQL_DESC_PRECISION 字段将设置为 *DecimalDigits*。 如果 *ParameterType* 是 SQL_NUMERIC 或 SQL_DECIMAL，则 IPD 的 SQL_DESC_SCALE 字段将设置为 *DecimalDigits*。 对于所有其他数据类型，将忽略 *DecimalDigits* 参数。  
  
## <a name="parametervalueptr-argument"></a>ParameterValuePtr 参数

 *ParameterValuePtr* 参数指向一个缓冲区，当调用 **SQLExecute** 或 **SQLExecDirect** 时，该缓冲区包含参数的实际数据。 数据必须采用 *ValueType* 参数所指定的格式。 此参数设置 APD 的 SQL_DESC_DATA_PTR 字段。 只要 *\* StrLen_or_IndPtr* SQL_NULL_DATA 或 SQL_DATA_AT_EXEC，应用程序就可以将 *ParameterValuePtr* 参数设置为 null 指针。  (这仅适用于输入参数或输入/输出参数。 )   
  
 如果 \* *StrLen_or_IndPtr* 是 SQL_LEN_DATA_AT_EXEC (*长度*) 宏或 SQL_DATA_AT_EXEC 的结果，则 *ParameterValuePtr* 是与参数关联的应用程序定义的指针值。 它通过 **SQLParamData** 返回到应用程序。 例如， *ParameterValuePtr* 可能是非零标记，如参数号、指向数据的指针或指向应用程序用于绑定输入参数的结构的指针。 但请注意，如果参数是一个输入/输出参数，则 *ParameterValuePtr* 必须是指向缓冲区的指针，将在其中存储输出值。 如果 SQL_ATTR_PARAMSET_SIZE 语句特性中的值大于1，则应用程序可将 SQL_ATTR_PARAMS_PROCESSED_PTR 语句特性指向的值与 *ParameterValuePtr* 参数一起使用。 例如， *ParameterValuePtr* 可能指向值的数组，并且应用程序可能使用指向的值 SQL_ATTR_PARAMS_PROCESSED_PTR 从数组中检索正确的值。 有关详细信息，请参阅本节后面的 "传递参数值"。  
  
 如果 *InputOutputType* 参数 SQL_PARAM_INPUT_OUTPUT 或 SQL_PARAM_OUTPUT，则 *ParameterValuePtr* 将指向驱动程序返回输出值的缓冲区。 如果过程返回一个或多个结果集，则在 \* 处理完所有结果集/行计数之前，不能保证 *ParameterValuePtr* 缓冲区的设置。 如果在处理完成之前未设置缓冲区，则在 **SQLMoreResults** 返回 SQL_NO_DATA 之前，输出参数和返回值将不可用。 使用 SQL_CLOSE 的选项调用 **SQLCloseCursor** 或 **SQLFreeStmt** 将导致这些值被丢弃。  
  
 如果 SQL_ATTR_PARAMSET_SIZE 语句特性中的值大于1，则 *ParameterValuePtr* 指向数组。 单个 SQL 语句处理输入或输入/输出参数的完整输入值数组，并返回输入/输出或输出参数的输出值的数组。  
  
## <a name="bufferlength-argument"></a>BufferLength 参数

 对于字符和二进制 C 数据， *BufferLength* 参数指定 \* *ParameterValuePtr* 缓冲区的长度 (如果它是单个元素) 或 ParameterValuePtr 数组中的元素的长度 \*  (如果 SQL_ATTR_PARAMSET_SIZE 语句特性中的值大于 1) ，则为。 此参数设置 APD 的 "SQL_DESC_OCTET_LENGTH 记录" 字段。 如果应用程序指定了多个值，则 *BufferLength* 用于确定 **ParameterValuePtr* 数组中的值的位置，这些值在输入和输出上都是如此。 对于输入/输出和输出参数，它用于确定是否在输出时截断字符和二进制 C 数据：  
  
-   对于字符 C 数据，如果可返回的字节数大于或等于 *BufferLength*，则 ParameterValuePtr 中的数据将被 \* 截断为 *BufferLength* 小于 null 终止字符的长度，并以 null 值终止于驱动程序。  
  
-   对于二进制 C 数据，如果可返回的字节数大于 *BufferLength*，则 ParameterValuePtr 中的数据将 \* 被截断为 *BufferLength* 字节。  
  
 对于所有其他类型的 C 数据，将忽略 *BufferLength* 参数。 \* *ParameterValuePtr* 缓冲区的长度 (如果它是单个元素) 或 ParameterValuePtr (数组中元素的长度，则为; \* 如果应用程序调用 **SQLSetStmtAttr** ，*属性* 参数为 SQL_ATTR_PARAMSET_SIZE 以指定每个参数的多个值) 假定为 C 数据类型的长度。  
  
 对于流式处理的输出或流式输入/输出参数， *BufferLength* 参数将被忽略，因为缓冲区长度是在 **SQLGetData** 中指定的。  
  
> [!NOTE]  
>  当 ODBC 1.0 应用程序调用 ODBC 3 中的 **SQLSetParam** 时。*x* 驱动程序，驱动程序管理器将此转换为对 **SQLBindParameter** 的调用，其中 *BufferLength* 参数始终 SQL_SETPARAM_VALUE_MAX。 因为如果 ODBC 3，驱动程序管理器会返回错误。*x* 应用程序将 *BufferLength* 设置为 SQL_SETPARAM_VALUE_MAX，这是一个 ODBC 3。*x* 驱动程序可以使用它来确定 ODBC 1.0 应用程序调用它的时间。  
  
> [!NOTE]  
>  在 **SQLSetParam** 中，应用程序指定 **ParameterValuePtr* 缓冲区长度的方式，以便驱动程序可以返回字符或二进制数据，以及应用程序将字符或二进制参数值数组发送到驱动程序的方式由驱动程序定义。  
  
## <a name="strlen_or_indptr-argument"></a>StrLen_or_IndPtr 参数

 *StrLen_or_IndPtr* 参数指向一个缓冲区，当调用 **SQLExecute** 或 **SQLExecDirect** 时，它包含以下内容之一。  (此参数设置应用程序参数指针的 SQL_DESC_OCTET_LENGTH_PTR 和 SQL_DESC_INDICATOR_PTR 记录字段。 )   
  
-   **ParameterValuePtr* 中存储的参数值的长度。 这会被忽略，但字符或二进制 C 数据除外。  
  
-   SQL_NTS。 参数值为以 null 值结束的字符串。  
  
-   SQL_NULL_DATA。 参数值为 NULL。  
  
-   SQL_DEFAULT_PARAM。 过程是使用参数的默认值，而不是从应用程序中检索到的值。 此值仅在 ODBC 规范语法中调用的过程中有效，并且仅当 *InputOutputType* 参数 SQL_PARAM_INPUT、SQL_PARAM_INPUT_OUTPUT 或 SQL_PARAM_INPUT_OUTPUT_STREAM 时才有效。 SQL_DEFAULT_PARAM \* *StrLen_or_IndPtr* 时，将忽略 *ValueType*、 *ParameterType*、 *ColumnSize*、 *DecimalDigits*、 *BufferLength* 和 *ParameterValuePtr* 参数的输入参数，这些参数仅用于定义输入/输出参数的输出参数值。  
  
-   SQL_LEN_DATA_AT_EXEC (*长度*) 宏的结果。 参数的数据将与 **SQLPutData** 一起发送。 如果 *ParameterType* 参数 SQL_LONGVARBINARY、SQL_LONGVARCHAR 或特定于数据源的数据类型，并且驱动程序为 **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型返回 "Y"，则 *length* 是要为参数发送的数据的字节数;否则， *长度* 必须为非负值并且将被忽略。 有关详细信息，请参阅本节后面的 "传递参数值"。  
  
     例如，若要指定在一个或多个调用中使用 **SQLPutData** 发送10000字节的数据，请为 SQL_LONGVARCHAR 参数指定一个应用程序集 **StrLen_or_IndPtr* 以 SQL_LEN_DATA_AT_EXEC (10000) 。  
  
-   SQL_DATA_AT_EXEC。 参数的数据将与 **SQLPutData** 一起发送。 此值由 ODBC 1.0 应用程序在调用 ODBC 3 时使用。*x* 驱动程序。 有关详细信息，请参阅本节后面的 "传递参数值"。  
  
 如果 *StrLen_or_IndPtr* 为 null 指针，则驱动程序会假定所有输入参数值均为非 null，并且字符和二进制数据以 null 结尾。 如果 *InputOutputType* 是 SQL_PARAM_OUTPUT 或 SQL_PARAM_OUTPUT_STREAM 并且 *ParameterValuePtr* 和 *StrLen_or_IndPtr* 都是 null 指针，则驱动程序将丢弃输出值。  
  
> [!NOTE]  
>  当 SQL_C_BINARY 参数的数据类型时，强烈建议应用程序开发人员为 *StrLen_or_IndPtr* 指定 null 指针。 若要确保驱动程序不会意外截断 SQL_C_BINARY 的数据， *StrLen_or_IndPtr* 应包含指向有效长度值的指针。  
  
 如果 *InputOutputType* 参数 SQL_PARAM_INPUT_OUTPUT、SQL_PARAM_OUTPUT、SQL_PARAM_INPUT_OUTPUT_STREAM 或 SQL_PARAM_OUTPUT_STREAM， *StrLen_or_IndPtr* 指向驱动程序返回的缓冲区 SQL_NULL_DATA，则可在 ParameterValuePtr 中返回的字节数 (\* 排除字符数据) 的 NULL 终止字节，如果无法确定可返回的字节数，则 SQL_NO_TOTAL (。 如果过程返回一个或多个结果集，则在提取所有结果之前，不能保证 **StrLen_or_IndPtr* 缓冲区的设置。  
  
 如果 SQL_ATTR_PARAMSET_SIZE 语句特性中的值大于1，则 *StrLen_or_IndPtr* 指向 SQLLEN 值的数组。 这些值可以是本部分前面列出的任何值，并使用单个 SQL 语句进行处理。  
  
## <a name="passing-parameter-values"></a>传递参数值

 应用程序可以在 \* *ParameterValuePtr* 缓冲区中或者通过一个或多个对 **SQLPutData** 的调用传递参数的值。 其数据与 **SQLPutData** 一起传递的参数称为 *执行时数据* 参数。 这些参数通常用于发送 SQL_LONGVARBINARY 和 SQL_LONGVARCHAR 参数的数据，并且可以与其他参数混合使用。  
  
 若要传递参数值，应用程序需要执行以下一系列步骤：  
  
1.  为每个参数调用 **SQLBindParameter** ，以便绑定参数值 (*ParameterValuePtr* 参数的缓冲区) 和长度/指示器 (*StrLen_or_IndPtr* 参数) 。 对于执行时数据参数， *ParameterValuePtr* 是应用程序定义的指针值，如参数号或指向数据的指针。 稍后将返回值，并可将其用于标识参数。  
  
2.  将输入和输入/输出参数的值放置在 \* *ParameterValuePtr* 和 **StrLen_or_IndPtr* 缓冲区中：  
  
    -   对于普通参数，应用程序会将参数值放在 \* *ParameterValuePtr* 缓冲区中，并将该值的长度置于 **StrLen_or_IndPtr* 缓冲区中。 有关详细信息，请参阅 [设置参数值](../../../odbc/reference/develop-app/setting-parameter-values.md)。  
  
    -   对于执行时数据参数，应用程序会在调用 ODBC 2.0 驱动程序时，将 SQL_LEN_DATA_AT_EXEC (*长度* 的结果) 宏 (，*) 缓冲区。*  
  
3.  调用 **SQLExecute** 或 **SQLExecDirect** 执行 SQL 语句。  
  
    -   如果没有执行时数据参数，则该过程已完成。  
  
    -   如果有任何执行时数据参数，则函数将返回 SQL_NEED_DATA。  
  
4.  调用 **SQLParamData** ，以检索在 **SQLBindParameter** 的 *ParameterValuePtr* 参数中指定的应用程序定义的值，以便处理第一个执行时数据参数。 **SQLParamData** 返回 SQL_NEED_DATA。  
  
    > [!NOTE]  
    >  尽管执行时数据参数与执行时数据列类似，但 **SQLParamData** 返回的值对于每个都是不同的。 执行时数据参数是 SQL 语句中的参数，当使用 **SQLExecDirect** 或 **SQLExecute** 执行语句时，该语句中的数据将与 **SQLPutData** 一起发送。 它们与 **SQLBindParameter** 绑定在一起。 **SQLParamData** 返回的值是在 *ParameterValuePtr* 参数中传递给 **SQLBindParameter** 的指针值。 执行时数据列是行集中的列，当使用 **SQLBulkOperations** 更新或添加行或使用 **SQLSetPos** 更新行时，数据将随 **SQLPutData** 一起发送。 它们与 **SQLBindCol** 绑定在一起。 **SQLParamData** 返回的值是 **TargetValuePtr* 缓冲区中的行的地址 (通过调用正在处理的 **SQLBindCol**) 设置。  
  
5.  调用 **SQLPutData** 一次或多次以发送参数的数据。 如果数据值大于在 SQLPutData 中指定的 ParameterValuePtr 缓冲区，则需要多个调用 \* ; 仅当将字符 c 数据发送到具有字符、二进制或数据源特定数据类型的列或将二进制 c 数据发送到具有字符、二进制或数据源特定数据类型的列时，才允许使用多个调用。  
  
6.  再次调用 **SQLParamData** ，以指示已发送参数的所有数据。  
  
    -   如果有多个执行时数据参数，则 **SQLParamData** 将返回 SQL_NEED_DATA 和应用程序定义的值，以便处理下一个执行时数据参数。 应用程序重复步骤4和5。  
  
    -   如果没有其他执行时数据参数，则该过程已完成。 如果语句已成功执行，则 **SQLParamData** 将返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO;如果执行失败，则它将返回 SQL_ERROR。 此时， **SQLParamData** 可以返回可由用于执行语句 (**SQLExecDirect** 或 **SQLExecute**) 的函数返回的任何 SQLSTATE。  
  
         在 \* 应用程序检索完语句生成的所有结果集后， *ParameterValuePtr* 和 **StrLen_or_IndPtr* 缓冲区中会提供任何输入/输出或输出参数的输出值。  
  
 调用 **SQLExecute** 或 **SQLExecDirect** 会将语句置于 SQL_NEED_DATA 状态。 此时，应用程序只能调用带有语句的 **SQLCancel**、 **SQLGetDiagField**、 **SQLGetDiagRec**、 **SQLGetFunctions**、 **SQLParamData** 或 **SQLPutData** 以及与该语句关联的 *连接句柄* 。 如果它调用具有语句的任何其他函数或与该语句关联的连接，则函数返回 SQLSTATE HY010 (函数序列错误) 。 当 **SQLParamData** 或 **SQLPutData** 返回错误时，语句将离开 SQL_NEED_DATA 状态， **SQLParamData** 将返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，否则语句将被取消。  
  
 如果应用程序调用 **SQLCancel** ，但驱动程序仍需要执行执行时数据参数的数据，则驱动程序将取消语句执行;然后，应用程序可以再次调用 **SQLExecute** 或 **SQLExecDirect** 。  
  
## <a name="retrieving-streamed-output-parameters"></a>检索流式处理输出参数

 当应用程序将 *InputOutputType* 设置为 SQL_PARAM_INPUT_OUTPUT_STREAM 或 SQL_PARAM_OUTPUT_STREAM 时，必须通过对 **SQLGetData** 的一个或多个调用来检索输出参数值。 当驱动程序具有要返回到应用程序的流式输出参数值时，它将返回 SQL_PARAM_DATA_AVAILABLE 来响应对以下函数的调用： **SQLMoreResults**、 **SQLExecute** 和 **SQLExecDirect**。 应用程序调用 **SQLParamData** 来确定可用的参数值。  
  
 有关 SQL_PARAM_DATA_AVAILABLE 和流式输出参数的详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
## <a name="using-arrays-of-parameters"></a>使用参数的数组

 当应用程序使用参数标记准备语句并传入参数数组时，可以执行两种不同的方式。 一种方法是让驱动程序依赖后端的阵列处理功能，在这种情况下，具有参数数组的整个语句将被视为一个原子单元。 Oracle 是支持数组处理功能的数据源的一个示例。 实现此功能的另一种方法是，驱动程序生成一批 SQL 语句，一个 SQL 语句用于参数数组中的每个参数集，然后执行批处理。 参数数组不能与当前语句的 **更新** 一起使用。  
  
 当处理参数数组时，每个结果集/行计数 (每个参数集) 可用，或者可以将结果集/行计数汇总到其中。 **SQLGetInfo** 中的 SQL_PARAM_ARRAY_ROW_COUNTS 选项指示行计数是否可用于 () SQL_PARC_BATCH 的每个参数集，或只有一个行计数可用于 (SQL_PARC_NO_BATCH) 。  
  
 **SQLGetInfo** 中的 SQL_PARAM_ARRAY_SELECTS 选项指示一个结果集是否可用于 () SQL_PAS_BATCH 的每个参数集，或仅有一个结果集可用于 (SQL_PAS_NO_BATCH) 。 如果驱动程序不允许使用一组参数执行结果集生成语句，SQL_PARAM_ARRAY_SELECTS 将返回 SQL_PAS_NO_SELECT。  
  
 有关详细信息，请参阅 [SQLGetInfo 函数](../../../odbc/reference/syntax/sqlgetinfo-function.md)。  
  
 为了支持参数数组，将 SQL_ATTR_PARAMSET_SIZE 语句特性设置为指定每个参数的值的数目。 如果字段大于1，则 APD 的 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 字段必须指向数组。 每个数组的基数等于 SQL_ATTR_PARAMSET_SIZE 的值。  
  
 APD 的 "SQL_DESC_ROWS_PROCESSED_PTR" 字段指向一个缓冲区，该缓冲区包含已处理的参数集的数目，包括错误集。 处理每组参数时，驱动程序会在缓冲区中存储新值。 如果为 null 指针，则不返回任何数字。 使用参数数组时，即使由 setting 函数返回 SQL_ERROR，APD 的 SQL_DESC_ROWS_PROCESSED_PTR 字段所指向的值也将被填充。 如果返回 SQL_NEED_DATA，则 APD 的 SQL_DESC_ROWS_PROCESSED_PTR 字段所指向的值将设置为正在处理的参数集。  
  
 绑定参数数组和执行当前语句的 **更新** 时，会发生什么情况。  
  
## <a name="column-wise-parameter-binding"></a>Column-Wise 参数绑定

 在按列绑定中，应用程序将单独的参数和长度/指示器数组绑定到每个参数。  
  
 若要使用按列绑定，应用程序首先将 SQL_ATTR_PARAM_BIND_TYPE 语句特性设置为 SQL_PARAM_BIND_BY_COLUMN。  (这是默认值。 ) 对于每个要绑定的列，应用程序执行以下步骤：  
  
1.  分配参数缓冲区数组。  
  
2.  分配长度/指示器缓冲区的数组。  
  
    > [!NOTE]  
    >  当使用按列绑定时，如果应用程序直接写入描述符，则可以使用单独的数组作为长度和指示器数据。  
  
3.  调用具有以下参数的 **SQLBindParameter** ：  
  
    -   *ValueType* 是参数缓冲区数组中单个元素的 C 类型。  
  
    -   *ParameterType* 是参数的 SQL 类型。  
  
    -   *ParameterValuePtr* 是参数缓冲区数组的地址。  
  
    -   *BufferLength* 是参数缓冲区数组中单个元素的大小。 当数据为固定长度数据时，将忽略 *BufferLength* 参数。  
  
    -   *StrLen_or_IndPtr* 是长度/指示器数组的地址。  
  
 有关如何使用此信息的详细信息，请参阅本节后面的 "注释" 中的 "ParameterValuePtr 参数"。 有关参数的列绑定的详细信息，请参阅 [绑定参数数组](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md)。  
  
## <a name="row-wise-parameter-binding"></a>Row-Wise 参数绑定

 在按行绑定中，应用程序将定义一个结构，该结构包含每个要绑定的参数的参数和长度/指示器缓冲区。  
  
 若要使用按行绑定，应用程序需要执行以下步骤：  
  
1.  定义一个结构来保存一组参数 (包括参数和长度/指示器缓冲区) 并分配这些结构的数组。  
  
    > [!NOTE]  
    >  当使用按行绑定时，如果应用程序直接写入描述符，则可以将单独的字段用于长度和指示器数据。  
  
2.  将 SQL_ATTR_PARAM_BIND_TYPE 语句特性设置为结构的大小，该结构包含一组参数，或参数将绑定到的缓冲区的实例大小。 长度必须包含所有绑定参数的空间以及结构或缓冲区的任何填充，以确保当绑定参数的地址递增指定的长度时，结果将指向下一行中的相同参数的开头。 在 ANSI C 中使用 *sizeof* 运算符时，此行为是保证的。  
  
3.  对要绑定的每个参数调用 **SQLBindParameter** ，并提供以下参数：  
  
    -   *ValueType* 是要绑定到列的参数缓冲区成员的类型。  
  
    -   *ParameterType* 是参数的 SQL 类型。  
  
    -   *ParameterValuePtr* 是第一个数组元素中的参数缓冲区成员的地址。  
  
    -   *BufferLength* 是参数缓冲区成员的大小。  
  
    -   *StrLen_or_IndPtr* 是要绑定的长度/指示器成员的地址。  
  
 有关如何使用此信息的详细信息，请参阅本节后面的 "*ParameterValuePtr* 参数"。 有关参数的按行绑定的详细信息，请参阅 [参数的绑定数组](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md)。  
  
## <a name="error-information"></a>错误信息

 如果驱动程序未将参数数组作为批处理实现 (SQL_PARAM_ARRAY_ROW_COUNTS 选项等于 SQL_PARC_NO_BATCH) ，则会像执行一个语句一样处理错误情况。 如果驱动程序将参数数组作为批处理实现，则应用程序可以使用 IPD 的 SQL_DESC_ARRAY_STATUS_PTR 标头字段来确定 SQL 语句的参数或参数数组中的哪个参数导致 **SQLExecDirect** 或 **SQLExecute** 返回错误。 此字段包含参数值的每一行的状态信息。 如果字段指示已发生错误，则诊断数据结构中的字段将指示失败的参数的行和参数号。 数组中的元素数将由 APD 中的 SQL_DESC_ARRAY_SIZE 标头字段定义，该字段可通过 SQL_ATTR_PARAMSET_SIZE 语句特性进行设置。  
  
> [!NOTE]  
>  APD 中的 SQL_DESC_ARRAY_STATUS_PTR 标头字段用于忽略参数。 有关忽略参数的详细信息，请参阅下一节 "忽略一组参数"。  
  
 当 **SQLExecute** 或 **SQLExecDirect** 返回 SQL_ERROR 时，IPD 中的 SQL_DESC_ARRAY_STATUS_PTR 字段所指向的数组中的元素将包含 SQL_PARAM_ERROR、SQL_PARAM_SUCCESS、SQL_PARAM_SUCCESS_WITH_INFO、SQL_PARAM_UNUSED 或 SQL_PARAM_DIAG_UNAVAILABLE。  
  
 对于此数组中的每个元素，诊断数据结构包含一个或多个状态记录。 结构的 "SQL_DIAG_ROW_NUMBER" 字段指示导致错误的参数值的行号。 如果可以确定导致错误的参数行中的特定参数，则将在 SQL_DIAG_COLUMN_NUMBER 字段中输入参数号。  
  
 当未使用参数时，将会输入 SQL_PARAM_UNUSED，因为在强制 **SQLExecute** 或 **SQLExecDirect** 中止的之前的参数中发生错误。 例如，如果存在50个参数，而在执行导致 **SQLExecute** 或 **SQLExecDirect** 的 fortieth 参数集时出现错误，则会在参数41到50的状态数组中输入 SQL_PARAM_UNUSED。  
  
 当驱动程序将参数数组视为单一单元时，将会输入 SQL_PARAM_DIAG_UNAVAILABLE，因此不会生成此单个参数级别的错误信息。  
  
 处理一组参数时的一些错误会导致在数组中处理后面的参数集以停止。 其他错误不会影响后续参数的处理。 将停止处理的错误由驱动程序定义。 如果未停止处理，则将处理数组中的所有参数，SQL_SUCCESS_WITH_INFO 将作为错误的结果返回，而 SQL_ATTR_PARAMS_PROCESSED_PTR 定义的缓冲区设置为 SQL_ATTR_PARAMSET_SIZE 语句特性) （其中包含错误集）定义的已处理 (参数集总数。  
  
> [!CAUTION]  
>  当处理参数数组时出现错误时的 ODBC 行为在 ODBC 3 中不同。*x* 与 ODBC 2 中的相同。*x*。 在 ODBC 2 中。*x*，函数返回 SQL_ERROR 和处理停止。 **SQLParamOptions** 的 *pirow* 参数指向的缓冲区包含错误行号。 ODBC 3 中的。*x*，函数返回 SQL_SUCCESS_WITH_INFO 并且处理可能会停止或继续。 如果继续，SQL_ATTR_PARAMS_PROCESSED_PTR 指定的缓冲区将被设置为处理的所有参数的值，包括导致错误的参数。 此行为更改可能会导致现有应用程序出现问题。  
  
 当 **SQLExecute** 或 **SQLExecDirect** 在完成处理参数数组中的所有参数集之前返回时（例如，当返回 SQL_ERROR 或 SQL_NEED_DATA 时），状态数组将包含已经处理的那些参数的状态。 IPD 中 SQL_DESC_ROWS_PROCESSED_PTR 字段指向的位置包含导致 SQL_ERROR 或 SQL_NEED_DATA 错误代码的参数数组中的行号。 将参数数组发送到 SELECT 语句时，状态数组值的可用性是由驱动程序定义的;它们可能在执行语句后或提取结果集后可用。  
  
## <a name="ignoring-a-set-of-parameters"></a>忽略一组参数

 SQL_ATTR_PARAM_STATUS_PTR 语句特性) 所设置的 APD (的 SQL_DESC_ARRAY_STATUS_PTR 字段可用于指示应忽略 SQL 语句中的一组绑定参数。 若要指示驱动程序在执行过程中忽略一个或多个参数集，应用程序应遵循以下步骤：  
  
1.  调用 **SQLSetDescField** ，将 APD 的 SQL_DESC_ARRAY_STATUS_PTR 标题字段设置为指向 SQLUSMALLINT 值的数组，使其包含状态信息。 还可以通过使用 SQL_ATTR_PARAM_OPERATION_PTR 的 *属性* 调用 **SQLSetStmtAttr** 来设置此字段，这允许应用程序在不获取描述符句柄的情况下设置字段。  
  
2.  将 APD 的 SQL_DESC_ARRAY_STATUS_PTR 字段定义的数组的每个元素设置为以下两个值之一：  
  
    -   SQL_PARAM_IGNORE，指示行已从语句执行中排除。  
  
    -   SQL_PARAM_PROCEED，指示该行包含在语句执行中。  
  
3.  调用 **SQLExecDirect** 或 **SQLExecute** 以执行预定义语句。  
  
 以下规则适用于 APD 的 "SQL_DESC_ARRAY_STATUS_PTR" 字段定义的数组：  
  
-   默认情况下，指针设置为 null。  
  
-   如果指针为 null，则将使用所有参数集，就像所有元素都设置为 SQL_ROW_PROCEED。  
  
-   将元素设置为 SQL_PARAM_PROCEED 不保证操作将使用该特定参数集。  
  
-   SQL_PARAM_PROCEED 在头文件中定义为0。  
  
 应用程序可以设置 APD 中的 "SQL_DESC_ARRAY_STATUS_PTR" 字段，使其指向 IRD 中 SQL_DESC_ARRAY_STATUS_PTR 字段所指向的相同数组。 当将参数绑定到行数据时，这很有用。 然后，可以根据行数据的状态忽略参数。 除了 SQL_PARAM_IGNORE 之外，以下代码还会导致忽略 SQL 语句中的参数： SQL_ROW_DELETED、SQL_ROW_UPDATED 和 SQL_ROW_ERROR。 除了 SQL_PARAM_PROCEED 之外，以下代码还会导致 SQL 语句继续： SQL_ROW_SUCCESS、SQL_ROW_SUCCESS_WITH_INFO 和 SQL_ROW_ADDED。  
  
## <a name="rebinding-parameters"></a>重新绑定参数

 应用程序可以执行以下两个操作之一来更改绑定：  
  
-   调用 **SQLBindParameter** 为已绑定的列指定新绑定。 驱动程序用新的绑定覆盖旧的绑定。  
  
-   指定要添加到由对 **SQLBindParameter** 的绑定调用指定的缓冲区地址的偏移量。 有关详细信息，请参阅下一节 "用偏移量重新绑定"。  
  
## <a name="rebinding-with-offsets"></a>用偏移量重新绑定

 当应用程序具有可包含多个参数但调用 **SQLExecDirect** 或 **SQLExecute** 仅使用几个参数的缓冲区域设置时，参数的重新绑定特别有用。 缓冲区中的剩余空间可用于通过按偏移量修改现有绑定来使用下一组参数。  
  
 APD 中的 SQL_DESC_BIND_OFFSET_PTR 标头字段指向绑定偏移量。 如果该字段为非 null，则驱动程序将取消引用指针，如果 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 字段中没有任何值为 null 指针，则会在执行时将取消引用的值添加到描述符记录中的字段。 当执行 SQL 语句时，将使用新的指针值。 重新绑定后，偏移量仍有效。 由于 SQL_DESC_BIND_OFFSET_PTR 是指向偏移量的指针而不是偏移本身，因此，应用程序可以直接更改偏移量，而不必调用 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) 或 [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md) 来更改描述符字段。 默认情况下，指针设置为 null。 可以通过调用[SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) [或通过](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)fAttribute SQL_ATTR_PARAM_BIND_OFFSET_PTR 的来设置 ARD 的 "SQL_DESC_BIND_OFFSET_PTR" 字段。  
  
 绑定偏移量始终直接添加到 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 字段的值。 如果偏移量更改为不同的值，则新值仍会直接添加到每个描述符字段中的值。 新偏移量不会添加到字段值和任何以前的偏移量之和。  
  
## <a name="descriptors"></a>描述符

 如何绑定参数由 Apd 和 Ipd 的字段确定。 **SQLBindParameter** 中的参数用于设置这些描述符字段。 还可以通过 **SQLSetDescField** 函数设置字段，但 **SQLBindParameter** 更有效地使用，因为应用程序不必获得用于调用 **SQLBindParameter** 的描述符句柄。  
  
> [!CAUTION]  
>  对一个语句调用 **SQLBindParameter** 可能会影响其他语句。 如果与该语句关联的 ARD 已显式分配并与其他语句相关联，则会发生这种情况。 因为 **SQLBindParameter** 修改了 APD 的字段，所以修改将应用到与此描述符关联的所有语句。 如果这不是所需的行为，应用程序应在调用 **SQLBindParameter** 之前，将此描述符与其他语句取消关联。  
  
 从概念上讲， **SQLBindParameter** 按顺序执行以下步骤：  
  
1.  调用 [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) 以获取 APD 句柄。  
  
2.  调用 [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) 以获取 APD 的 SQL_DESC_COUNT 字段，如果 *ColumnNumber* 参数的值超过了 SQL_DESC_COUNT 的值，则调用 **SQLSetDescField** 将 SQL_DESC_COUNT 的值增加到 *ColumnNumber*。  
  
3.  多次调用 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) ，将值分配给 APD 的以下字段：  
  
    -   将 SQL_DESC_TYPE 和 SQL_DESC_CONCISE_TYPE 设置为 *ValueType* 的值，只不过如果 *ValueType* 是 datetime 或 interval 子类型的一个简洁标识符，则它会将 SQL_DESC_TYPE 分别设置为 SQL_DATETIME 或 SQL_INTERVAL，将 SQL_DESC_CONCISE_TYPE 设置为简明标识符，并将 SQL_DESC_DATETIME_INTERVAL_CODE 设置为相应的 datetime 或 interval 子代码。  
  
    -   将 SQL_DESC_OCTET_LENGTH 字段设置为 *BufferLength* 的值。  
  
    -   将 SQL_DESC_DATA_PTR 字段设置为 *ParameterValue* 的值。  
  
    -   将 SQL_DESC_OCTET_LENGTH_PTR 字段设置为 *StrLen_or_Ind* 的值。  
  
    -   将 SQL_DESC_INDICATOR_PTR 字段还设置为 *StrLen_or_Ind* 的值。  
  
     *StrLen_or_Ind* 参数同时指定指标信息和参数值的长度。  
  
4.  调用 [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) 以获取 IPD 句柄。  
  
5.  调用 [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) 获取 IPD 的 SQL_DESC_COUNT 字段，如果 *ColumnNumber* 参数的值超过了 SQL_DESC_COUNT 的值，则调用 **SQLSetDescField** 以将 SQL_DESC_COUNT 的值增加到 *ColumnNumber*。  
  
6.  多次调用 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) ，将值分配给 IPD 的以下字段：  
  
    -   将 SQL_DESC_TYPE 和 SQL_DESC_CONCISE_TYPE 设置为 *ParameterType* 的值，但如果 *ParameterType* 是 datetime 或 interval 子类型的一个简洁标识符，则它会将 SQL_DESC_TYPE 分别设置为 SQL_DATETIME 或 SQL_INTERVAL，将 SQL_DESC_CONCISE_TYPE 设置为简明标识符，并将 SQL_DESC_DATETIME_INTERVAL_CODE 设置为相应的 datetime 或 interval 子代码。  
  
    -   根据 *ParameterType* 的需要，设置一个或多个 SQL_DESC_LENGTH、SQL_DESC_PRECISION 和 SQL_DESC_DATETIME_INTERVAL_PRECISION。  
  
    -   将 SQL_DESC_SCALE 设置为 *DecimalDigits* 的值。  
  
 如果对 **SQLBindParameter** 的调用失败，则它将在 APD 中设置的描述符字段的内容是未定义的，且 APD 的 "SQL_DESC_COUNT" 字段不变。 此外，IPD 中相应记录的 "SQL_DESC_LENGTH"、"SQL_DESC_PRECISION"、"SQL_DESC_SCALE" 和 "SQL_DESC_TYPE" 字段未定义，并且 IPD 的 "SQL_DESC_COUNT" 字段保持不变。  
  
## <a name="conversion-of-calls-to-and-from-sqlsetparam"></a>与 SQLSetParam 的调用之间的转换

 当 ODBC 1.0 应用程序调用 ODBC 3 中的 **SQLSetParam** 时。*x* 驱动程序，ODBC 3。*x* 驱动程序管理器按下表所示映射调用。  
  
|由 ODBC 1.0 应用程序调用|调用 ODBC 3。*x* 驱动程序|  
|----------------------------------|-------------------------------|  
|SQLSetParam ( StatementHandle、ParameterNumber、ValueType、ParameterType、LengthPrecision、ParameterScale、ParameterValuePtr、StrLen_or_IndPtr) ;|SQLBindParameter ( StatementHandle，ParameterNumber，SQL_PARAM_INPUT_OUTPUT，ValueType，ParameterType，      *ColumnSize*，      *DecimalDigits*，ParameterValuePtr，SQL_SETPARAM_VALUE_MAX，StrLen_or_IndPtr) ;|  
  
## <a name="examples"></a>示例  

### <a name="a-use-sqlbindparameter-function"></a>A. 使用 SQLBindParameter 函数

 在下面的示例中，应用程序准备 SQL 语句，以便将数据插入 ORDERS 表。 对于语句中的每个参数，应用程序会调用 **SQLBindParameter** 来指定 ODBC C 数据类型和参数的 SQL 数据类型，并将缓冲区绑定到每个参数。 对于每个数据行，应用程序将数据值分配给每个参数，并调用 **SQLExecute** 来执行该语句。  
  
 下面的示例假设计算机上有一个名为 Northwind 的 ODBC 数据源，该数据源与 Northwind 数据库相关联。  
  
 有关更多代码示例，请参阅 [SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)、 [SQLProcedures 函数](../../../odbc/reference/syntax/sqlprocedures-function.md)、 [SQLPutData 函数](../../../odbc/reference/syntax/sqlputdata-function.md)和 [SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)。  
  
```cpp
// SQLBindParameter_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define EMPLOYEE_ID_LEN 10  
  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLSMALLINT sCustID;  
  
SQLCHAR szEmployeeID[EMPLOYEE_ID_LEN];  
SQL_DATE_STRUCT dsOrderDate;  
SQLINTEGER cbCustID = 0, cbOrderDate = 0, cbEmployeeID = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, EMPLOYEE_ID_LEN, 0, szEmployeeID, 0, &cbEmployeeID);  
   retcode = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_SSHORT, SQL_INTEGER, 0, 0, &sCustID, 0, &cbCustID);  
   retcode = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TIMESTAMP, sizeof(dsOrderDate), 0, &dsOrderDate, 0, &cbOrderDate);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"INSERT INTO Orders(CustomerID, EmployeeID, OrderDate) VALUES (?, ?, ?)", SQL_NTS);  
  
   strcpy_s((char*)szEmployeeID, _countof(szEmployeeID), "BERGS");  
   sCustID = 5;  
   dsOrderDate.year = 2006;  
   dsOrderDate.month = 3;  
   dsOrderDate.day = 17;  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
### <a name="b-execute-a-stored-procedure-using-a-named-parameter"></a>B. 使用命名参数执行存储过程

 在下面的示例中，应用程序使用命名参数执行 SQL Server 存储过程。  
  
```cpp
// SQLBindParameter_Function_2.cpp  
// compile with: ODBC32.lib  
// sample assumes the following stored procedure:  
// use northwind  
// DROP PROCEDURE SQLBindParameter  
// GO  
//   
// CREATE PROCEDURE SQLBindParameter @quote int  
// AS  
// delete from orders where OrderID >= @quote  
// GO  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
SQLHDESC hIpd = NULL;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLCHAR szQuote[50] = "100084";  
SQLINTEGER cbValue = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"{call SQLBindParameter(?)}", SQL_NTS);  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 50, 0, szQuote, 0, &cbValue);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_IMP_PARAM_DESC, &hIpd, 0, 0);  
   retcode = SQLSetDescField(hIpd, 1, SQL_DESC_NAME, "@quote", SQL_NTS);  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|在语句中返回有关参数的信息|[SQLDescribeParam 函数](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|释放语句的参数缓冲区|[SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|返回语句参数的数目|[SQLNumParams 函数](../../../odbc/reference/syntax/sqlnumparams-function.md)|  
|返回要为其发送数据的下一个参数|[SQLParamData 函数](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|指定多个参数值|[SQLParamOptions 函数](../../../odbc/reference/syntax/sqlparamoptions-function.md)|  
|在执行时发送参数数据|[SQLPutData 函数](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>另请参阅

 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
