---
description: SQLCopyDesc 函数
title: SQLCopyDesc 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCopyDesc
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLCopyDesc
helpviewer_keywords:
- SQLCopyDesc function [ODBC]
ms.assetid: d5450895-3824-44c4-8aa4-d4f9752a9602
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab2ef18e777d433daf4ab93b73d249e62507ea3c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206304"
---
# <a name="sqlcopydesc-function"></a>SQLCopyDesc 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLCopyDesc** 将描述符信息从一个描述符句柄复制到另一个描述符句柄。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLCopyDesc(  
     SQLHDESC     SourceDescHandle,  
     SQLHDESC     TargetDescHandle);  
```  
  
## <a name="arguments"></a>参数  
 *SourceDescHandle*  
 送源描述符句柄。  
  
 *TargetDescHandle*  
 送目标描述符句柄。 *TargetDescHandle* 参数可以是应用程序描述符或 IPD 的句柄。 不能将 *TargetDescHandle* 设置为 IRD 的句柄，否则 **SQLCOPYDESC** 将返回 SQLSTATE HY016 (不能) 修改实现行描述符。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLCopyDesc** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DESC 和 *TargetDescHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 如果调用中传递了无效的 *SourceDescHandle* ，则将返回 SQL_INVALID_HANDLE，但不会返回 SQLSTATE。 下表列出了通常由 **SQLCopyDesc** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
 返回错误时，对 **SQLCopyDesc** 的调用将立即中止，并且 *TargetDescHandle* 说明符中的字段内容不确定。  
  
 由于可以通过调用 **SQLGetDescField** 和 **SQLSetDescField** 来实现 **SQLCopyDesc** ，因此 **SQLCopyDesc** 可能返回 **SQLSTATEs** 或 **SQLGetDescField** 返回的 SQLSetDescField。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY007|未准备关联语句|*SourceDescHandle* 与 IRD 相关联，并且关联的语句句柄未处于已准备或已执行状态。|  
|HY010|函数序列错误| (DM) *SourceDescHandle* 或 *TargetDescHandle* 中的描述符句柄与一个 StatementHandle 相关联，该的异步执行函数 (未调用) 此函数，并且在调用此函数时仍在执行。<br /><br />  (DM) *SourceDescHandle* 或 *TargetDescHandle* 中的描述符句柄与调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 的 *StatementHandle* 相关联，并 SQL_NEED_DATA 返回。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 为与 *SourceDescHandle* 或 *TargetDescHandle* 相关联的连接句柄调用了异步执行函数。 调用 **SQLCopyDesc** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为与 *SourceDescHandle* 或 *TargetDescHandle* 关联的其中一个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY016|无法修改实现行描述符|*TargetDescHandle* 与 IRD 相关联。|  
|HY021|描述符信息不一致|在一致性检查过程中检查的描述符信息不一致。 有关详细信息，请参阅 **SQLSetDescField** 中的 "一致性检查"。|  
|HY092|无效的属性/选项标识符|调用 **SQLCopyDesc** 时，会提示调用 **SQLSetDescField**，但 *\* 将 valueptr* 对 *TargetDescHandle* 上的 *FieldIdentifier* 参数无效。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *SourceDescHandle* 或 *TargetDescHandle* 相关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 对 **SQLCopyDesc** 的调用会将源描述符句柄的字段复制到目标描述符句柄。 字段只能复制到应用程序描述符或 IPD，但不能复制到 IRD。 可以从应用程序或实现描述符复制字段。  
  
 仅当语句句柄处于已准备或已执行状态时，才可以从 IRD 复制字段;否则，该函数返回 SQLSTATE HY007 (关联语句未) 准备。  
  
 无论语句是否已准备就绪，都可以从 IPD 复制字段。 如果已准备好具有动态参数的 SQL 语句，并支持和启用 IPD 的自动填充，则驱动程序将填充 IPD。 当用 IPD 作为 *SourceDescHandle* 调用 **SQLCopyDesc** 时，会复制填充的字段。 如果驱动程序未填充 IPD，则会复制 IPD 中最初的字段的内容。  
  
 描述符的所有字段（除了 SQL_DESC_ALLOC_TYPE (指定是否会复制说明符句柄) ，无论是否为目标说明符定义了字段），都是如此。 复制的字段覆盖现有字段。  
  
 如果 *SourceDescHandle* 和 *TargetDescHandle* 参数与同一驱动程序相关联，则驱动程序将复制所有描述符字段，即使这些驱动程序在两个不同的连接或环境中也是如此。 如果 *SourceDescHandle* 和 *TargetDescHandle* 参数与不同的驱动程序相关联，则驱动程序管理器会复制 odbc 定义的字段，但不会复制不是由 ODBC 为描述符类型定义的驱动程序定义的字段或字段。  
  
 如果发生错误，则立即中止对 **SQLCopyDesc** 的调用。  
  
 复制 SQL_DESC_DATA_PTR 字段时，会对目标描述符执行一致性检查。 如果一致性检查失败，则返回 SQLSTATE HY021 (不一致的描述符信息) ，并立即中止对 **SQLCopyDesc** 的调用。 有关一致性检查的详细信息，请参阅 [SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)中的 "一致性检查"。  
  
 即使连接在不同环境下，也可以在连接间复制描述符句柄。 如果驱动程序管理器检测到源和目标描述符句柄不属于相同的连接，并且两个连接属于单独的驱动程序，则它将通过使用 **SQLGetDescField** 和 **SQLSetDescField** 执行逐字段复制来实现 **SQLCopyDesc** 。  
  
 当使用一个驱动程序的 *SourceDescHandle* 调用 **SQLCopyDesc** ，并在另一个驱动程序上调用 *TargetDescHandle* 时，将清除 *SourceDescHandle* 的错误队列。 出现这种情况的原因是，在这种情况下， **SQLCopyDesc** 是通过调用 **SQLGetDescField** 和 **SQLSetDescField** 来实现的。  
  
> [!NOTE]  
>  应用程序可以将显式分配的描述符句柄与 *StatementHandle* 关联，而不是调用 **SQLCopyDesc** 将字段从一个描述符复制到另一个说明符。 可以通过将 SQL_ATTR_APP_ROW_DESC 或 SQL_ATTR_APP_PARAM_DESC 语句特性设置为显式分配的描述符的句柄，将显式分配的描述符与同一 *ConnectionHandle* 上的另一个 *StatementHandle* 关联。 完成此操作后，无需调用 **SQLCopyDesc** 即可将描述符字段值从一个描述符复制到另一个描述符。 但描述符句柄不能与另一个 *ConnectionHandle* 上的 *StatementHandle* 相关联;若要在不同 *ConnectionHandles* 上的 *StatementHandles* 上使用相同的描述符字段值，则必须调用 **SQLCopyDesc** 。  
  
 有关描述符标头或记录中的字段的说明，请参阅 [SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)。 有关描述符的详细信息，请参阅 [描述符](../../../odbc/reference/develop-app/descriptors.md)。  
  
## <a name="copying-rows-between-tables"></a>在表之间复制行  
 应用程序可以将数据从一个表复制到另一个表，而无需在应用程序级别复制数据。 为此，应用程序将相同的数据缓冲区和描述符信息绑定到一个语句，该语句提取数据和将数据插入副本的语句。 这可以通过以下方式完成：共享应用程序描述符 (将显式分配的描述符绑定为 ARD 到一个语句，将 APD 同时绑定到另一个) 中，或通过使用 **SQLCopyDesc** 复制两个语句的 ARD 和 APD 之间的绑定。 如果语句位于不同的连接上，则必须使用 **SQLCopyDesc** 。 此外，还必须调用 **SQLCopyDesc** ，以便复制 IRD 和两个语句的 IPD 之间的绑定。 在同一连接上跨语句复制时，驱动程序返回的 SQL_ACTIVE_STATEMENTS 信息类型必须大于1，此 **操作才能成功** 。  (在连接间进行复制时，不会出现这种情况。 )   
  
### <a name="code-example"></a>代码示例  
 在下面的示例中，使用描述符操作将 PartsSource 表的字段复制到 PartsCopy 表中。 PartsSource 表的内容将提取到 *hstmt0* 中的行集缓冲区中。 这些值用作 *hstmt1* 上 INSERT 语句的参数，用于填充 PartsCopy 表的列。 为此， *hstmt0* 的 IRD 字段将被复制到 *hstmt1* 的 IPD 字段，并将 *hstmt0* 的 ARD 字段复制到 *APD 的 hstmt1 字段。* 将 IRD 字段从包含输出参数的语句复制到需要输入参数的 IPD 字段时，可以使用 **SQLSetDescField** 将 IPD SQL_DESC_PARAMETER_TYPE 特性设置为 SQL_PARAM_INPUT。  
  
```cpp  
#define ROWS 100  
#define DESC_LEN 50  
#define SQL_SUCCEEDED(rc) (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO)  
  
// Template for a row  
typedef struct {  
   SQLINTEGER   sPartID;  
   SQLINTEGER   cbPartID;  
   SQLUCHAR     szDescription[DESC_LENGTH];  
   SQLINTEGER   cbDescription;  
   REAL         sPrice;  
   SQLINTEGER   cbPrice;  
} PartsSource;  
  
PartsSource    rget[ROWS];          // rowset buffer  
SQLUSMALLINT   sts_ptr[ROWS];       // status pointer  
SQLHSTMT       hstmt0, hstmt1;  
SQLHDESC       hArd0, hIrd0, hApd1, hIpd1;  
  
// ARD and IRD of hstmt0  
SQLGetStmtAttr(hstmt0, SQL_ATTR_APP_ROW_DESC, &hArd0, 0, NULL);  
SQLGetStmtAttr(hstmt0, SQL_ATTR_IMP_ROW_DESC, &hIrd0, 0, NULL);  
  
// APD and IPD of hstmt1  
SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_PARAM_DESC, &hApd1, 0, NULL);  
SQLGetStmtAttr(hstmt1, SQL_ATTR_IMP_PARAM_DESC, &hIpd1, 0, NULL);  
  
// Use row-wise binding on hstmt0 to fetch rows  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER) sizeof(PartsSource), 0);  
  
// Set rowset size for hstmt0  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
  
// Execute a select statement  
SQLExecDirect(hstmt0, "SELECT PARTID, DESCRIPTION, PRICE FROM PARTS ORDER BY 3, 1, 2"",  
               SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt0, 1, SQL_C_SLONG, rget[0].sPartID, 0,   
   &rget[0].cbPartID);  
SQLBindCol(hstmt0, 2, SQL_C_CHAR, &rget[0].szDescription, DESC_LEN,   
   &rget[0].cbDescription);  
SQLBindCol(hstmt0, 3, SQL_C_FLOAT, rget[0].sPrice,   
   0, &rget[0].cbPrice);  
  
// Perform parameter bindings on hstmt1.   
SQLCopyDesc(hArd0, hApd1);  
SQLCopyDesc(hIrd0, hIpd1);  
  
// Set the array status pointer of IRD  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_STATUS_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the ARRAY_STATUS_PTR field of APD to be the same  
// as that in IRD.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_PARAM_OPERATION_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the hIpd1 records as input parameters  
rc = SQLSetDescField(hIpd1, 1, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 2, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 3, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
  
// Prepare an insert statement on hstmt1. PartsCopy is a copy of  
// PartsSource  
SQLPrepare(hstmt1, "INSERT INTO PARTS_COPY VALUES (?, ?, ?)", SQL_NTS);  
  
// In a loop, fetch a rowset, and copy the fetched rowset to PARTS_COPY  
  
rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
while (SQL_SUCCEEDED(rc)) {  
  
   // After the call to SQLFetchScroll, the status array has row   
   // statuses. This array is used as input status in the APD  
   // and hence determines which elements of the rowset buffer  
   // are inserted.  
   SQLExecute(hstmt1);  
  
   rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
} // while  
```  
  
### <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|获取多个描述符字段|[SQLGetDescRec 函数](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|设置单个描述符字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|设置多个描述符字段|[SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
