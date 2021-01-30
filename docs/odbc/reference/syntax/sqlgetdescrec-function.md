---
description: SQLGetDescRec 函数
title: SQLGetDescRec 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescRec
helpviewer_keywords:
- SQLGetDescRec function [ODBC]
ms.assetid: 325e0907-8e87-44e8-a111-f39e636a9cbc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a340ecc75e7ae4f03c44051f594fda7305a2a00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205637"
---
# <a name="sqlgetdescrec-function"></a>SQLGetDescRec 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetDescRec** 返回描述符记录的多个字段的当前设置或值。 返回的字段描述列或参数数据的名称、数据类型和存储。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetDescRec(  
      SQLHDESC        DescriptorHandle,  
      SQLSMALLINT     RecNumber,  
      SQLCHAR *       Name,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLSMALLINT *   TypePtr,  
      SQLSMALLINT *   SubTypePtr,  
      SQLLEN *        LengthPtr,  
      SQLSMALLINT *   PrecisionPtr,  
      SQLSMALLINT *   ScalePtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>参数  
 *DescriptorHandle*  
 送描述符句柄。  
  
 *RecNumber*  
 送指示应用程序从中查找信息的描述符记录。 说明符记录的编号为1，记录号0为书签记录。 *RecNumber* 参数必须小于或等于 SQL_DESC_COUNT 的值。 如果 *RecNumber* 小于或等于 SQL_DESC_COUNT 但行不包含列或参数的数据，则对 **SQLGetDescRec** 的调用将返回字段的默认值。  (有关详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中的 "描述符字段的初始化"。 )   
  
 *名称*  
 输出指向缓冲区的指针，将在该缓冲区中返回描述符记录的 SQL_DESC_NAME 字段。  
  
 如果 *name* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符的总字符数) 可在按 *名称* 指向的缓冲区中返回。  
  
 *BufferLength*  
 送**名称* 缓冲区的长度（以字符为限）。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回名称缓冲区中可返回的数据的字符数 \*  ，不包括 null 终止字符。 如果字符数大于或等于 *BufferLength*，则名称中的数据 \* 将被截断为 *BufferLength* 减去 null 终止字符的长度，并以 null 值终止于驱动程序。  
  
 *TypePtr*  
 输出指向缓冲区的指针，该缓冲区用于返回描述符记录的 SQL_DESC_TYPE 字段的值。  
  
 *SubTypePtr*  
 输出对于类型为 SQL_DATETIME 或 SQL_INTERVAL 的记录，这是指向要在其中返回 SQL_DESC_DATETIME_INTERVAL_CODE 字段值的缓冲区的指针。  
  
 *LengthPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回描述符记录的 SQL_DESC_OCTET_LENGTH 字段的值。  
  
 *PrecisionPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回描述符记录的 SQL_DESC_PRECISION 字段的值。  
  
 *ScalePtr*  
 输出指向缓冲区的指针，该缓冲区用于返回描述符记录的 SQL_DESC_SCALE 字段的值。  
  
 *NullablePtr*  
 输出指向缓冲区的指针，该缓冲区用于返回描述符记录的 SQL_DESC_NULLABLE 字段的值。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_NO_DATA 或 SQL_INVALID_HANDLE。  
  
 如果 *RecNumber* 大于当前说明符记录数，则返回 SQL_NO_DATA。  
  
 如果 *DescriptorHandle* 是 IRD 句柄，并且该语句处于已准备或已执行状态，但没有打开的游标，则返回 SQL_NO_DATA。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetDescRec** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DESC 和 *DescriptorHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetDescRec** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *名称* 不够大，无法返回整个描述符字段。 因此，该字段已被截断。 在 **StringLengthPtr* 中返回未截断描述符字段的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07009|描述符索引无效|*FieldIdentifier* 参数是一个记录字段， *RecNumber* 参数设置为0， *DescriptorHandle* 参数是 IPD 句柄。<br /><br />  (DM) *RecNumber* 参数设置为0，SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF，并且 *DESCRIPTORHANDLE* 参数为 IRD 句柄。<br /><br /> *RecNumber* 参数小于0。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY007|未准备关联语句|*DescriptorHandle* 与 IRD 相关联，并且关联的语句句柄未处于已准备或已执行状态。|  
|HY010|函数序列错误| (DM) *DescriptorHandle* 与一个 StatementHandle 相关联，该的异步执行函数 (未调用) 此函数，并且在调用此函数时仍在执行。<br /><br />  (DM) *DescriptorHandle* 与已调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 的 *StatementHandle* 相关联，并 SQL_NEED_DATA 返回。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 为与 *DescriptorHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLGetDescRec** 时仍在执行此异步函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *DescriptorHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 应用程序可以调用 **SQLGetDescRec** 来检索单个列或参数的下列描述符字段的值：  
  
-   SQL_DESC_NAME  
  
-   SQL_DESC_TYPE  
  
-   为类型为 SQL_DATETIME 或 SQL_INTERVAL 的记录 SQL_DESC_DATETIME_INTERVAL_CODE ()   
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_NULLABLE  
  
 **SQLGetDescRec** 不会检索标头字段的值。  
  
 应用程序可以通过将与字段相对应的参数设置为 null 指针，来防止返回字段的设置。  
  
 当应用程序调用 **SQLGetDescRec** 来检索为特定描述符类型定义的字段的值时，该函数将返回 SQL_SUCCESS 但为该字段返回的值是不确定的。 例如，对 APD 或 ARD 的 SQL_DESC_NAME 或 SQL_DESC_NULLABLE 字段调用 **SQLGetDescRec** 将返回 SQL_SUCCESS，但不会返回该字段的未定义值。  
  
 当应用程序调用 **SQLGetDescRec** 来检索为特定描述符类型定义的字段的值，但该字段没有默认值并且尚未设置，则该函数将返回 SQL_SUCCESS 但不定义为该字段返回的值。 有关详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中的 "描述符字段的初始化"。  
  
 还可以通过调用 **SQLGetDescField** 来单独检索字段的值。 有关描述符标头或记录中的字段的说明，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)。 有关描述符的详细信息，请参阅 [描述符](../../../odbc/reference/develop-app/descriptors.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|绑定列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|绑定参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|获取描述符字段|[SQLGetDescField 函数](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|设置多个描述符字段|[SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
