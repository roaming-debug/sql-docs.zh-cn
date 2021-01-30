---
description: SQLSetDescRec 函数
title: SQLSetDescRec 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescRec
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescRec
helpviewer_keywords:
- SQLSetDescRec function [ODBC]
ms.assetid: bf55256c-7eb7-4e3f-97ef-b0fee09ba829
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 65e0a789f38ad9f0d1d4b624caca896d01356ce2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160188"
---
# <a name="sqlsetdescrec-function"></a>SQLSetDescRec 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLSetDescRec** 函数将设置多个描述符字段，这些字段会影响绑定到列或参数数据的数据类型和缓冲区。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLSetDescRec(  
      SQLHDESC      DescriptorHandle,  
      SQLSMALLINT   RecNumber,  
      SQLSMALLINT   Type,  
      SQLSMALLINT   SubType,  
      SQLLEN        Length,  
      SQLSMALLINT   Precision,  
      SQLSMALLINT   Scale,  
      SQLPOINTER    DataPtr,  
      SQLLEN *      StringLengthPtr,  
      SQLLEN *      IndicatorPtr);  
```  
  
## <a name="arguments"></a>参数  
 *DescriptorHandle*  
 送描述符句柄。 这不能是 IRD 句柄。  
  
 *RecNumber*  
 送指示包含要设置的字段的描述符记录。 描述符记录从0开始编号，记录号0为书签记录。 此参数必须等于或大于0。 如果 *RecNumber* 大于 SQL_DESC_COUNT 的值，SQL_DESC_COUNTis 更改为 *RecNumber* 的值。  
  
 *Type*  
 送要为描述符记录设置 SQL_DESC_TYPE 字段的值。  
  
 *子类型*  
 送对于类型为 SQL_DATETIME 或 SQL_INTERVAL 的记录，这是要将 SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为的值。  
  
 *时长*  
 送要为描述符记录设置 SQL_DESC_OCTET_LENGTH 字段的值。  
  
 *精度*  
 送要为描述符记录设置 SQL_DESC_PRECISION 字段的值。  
  
 *缩放*  
 送要为描述符记录设置 SQL_DESC_SCALE 字段的值。  
  
 *DataPtr*  
 [延迟输入或输出]要为描述符记录设置 SQL_DESC_DATA_PTR 字段的值。 *DataPtr* 可以设置为 null 指针。  
  
 可以将 *DataPtr* 参数设置为 null 指针，以将 SQL_DESC_DATA_PTR 字段设置为 null 指针。 如果 *DescriptorHandle* 参数中的句柄与 ARD 相关联，则会解除对列的绑定。  
  
 *StringLengthPtr*  
 [延迟输入或输出]要为描述符记录设置 SQL_DESC_OCTET_LENGTH_PTR 字段的值。 可以将 *StringLengthPtr* 设置为 null 指针，以将 SQL_DESC_OCTET_LENGTH_PTR 字段设置为 null 指针。  
  
 *IndicatorPtr*  
 [延迟输入或输出]要为描述符记录设置 SQL_DESC_INDICATOR_PTR 字段的值。 可以将 *IndicatorPtr* 设置为 null 指针，以将 SQL_DESC_INDICATOR_PTR 字段设置为 null 指针。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetDescRec** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DESC 和 *DescriptorHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSetDescRec** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07009|描述符索引无效|*RecNumber* 参数设置为0， *DescriptorHandle* 引用 IPD 句柄。<br /><br /> *RecNumber* 参数小于0。<br /><br /> *RecNumber* 参数大于数据源可以支持的列或参数的最大数目，而 *DESCRIPTORHANDLE* 参数是 APD、IPD 或 ARD。<br /><br /> *RecNumber* 参数等于0， *DescriptorHandle* 参数称为隐式分配的 APD。  (显式分配的应用程序描述符不会出现此错误，这是因为在执行时间之前显式分配的应用程序描述符是 APD 还是 ARD。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) *DescriptorHandle* 已与一个 StatementHandle 相关联，该的异步执行函数 (未调用) 此函数，并且在调用此函数时仍在执行。<br /><br /> 为 *StatementHandle* 关联并返回 SQL_NEED_DATA (DM) **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 为与 *DescriptorHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLSetDescRec** 函数时，此 aynchronous 函数仍在执行。<br /><br />  (DM) 为与 *DescriptorHandle* 关联的其中一个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY016|无法修改实现行描述符|*DescriptorHandle* 参数与 IRD 关联。|  
|HY021|描述符信息不一致|*类型* 字段或与说明符中的 SQL_DESC_TYPE 字段关联的任何其他字段无效或不一致。<br /><br /> 一致性检查过程中检查的描述符信息不一致。  (参阅本部分后面的 "一致性检查"。 ) |  
|HY090|字符串或缓冲区长度无效| (DM) *驱动程序是一个 ODBC 2.x* 驱动程序，描述符为 ARD， *ColumnNumber* 参数设置为0，并且为参数 *BufferLength* 指定的值不等于4。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *DescriptorHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 应用程序可以调用 **SQLSetDescRec** 来设置单个列或参数的以下字段：  
  
-   SQL_DESC_TYPE  
  
-   为类型为 SQL_DATETIME 或 SQL_INTERVAL 的记录 SQL_DESC_DATETIME_INTERVAL_CODE ()   
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_DATA_PTR  
  
-   SQL_DESC_OCTET_LENGTH_PTR  
  
-   SQL_DESC_INDICATOR_PTR  
  
> [!NOTE]  
>  如果对 **SQLSetDescRec** 的调用失败，则由 *RecNumber* 参数标识的说明符记录的内容是不确定的。  
  
 绑定列或参数时， **SQLSetDescRec** 允许更改影响绑定的多个字段，而无需调用 **SQLBindCol** 或 **SQLBindParameter** ，也无需多次调用 **SQLSetDescField**。 **SQLSetDescRec** 可以对当前不与语句关联的说明符设置字段。 请注意， **SQLBindParameter** 设置了比 **SQLSetDescRec** 更多的字段，可以在一个调用中设置 APD 和 IPD 上的字段，并且不需要描述符句柄。  
  
> [!NOTE]  
>  在调用 **SQLSetDescRec** 时，应始终设置语句特性 SQL_ATTR_USE_BOOKMARKS，并将 *RecNumber* 参数设置为0来设置书签字段。 虽然这不是必需的，但强烈建议这样做。  
  
## <a name="consistency-checks"></a>一致性检查  
 每当应用程序设置 APD、ARD 或 IPD 的 SQL_DESC_DATA_PTR 字段时，驱动程序就会自动执行一致性检查。 如果任何字段与其他字段不一致，则 **SQLSetDescRec** 将返回 SQLSTATE HY021 (不一致的描述符信息) 。  
  
 每当应用程序设置 APD、ARD 或 IPD 的 SQL_DESC_DATA_PTR 字段时，驱动程序都会检查 SQL_DESC_TYPE 字段的值以及适用于该 SQL_DESC_TYPE 字段的值是否有效和一致。 调用 **SQLBindParameter** 或 **SQLBindCol** 时，或在为 APD、ARD 或 IPD 调用 **SQLSetDescRec** 时，始终会执行此检查。 此一致性检查包括对描述符字段的以下检查：  
  
-   SQL_DESC_TYPE 字段必须是有效的 ODBC C 或 SQL 类型之一，或者是特定于驱动程序的 SQL 类型。 "SQL_DESC_CONCISE_TYPE" 字段必须是有效的 ODBC C 或 SQL 类型之一，或者是特定于驱动程序的 C 或 SQL 类型，包括简洁日期时间和间隔类型。  
  
-   如果 "SQL_DESC_TYPE 记录" 字段为 SQL_DATETIME 或 SQL_INTERVAL，则 SQL_DESC_DATETIME_INTERVAL_CODE 字段必须为有效的日期时间或间隔代码之一。  (在 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中查看 SQL_DESC_DATETIME_INTERVAL_CODE 字段的说明 )   
  
-   如果 SQL_DESC_TYPE 字段指示数值类型，则将 SQL_DESC_PRECISION 和 SQL_DESC_SCALE 字段验证为有效。  
  
-   如果 SQL_DESC_CONCISE_TYPE 字段是时间或时间戳数据类型、带有秒部分的间隔类型或带有时间部分的间隔数据类型之一，则将 SQL_DESC_PRECISION 字段验证为有效的秒精度。  
  
-   如果 SQL_DESC_CONCISE_TYPE 是 interval 数据类型，则验证 SQL_DESC_DATETIME_INTERVAL_PRECISION 字段是否为有效的间隔前导精度值。  
  
 通常不会设置 IPD 的 SQL_DESC_DATA_PTR 字段;但是，应用程序可以执行此操作来强制执行 IPD 字段的一致性检查。 不能对 IRD 执行一致性检查。 将 IPD 的 SQL_DESC_DATA_PTR 字段设置为的值实际上并不存储，无法通过对 **SQLGetDescField** 或 **SQLGetDescRec** 的调用来检索。设置仅用于强制执行一致性检查。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|绑定列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|绑定参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|获取单个描述符字段|[SQLGetDescField 函数](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|获取多个描述符字段|[SQLGetDescRec 函数](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|设置单个描述符字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
