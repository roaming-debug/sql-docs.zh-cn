---
description: SQLGetDescField 函数
title: SQLGetDescField 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescField
helpviewer_keywords:
- SQLGetDescField function [ODBC]
ms.assetid: f09ff660-1e4a-4370-be85-90d4da0487d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0692c769921d8e0ee326b19f0b5d501367237c28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201951"
---
# <a name="sqlgetdescfield-function"></a>SQLGetDescField 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetDescField** 返回描述符记录的单个字段的当前设置或值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>参数  
 *DescriptorHandle*  
 送描述符句柄。  
  
 *RecNumber*  
 送指示应用程序从中查找信息的描述符记录。 描述符记录从0开始编号，记录号0为书签记录。 如果 *FieldIdentifier* 参数指示标头字段，则将忽略 *RecNumber* 。 如果 *RecNumber* 小于或等于 SQL_DESC_COUNT 但行不包含列或参数的数据，则对 **SQLGetDescField** 的调用将返回字段的默认值。  (有关详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中的 "描述符字段的初始化"。 )   
  
 *FieldIdentifier*  
 送指示要返回其值的描述符字段。 有关详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中的 "*FieldIdentifier* 参数" 一节。  
  
 *ValuePtr*  
 输出一个指针，指向要在其中返回描述符信息的缓冲区。 数据类型取决于 *FieldIdentifier* 的值。  
  
 如果 *将 valueptr* 是整数类型，应用程序应使用 sqlulen 生成的缓冲区并在调用此函数之前将值初始化为0，因为某些驱动程序只能写入缓冲区的较低32位或16位，并使高阶位保持不变。  
  
 如果 *将 valueptr* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的总字节数) 可在 *将 valueptr* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送如果 *FieldIdentifier* 是一个 ODBC 定义的字段，而 *将 valueptr* 指向某个字符串或二进制缓冲区，则此参数的长度应为 \* *将 valueptr*。 如果 *FieldIdentifier* 是一个 ODBC 定义的字段，而 \* *将 valueptr* 是一个整数，则将忽略 *BufferLength* 。 如果在调用 **SQLGetDescFieldW**) 时， *\* 将 valueptr* 中的值为 Unicode 数据类型 (，则 *BufferLength* 参数必须是偶数。  
  
 如果 *FieldIdentifier* 是驱动程序定义的字段，应用程序会通过设置 *BufferLength* 参数来指示该字段的特性。 *BufferLength* 可具有以下值：  
  
-   如果 *\* 将 valueptr* 是指向字符串的指针，则 *BufferLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *\* 将 valueptr* 是一个指向二进制缓冲区的指针，则该应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *BufferLength* 中。 这会在 *BufferLength* 中置入负值。  
  
-   如果 *\* 将 valueptr* 是一个指向字符串或二进制字符串以外的值的指针，则 *BufferLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *\* 将 valueptr* 包含固定长度的数据类型，则 *BufferLength* SQL_IS_INTEGER、SQL_IS_UINTEGER、SQL_IS_SMALLINT 或 SQL_IS_USMALLINT，视情况而定。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 null 终止) 字符所需的字节数 (排除 **将 valueptr* 中可返回的字节数。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_NO_DATA 或 SQL_INVALID_HANDLE。  
  
 如果 *RecNumber* 大于当前说明符记录数，则返回 SQL_NO_DATA。  
  
 如果 *DescriptorHandle* 是 IRD 句柄，并且该语句处于已准备或已执行状态，但没有打开的游标，则返回 SQL_NO_DATA。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetDescField** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetDescField** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *将 valueptr* 不够大，无法返回整个描述符字段，因此字段被截断。 在 **StringLengthPtr* 中返回未截断描述符字段的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07009|描述符索引无效| (DM) *RecNumber* 参数等于0，SQL_ATTR_USE_BOOKMARK 语句特性为 SQL_UB_OFF， *DESCRIPTORHANDLE* 参数为 IRD 句柄。  (如果描述符与语句句柄相关联，则只能为显式分配的描述符返回此错误。 ) <br /><br /> *FieldIdentifier* 参数是一个记录字段， *RecNumber* 参数为0， *DescriptorHandle* 参数是 IPD 句柄。<br /><br /> *RecNumber* 参数小于0。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY007|未准备关联语句|*DescriptorHandle* 已与 *StatementHandle* 相关联，但未准备好或未执行关联的语句句柄。|  
|HY010|函数序列错误| (DM) *DescriptorHandle* 与一个 StatementHandle 相关联，该的异步执行函数 (未调用) 此函数，并且在调用此函数时仍在执行。<br /><br />  (DM) *DescriptorHandle* 与已调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 的 *StatementHandle* 相关联，并 SQL_NEED_DATA 返回。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 为与 *DescriptorHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLGetDescField** 函数时，此异步函数仍在执行。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY021|描述符信息不一致|"SQL_DESC_TYPE" 和 "SQL_DESC_DATETIME_INTERVAL_CODE" 字段不构成有效的 ODBC SQL 类型、适用于 Ipd) 的特定于驱动程序的有效 SQL 类型 (或 Apd 或 ARDs (的有效 ODBC C 类型。|  
|HY090|字符串或缓冲区长度无效| (DM) *\* 将 valueptr* 是一个字符串，而 *BufferLength* 小于零。|  
|HY091|描述符字段标识符无效|*FieldIdentifier* 不是一个 ODBC 定义的字段，并且不是实现定义的值。<br /><br /> 未为 *DescriptorHandle* 定义 *FieldIdentifier* 。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *DescriptorHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 应用程序可以调用 **SQLGetDescField** 来返回描述符记录的单个字段的值。 对 **SQLGetDescField** 的调用可以返回任何描述符类型中任何字段的设置，包括标头字段、记录字段和书签字段。 应用程序可以通过重复调用 **SQLGetDescField**，以任意顺序获取同一或不同描述符中多个字段的设置。 还可以调用 **SQLGetDescField** 来返回驱动程序定义的描述符字段。  
  
 出于性能方面的考虑，应用程序不应在执行语句前调用 IRD 的 **SQLGetDescField** 。  
  
 还可以通过对 **SQLGetDescRec** 的单一调用来检索描述列或参数数据的名称、数据类型和存储的多个字段的设置。 可以调用 **SQLGetStmtAttr** 来返回描述符标头中的单个字段的设置，该字段也是语句特性。 **SQLColAttribute**、 **SQLDescribeCol** 和 **SQLDescribeParam** 返回记录或书签字段。  
  
 当应用程序调用 **SQLGetDescField** 来检索为特定描述符类型定义的字段的值时，该函数将返回 SQL_SUCCESS 但为该字段返回的值是不确定的。 例如，对 APD 或 ARD 的 SQL_DESC_NAME 或 SQL_DESC_NULLABLE 字段调用 **SQLGetDescField** 将返回 SQL_SUCCESS，但不会返回该字段的未定义值。  
  
 当应用程序调用 **SQLGetDescField** 来检索为特定描述符类型定义的字段的值，但该字段没有默认值并且尚未设置，则该函数将返回 SQL_SUCCESS 但不定义为该字段返回的值。 有关初始化描述符字段和字段说明的详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中的 "描述符字段的初始化"。 有关描述符的详细信息，请参阅 [描述符](../../../odbc/reference/develop-app/descriptors.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|获取多个描述符字段|[SQLGetDescRec 函数](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|设置单个描述符字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|设置多个描述符字段|[SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
