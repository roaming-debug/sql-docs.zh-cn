---
description: SQLSetStmtAttr 函数
title: SQLSetStmtAttr 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetStmtAttr
helpviewer_keywords:
- SQLSetStmtAttr function [ODBC]
ms.assetid: 7abc5260-733a-48d4-9974-2d1a6a9ea5f6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae590b9ba8a01182f32928f874f0417b554f00a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191770"
---
# <a name="sqlsetstmtattr-function"></a>SQLSetStmtAttr 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLSetStmtAttr** 设置与语句相关的属性。  
  
> [!NOTE]
>  有关 ODBC *3.x 应用程序**使用 odbc 2.x* 驱动程序时，驱动程序管理器将此函数映射到的内容的详细信息，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLSetStmtAttr(  
     SQLHSTMT      StatementHandle,  
     SQLINTEGER    Attribute,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    StringLength);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *Attribute*  
 送要设置的选项，请在 "注释" 中列出。  
  
 *ValuePtr*  
 送要与 *属性* 关联的值。 根据 *属性* 的值， *将 valueptr* 将是以下项之一：  
  
-   ODBC 描述符句柄。  
  
-   SQLUINTEGER 值。  
  
-   SQLULEN 生成值。  
  
-   指向以下项之一的指针：  
  
    -   以 null 值结束的字符串。  
  
    -   二进制缓冲区。  
  
    -   类型为 SQLLEN、SQLULEN 生成或 SQLUSMALLINT 的值或数组。  
  
    -   驱动程序定义的值。  
  
 如果 *特性* 参数是驱动程序特定的值，则 *将 valueptr* 可能是一个有符号整数。  
  
 *StringLength*  
 送如果 *attribute* 是一个 ODBC 定义的属性，并且 *将 valueptr* 指向某个字符串或二进制缓冲区，则此参数应为 \* *将 valueptr* 的长度。 如果 *attribute* 是一个 ODBC 定义的属性，并且 *将 valueptr* 是一个整数，则将忽略 *StringLength* 。  
  
 如果 *attribute* 是驱动程序定义的属性，则应用程序通过设置 *StringLength* 参数来指示属性对驱动程序管理器的性质。 *StringLength* 可具有以下值：  
  
-   如果 *将 valueptr* 是指向字符串的指针，则 *StringLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *将 valueptr* 是一个指向二进制缓冲区的指针，则该应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *StringLength* 中。 这会在 *StringLength* 中置入负值。  
  
-   如果 *将 valueptr* 是一个指向字符串或二进制字符串以外的值的指针，则 *StringLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *将 valueptr* 包含固定长度的值，则 *StringLength* 根据需要 SQL_IS_INTEGER 或 SQL_IS_UINTEGER。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetStmtAttr** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSetStmtAttr** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|该驱动程序不支持 *将 valueptr* 中指定的值，或者在 *将 valueptr* 中指定的值因实现工作条件而无效，因此驱动程序将替换相似的值。 可以调用 (**SQLGetStmtAttr** 来确定暂时替换的值。 ) 替换值对 *StatementHandle* 有效，直到游标关闭，此时语句特性会恢复为其以前的值。 可以更改的语句特性如下：<br /><br /> SQL_ ATTR_CONCURRENCY SQL_ ATTR_CURSOR_TYPE SQL_ ATTR_KEYSET_SIZE SQL_ ATTR_MAX_LENGTH SQL_ ATTR_MAX_ROWS SQL_ ATTR_QUERY_TIMEOUT SQL_ATTR_ROW_ARRAY_SIZE SQL_ ATTR_SIMULATE_CURSOR<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|*属性* 已 SQL_ATTR_CONCURRENCY、SQL_ATTR_CURSOR_TYPE、SQL_ATTR_SIMULATE_CURSOR 或 SQL_ATTR_USE_BOOKMARKS，并且游标已打开。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY009|空值指针的使用无效|*特性* 参数标识了需要字符串特性的语句特性，而 *将 valueptr* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLSetStmtAttr** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY011|现在无法设置属性|*属性* 已 SQL_ATTR_CONCURRENCY、SQL_ ATTR_CURSOR_TYPE、SQL_ ATTR_SIMULATE_CURSOR 或 SQL_ ATTR_USE_BOOKMARKS 并且已准备好该语句。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY017|使用自动分配的描述符句柄无效| (DM) SQL_ATTR_IMP_ROW_DESC 或 SQL_ATTR_IMP_PARAM_DESC *属性* 参数。<br /><br />  (DM) *属性* 参数是 SQL_ATTR_APP_ROW_DESC 或 SQL_ATTR_APP_PARAM_DESC，并且 *将 valueptr* 中的值是隐式分配的描述符句柄，而不是最初为 ARD 或 APD 分配的句柄。|  
|HY024|属性值无效|给定指定的 *属性* 值， *将 valueptr* 中指定的值无效。  (，驱动程序管理器只为接受一组离散值（如 SQL_ATTR_ACCESS_MODE 或 SQL_ ATTR_ASYNC_ENABLE 的连接和语句属性返回此 SQLSTATE。 对于所有其他连接和语句属性，驱动程序必须验证 *将 valueptr* 中指定的值。 ) <br /><br /> *特性* 参数是 SQL_ATTR_APP_ROW_DESC 或 SQL_ATTR_APP_PARAM_DESC，*将 valueptr* 是显式分配的描述符句柄，该句柄不在与 *StatementHandle* 参数相同的连接上。|  
|HY090|字符串或缓冲区长度无效| (DM) *\* 将 valueptr* 是一个字符串，而 *StringLength* 参数小于0，但却不是 SQL_NTS 的。|  
|HY092|无效的属性/选项标识符| (DM) 为参数 *属性* 指定的值对于该驱动程序支持的 ODBC 版本无效。<br /><br />  (DM) 为参数 *属性* 指定的值为只读属性。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|为 argument *特性* 指定的值为驱动程序支持的 odbc 版本的有效 odbc 语句属性，但驱动程序不支持该属性。<br /><br /> *特性* 参数已 SQL_ATTR_ASYNC_ENABLE，对 *InfoType* 为 SQL_ASYNC_MODE 的 **SQLGetInfo** 的调用返回 SQL_AM_CONNECTION。<br /><br /> *特性* 参数 SQL_ATTR_ENABLE_AUTO_IPD，而连接属性 SQL_ATTR_AUTO_IPD 的值 SQL_FALSE 为。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|S1118|驱动程序不支持异步通知|如果调用 **SQLSetStmtAttr** SQL_ATTR_ASYNC_STMT_EVENT 设置，则为;驱动程序不支持异步通知。|  
  
## <a name="comments"></a>注释  
 语句的语句特性始终有效，直到对 **SQLSetStmtAttr** 的另一次调用更改或通过调用 **SQLFreeHandle** 删除该语句。 用 SQL_CLOSE、SQL_UNBIND 或 SQL_RESET_PARAMS 选项调用 **SQLFreeStmt** 不会重置语句特性。  
  
 如果数据源不支持 *将 valueptr* 中指定的值，则某些语句特性支持替换相似的值。 在这种情况下，驱动程序将返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01S02 (选项值) 更改。 例如，如果 *属性* 为 SQL_ATTR_CONCURRENCY 并且 *将 valueptr* 是 SQL_CONCUR_ROWVER 的，并且数据源不支持此功能，则驱动程序将替换 SQL_CONCUR_VALUES 并返回 SQL_SUCCESS_WITH_INFO。 若要确定替换值，应用程序将调用 **SQLGetStmtAttr**。  
  
 用 *将 valueptr* 设置的信息的格式取决于指定的 *属性*。 **SQLSetStmtAttr** 接受以下两种不同格式的特性信息：字符串或整数值。 每个的格式记录在属性的说明中。 此格式适用于在 **SQLGetStmtAttr** 中为每个属性返回的信息。 **SQLSetStmtAttr** 的 *将 valueptr* 参数指向的字符串的长度为 *StringLength*。  
  
> [!NOTE]
>  通过调用 **SQLSetConnectAttr** 在连接级别设置语句属性的功能已 *在 ODBC 2.x 中弃用。* ODBC *2.x* 应用程序决不应在连接级别设置语句属性。 ODBC *2.x* 语句特性不能在连接级别设置，但 SQL_ATTR_METADATA_ID 和 SQL_ATTR_ASYNC_ENABLE 属性（它们都是连接属性和语句特性）除外，可以在连接级别或语句级别设置。  
> 
> [!NOTE]
>  如果 ODBC 2.x 驱动程序应使用在连接级别设置 ODBC *2.x 语句选项* 的 *odbc 2.x* *应用程序，* 则只需支持此功能。 有关详细信息，请参阅附录 G：用于向后兼容的驱动程序准则中的 [SQLSetConnectOption 映射](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md) 中的 "在连接级别设置语句选项"。  
  
## <a name="statement-attributes-that-set-descriptor-fields"></a>设置描述符字段的语句特性  
 许多语句特性对应于描述符的标头字段。 设置这些属性实际上会导致描述符字段的设置。 通过调用 **SQLSetStmtAttr** 而不是 **SQLSetDescField** 设置字段具有这样的优点：不必为函数调用获取描述符句柄。  
  
> [!CAUTION]  
>  对一个语句调用 **SQLSetStmtAttr** 可能会影响其他语句。 当显式分配与该语句关联的 APD 或 ARD 并同时与其他语句关联时，会发生这种情况。 由于 **SQLSetStmtAttr** 修改了 APD 或 ARD，因此修改应用于与此描述符关联的所有语句。 如果这不是所需的行为，应用程序应将此描述符与其他语句取消关联 (通过调用 **SQLSetStmtAttr** 将 SQL_ATTR_APP_ROW_DESC 或 SQL_ATTR_APP_PARAM_DESC 字段设置为另一个描述符句柄) ，然后再次调用 **SQLSetStmtAttr** 。  
  
 如果将描述符字段设置为所设置的相应语句属性的结果，则仅为当前与 *StatementHandle* 参数标识的语句相关联的适用描述符设置该字段，并且该属性设置不会影响将来可能与该语句关联的所有描述符。 当也是语句属性的描述符字段通过调用 **SQLSetDescField** 设置时，将设置相应的语句属性。 如果显式分配的描述符与语句取消关联，则与标头字段对应的语句特性将恢复为隐式分配的描述符中的字段的值。  
  
 分配语句时 (参阅 [SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)) ，系统会自动分配四个描述符句柄并将其与该语句相关联。 可以通过使用 SQL_HANDLE_DESC 的 *fHandleType* 调用 **SQLAllocHandle** 来关联显式分配的描述符句柄，以便分配描述符句柄，然后调用 **SQLSetStmtAttr** 以将描述符句柄与语句相关联。  
  
 下表中的语句特性对应于描述符标头字段。  
  
|语句特性|标头字段|Desc.|  
|-------------------------|------------------|-----------|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR|SQL_DESC_BIND_OFFSET_PTR|APD|  
|SQL_ATTR_PARAM_BIND_TYPE|SQL_DESC_BIND_TYPE|APD|  
|SQL_ATTR_PARAM_OPERATION_PTR|SQL_DESC_ARRAY_STATUS_PTR|APD|  
|SQL_ATTR_PARAM_STATUS_PTR|SQL_DESC_ARRAY_STATUS_PTR|IPD|  
|SQL_ATTR_PARAMS_PROCESSED_PTR|SQL_DESC_ROWS_PROCESSED_PTR|IPD|  
|SQL_ATTR_PARAMSET_SIZE|SQL_DESC_ARRAY_SIZE|APD|  
|SQL_ATTR_ROW_ARRAY_SIZE|SQL_DESC_ARRAY_SIZE|ARD|  
|SQL_ATTR_ROW_BIND_OFFSET_PTR|SQL_DESC_BIND_OFFSET_PTR|ARD|  
|SQL_ATTR_ROW_BIND_TYPE|SQL_DESC_BIND_TYPE|ARD|  
|SQL_ATTR_ROW_OPERATION_PTR|SQL_DESC_ARRAY_STATUS_PTR|ARD|  
|SQL_ATTR_ROW_STATUS_PTR|SQL_DESC_ARRAY_STATUS_PTR|IRD|  
|SQL_ATTR_ROWS_FETCHED_PTR|SQL_DESC_ROWS_PROCESSED_PTR|IRD|  
  
## <a name="statement-attributes"></a>语句属性  
 下表显示了当前定义的属性及其引入的 ODBC 版本。预计，驱动程序会定义更多的属性以利用不同的数据源。 ODBC 保留了一系列属性;驱动程序开发人员必须在开放组中保留其自己的特定于驱动程序的使用值。 有关详细信息，请参阅 [驱动程序特定的数据类型、描述符类型、信息类型、诊断类型和属性](../../../odbc/reference/develop-app/driver-specific-data-types-descriptor-information-diagnostic.md)。  
  
|Attribute|*将 valueptr* 内容|  
|---------------|-------------------------|  
|SQL_ATTR_APP_PARAM_DESC (ODBC 3.0) |对语句句柄上的 **SQLExecute** 和 **SQLExecDirect** 的后续调用的 APD 的句柄。 此属性的初始值是最初分配语句时隐式分配的说明符。 如果此特性的值设置为 SQL_NULL_DESC 或最初为描述符分配的句柄，则以前与语句句柄关联的显式分配的 APD 句柄与该句柄关联，并将其还原为隐式分配的 APD 句柄。<br /><br /> 不能将此属性设置为为另一语句或在同一语句上隐式设置的另一个描述符句柄隐式分配的描述符句柄;隐式分配的描述符句柄不能与多个语句或描述符句柄关联。|  
|SQL_ATTR_APP_ROW_DESC (ODBC 3.0) |用于后续语句句柄上的 ARD 的句柄。 此属性的初始值是最初分配语句时隐式分配的说明符。 如果此特性的值设置为 SQL_NULL_DESC 或最初为描述符分配的句柄，则以前与语句句柄关联的显式分配的 ARD 句柄与该句柄关联，并将其还原为隐式分配的 ARD 句柄。<br /><br /> 不能将此属性设置为为另一语句或在同一语句上隐式设置的另一个描述符句柄隐式分配的描述符句柄;隐式分配的描述符句柄不能与多个语句或描述符句柄关联。|  
|SQL_ATTR_ASYNC_ENABLE (ODBC 1.0) |一个 SQLULEN 生成值，该值指定是否异步执行用指定语句调用的函数：<br /><br /> SQL_ASYNC_ENABLE_OFF = Disable 语句级别的异步执行支持 (默认) 。<br /><br /> SQL_ASYNC_ENABLE_ON = Enable 语句级别的异步执行支持。<br /><br /> 有关详细信息，请参阅 [异步执行 (轮询方法) ](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)。<br /><br /> 对于具有语句级别异步执行支持的驱动程序，语句属性 SQL_ATTR_ASYNC_ENABLE 为只读。 它的值与分配语句句柄时具有相同名称的连接级别属性的值相同。<br /><br /> 当 SQL_ASYNC_MODE *InfoType* 返回 SQL_AM_CONNECTION 返回 SQLSTATE (HYC00 未实现) 的可选功能时，调用 **SQLSetStmtAttr** 以设置 SQL_ATTR_ASYNC_ENABLE。 有关详细信息，请参阅 [SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md) 。|  
|SQL_ATTR_ASYNC_STMT_EVENT (ODBC 3.8) |作为事件句柄的 SQLPOINTER 值。<br /><br /> 异步函数的完成通知是通过调用 **SQLSetStmtAttr** 来设置 **SQL_ATTR_ASYNC_STMT_EVENT** 特性并指定事件句柄而启用的。|  
|SQL_ATTR_ASYNC_STMT_PCALLBACK (ODBC 3.8) |异步回调函数的 SQLPOINTER。<br /><br /> 只有驱动程序管理器才能使用此属性调用驱动程序的 **SQLSetStmtAttr** 函数。|  
|SQL_ATTR_ASYNC_STMT_PCONTEXT (ODBC 3.8) |上下文结构的 SQLPOINTER<br /><br /> 只有驱动程序管理器才能使用此属性调用驱动程序的 **SQLSetStmtAttr** 函数。|  
|SQL_ATTR_CONCURRENCY (ODBC 2.0) |指定游标并发的 SQLULEN 生成值：<br /><br /> SQL_CONCUR_READ_ONLY = 游标是只读的。 不允许更新。<br /><br /> SQL_CONCUR_LOCK = 游标使用足以确保行可更新的最低锁定级别。<br /><br /> SQL_CONCUR_ROWVER = 游标使用乐观并发控制，比较行版本（如 SQLBase ROWID 或 Sybase TIMESTAMP）。<br /><br /> SQL_CONCUR_VALUES = 游标使用乐观并发控制，比较值。<br /><br /> SQL_ATTR_CONCURRENCY 的默认值为 SQL_CONCUR_READ_ONLY。<br /><br /> 不能为打开的游标指定此属性。 有关详细信息，请参阅 [并发类型](../../../odbc/reference/develop-app/concurrency-types.md)。<br /><br /> 如果 SQL_ATTR_CURSOR_TYPE *特性* 更改为不支持 SQL_ATTR_CONCURRENCY 的当前值的类型，则在执行时将更改 SQL_ATTR_CONCURRENCY 的值，并在调用 **SQLExecDirect** 或 **SQLPrepare** 时发出警告。<br /><br /> 如果驱动程序支持 **SELECT FOR UPDATE** 语句，并且在 SQL_ATTR_CONCURRENCY 的值设置为 SQL_CONCUR_READ_ONLY 时执行此类语句，则将返回错误。 如果 SQL_ATTR_CONCURRENCY 的值更改为驱动程序为 SQL_ATTR_CURSOR_TYPE 的某些值（而不是 SQL_ATTR_CURSOR_TYPE 的当前值）所支持的值，则在执行时将更改 SQL_ATTR_CURSOR_TYPE 的值，并在调用 **SQLExecDirect** 或 **SQLPREPARE** 时发出 SQLSTATE 01S02 (选项值更改) 。<br /><br /> 如果数据源不支持指定的并发，则驱动程序将替换不同的并发，并返回 SQLSTATE 01S02 (选项值已更改) 。 对于 SQL_CONCUR_VALUES，驱动程序将替换 SQL_CONCUR_ROWVER，反之亦然。 对于 SQL_CONCUR_LOCK，驱动程序按顺序替换 SQL_CONCUR_ROWVER 或 SQL_CONCUR_VALUES。 在执行时间之前，不会检查替换值的有效性。<br /><br /> 有关 SQL_ATTR_CONCURRENCY 和其他游标属性之间的关系的详细信息，请参阅 [Cursor 特性和游标类型](../../../odbc/reference/develop-app/cursor-characteristics-and-cursor-type.md)。|  
|SQL_ATTR_CURSOR_SCROLLABLE (ODBC 3.0) |指定应用程序所需的支持级别的 SQLULEN 生成值。 设置此属性将影响对 **SQLExecDirect** 和 **SQLExecute** 的后续调用。<br /><br /> SQL_NONSCROLLABLE = 语句句柄上不需要可滚动游标。 如果应用程序在此句柄上调用 **SQLFetchScroll** ，则唯一的有效值为 *FetchOrientation* SQL_FETCH_NEXT。 这是默认值。<br /><br /> SQL_SCROLLABLE = 语句句柄上需要可滚动游标。 在调用 **SQLFetchScroll** 时，应用程序可以指定任何有效值为 *FetchOrientation*，从而实现除顺序模式之外的其他模式下的游标定位。<br /><br /> 有关可滚动游标的详细信息，请参阅可 [滚动游标](../../../odbc/reference/develop-app/scrollable-cursors.md)。 有关 SQL_ATTR_CURSOR_SCROLLABLE 和其他游标属性之间的关系的详细信息，请参阅 [Cursor 特性和游标类型](../../../odbc/reference/develop-app/cursor-characteristics-and-cursor-type.md)|  
|SQL_ATTR_CURSOR_SENSITIVITY (ODBC 3.0) |一个 SQLULEN 生成值，指定语句句柄上的游标是否会使另一个游标对结果集所做的更改可见。 设置此属性将影响对 **SQLExecDirect** 和 **SQLExecute** 的后续调用。 应用程序可以读取此属性的值，以获取其初始状态或它在应用程序最近设置的状态。<br /><br /> SQL_UNSPECIFIED = 未指定游标类型是什么，语句句柄上的游标是否会使另一个游标对结果集所做的更改可见。 语句句柄上的游标可能会使无、部分或所有此类更改可见。 这是默认值。<br /><br /> SQL_INSENSITIVE = 语句句柄上的所有游标都显示结果集，而不反映任何其他游标对其所做的任何更改。 不敏感游标是只读的。 这对应于静态游标，该游标的并发性为只读。<br /><br /> SQL_SENSITIVE = 语句句柄上的所有游标都使另一个游标对结果集进行的所有更改都可见。<br /><br /> 有关 SQL_ATTR_CURSOR_SENSITIVITY 和其他游标属性之间的关系的详细信息，请参阅 [Cursor 特性和游标类型](../../../odbc/reference/develop-app/cursor-characteristics-and-cursor-type.md)。|  
|SQL_ATTR_CURSOR_TYPE (ODBC 2.0) |指定游标类型的 SQLULEN 生成值：<br /><br /> SQL_CURSOR_FORWARD_ONLY = 游标只向前滚动。<br /><br /> SQL_CURSOR_STATIC = 结果集中的数据是静态的。<br /><br /> SQL_CURSOR_KEYSET_DRIVEN = 驱动程序保存并使用 SQL_ATTR_KEYSET_SIZE 语句特性中指定的行数的键。<br /><br /> SQL_CURSOR_DYNAMIC = 驱动程序只保存行集中的行并使用这些行的键。<br /><br /> 默认值为 SQL_CURSOR_FORWARD_ONLY。 在准备好 SQL 语句后，不能指定此属性。<br /><br /> 如果数据源不支持指定的游标类型，则驱动程序将替换不同的游标类型，并返回 SQLSTATE 01S02 (选项值已更改) 。 对于混合或动态游标，驱动程序将按顺序替代由键集驱动的游标或静态游标。 对于由键集驱动的游标，驱动程序将替换静态游标。<br /><br /> 有关可滚动游标类型的详细信息，请参阅可 [滚动游标类型](../../../odbc/reference/develop-app/scrollable-cursor-types.md)。 有关 SQL_ATTR_CURSOR_TYPE 和其他游标属性之间的关系的详细信息，请参阅 [Cursor 特性和游标类型](../../../odbc/reference/develop-app/cursor-characteristics-and-cursor-type.md)。|  
|SQL_ATTR_ENABLE_AUTO_IPD (ODBC 3.0) |一个 SQLULEN 生成值，该值指定是否执行 IPD 的自动填充：<br /><br /> SQL_TRUE = 在调用 **SQLPrepare** 后启用 IPD 的自动填充。 SQL_FALSE = 在调用 **SQLPrepare** 后关闭 IPD 的自动填充。  (如果支持，应用程序仍可以通过调用 **SQLDescribeParam** 来获取 IPD 字段信息。 ) SQL_FALSE 的语句属性 SQL_ATTR_ENABLE_AUTO_IPD 的默认值。 有关详细信息，请参阅 [IPD 的自动填充](../../../odbc/reference/develop-app/automatic-population-of-the-ipd.md)。|  
|SQL_ATTR_FETCH_BOOKMARK_PTR (ODBC 3.0) |\*指向二进制书签值的 SQLLEN。 当调用 **SQLFetchScroll** 等于 SQL_FETCH_BOOKMARK 的 *fFetchOrientation* 时，驱动程序将从此字段选取书签值。 此字段默认为 null 指针。 有关详细信息，请参阅 [按书签滚动](../../../odbc/reference/develop-app/scrolling-by-bookmark.md)。<br /><br /> 此字段指向的值不用于按书签删除、按书签更新或在 **SQLBulkOperations** 中通过书签操作提取，后者使用在行集缓冲区中缓存的书签。|  
|SQL_ATTR_IMP_PARAM_DESC (ODBC 3.0) |IPD 的句柄。 此属性的值是最初分配语句时所分配的描述符。 应用程序不能设置此属性。<br /><br /> 此属性可通过调用 **SQLGetStmtAttr** 进行检索，但不能通过调用 **SQLSetStmtAttr** 进行设置。|  
|SQL_ATTR_IMP_ROW_DESC (ODBC 3.0) |IRD 的句柄。 此属性的值是最初分配语句时所分配的描述符。 应用程序不能设置此属性。<br /><br /> 此属性可通过调用 **SQLGetStmtAttr** 进行检索，但不能通过调用 **SQLSetStmtAttr** 进行设置。|  
|SQL_ATTR_KEYSET_SIZE (ODBC 2.0) |指定由键集驱动的游标的键集中的行数的 SQLULEN 生成。 如果键集大小为 0 (默认值) ，则游标是完全键集驱动的。 如果键集大小大于0，则游标会混合 (键集在键集内驱动，并在键集) 外动态。 默认键集大小为0。 有关由键集驱动的游标的详细信息，请参阅由 [键集驱动的游标](../../../odbc/reference/develop-app/keyset-driven-cursors.md)。<br /><br /> 如果指定的大小超过最大键集大小，则驱动程序将替换该大小并返回 SQLSTATE 01S02 (选项值已更改) 。<br /><br /> 如果键集大小大于0且小于行集大小，则 **SQLFetch** 或 **SQLFetchScroll** 将返回错误。|  
|SQL_ATTR_MAX_LENGTH (ODBC 1.0) |一个 SQLULEN 生成值，指定驱动程序从字符列或二进制列返回的最大数据量。 如果 *将 valueptr* 小于可用数据的长度，则 **SQLFetch** 或 **SQLGetData** 将截断数据并返回 SQL_SUCCESS。 如果 *将 valueptr* 为 0 (默认) ，则驱动程序将尝试返回所有可用的数据。<br /><br /> 如果指定的长度小于数据源可以返回的最小数据量或大于数据源可以返回的最大数据量，则驱动程序将替换该值并返回 SQLSTATE 01S02 (选项值已更改) 。<br /><br /> 可以在打开的游标上设置此属性的值;但是，该设置可能不会立即生效，在这种情况下，驱动程序将返回 SQLSTATE 01S02 (选项值已更改) 并将该属性重置为其原始值。<br /><br /> 此属性旨在减少网络流量，并且仅当数据源 (与多层驱动程序中的驱动程序) 可以实现时才受支持。 应用程序不应使用此机制来截断数据;若要截断接收的数据，应用程序应在 **SQLBindCol** 或 **SQLGetData** 的 *BufferLength* 参数中指定最大缓冲区长度。|  
|SQL_ATTR_MAX_ROWS (ODBC 1.0) |与为 **SELECT** 语句返回到应用程序的最大行数对应的 sqlulen 生成值。 如果 \* *将 valueptr* 等于 0 (默认值) ，则驱动程序将返回所有行。<br /><br /> 此属性旨在减少网络流量。 从概念上讲，此方法在创建结果集时应用，并将结果集限制为第一 *将 valueptr* 行。 如果结果集中的行数大于 *将 valueptr*，则结果集将被截断。<br /><br /> SQL_ATTR_MAX_ROWS 适用于 *语句* 中的所有结果集，包括目录函数返回的结果集。 SQL_ATTR_MAX_ROWS 为游标行计数的值建立最大值。<br /><br /> 如果不能在数据源中实现结果集大小限制，驱动程序不应为 **SQLFetch** 或 **SQLFetchScroll** (模拟 SQL_ATTR_MAX_ROWS 行为) 如果无法保证 SQL_ATTR_MAX_ROWS 将正确实现。<br /><br /> 它是由驱动程序定义的，无论 SQL_ATTR_MAX_ROWS 应用于 SELECT 语句以外的语句 (例如) 目录函数。<br /><br /> 可以在打开的游标上设置此属性的值;但是，该设置可能不会立即生效，在这种情况下，驱动程序将返回 SQLSTATE 01S02 (选项值已更改) 并将该属性重置为其原始值。|  
|SQL_ATTR_METADATA_ID (ODBC 3.0) |一个 SQLULEN 生成值，确定如何处理目录函数的字符串参数。<br /><br /> 如果 SQL_TRUE，则将目录函数的字符串参数作为标识符处理。 这种情况并不重要。 对于 nondelimited 字符串，驱动程序将删除任何尾随空格，并将该字符串折叠为大写。 对于带分隔符的字符串，驱动程序将删除任何前导空格或尾随空格，并按字面区分分隔符。 如果将其中一个参数设置为 null 指针，则该函数将返回 SQL_ERROR 和 SQLSTATE HY009 (使用 null 指针) 无效。<br /><br /> 如果 SQL_FALSE，则不会将目录函数的字符串自变量作为标识符处理。 这种情况很重要。 它们可以包含字符串搜索模式，具体取决于参数。<br /><br /> 默认值为 SQL_FALSE。<br /><br /> 使用值列表的 **SQLTables** 的 *TableType* 参数不受此特性的影响。<br /><br /> 还可以对连接级别设置 SQL_ATTR_METADATA_ID。  (它和 SQL_ATTR_ASYNC_ENABLE 是同样是连接特性的语句特性。 ) <br /><br /> 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。|  
|SQL_ATTR_NOSCAN (ODBC 1.0) |一个 SQLULEN 生成值，该值指示驱动程序是否应扫描 SQL 字符串中的转义序列：<br /><br /> SQL_NOSCAN_OFF = 驱动程序 (默认) 为转义序列扫描 SQL 字符串。<br /><br /> SQL_NOSCAN_ON = 驱动程序不扫描 SQL 字符串是否有转义序列。 相反，驱动程序将语句直接发送到数据源。<br /><br /> 有关详细信息，请参阅 [ODBC 中的转义序列](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md)。|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR (ODBC 3.0) |一个 SQLULEN 生成 * 值，它指向添加到指针的偏移量，以更改动态参数的绑定。 如果此字段为非 null，则驱动程序将取消引用指针，将取消引用的值添加到描述符记录中每个延迟字段 (SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR) ，并在绑定时使用新的指针值。 默认情况下，它设置为 null。<br /><br /> 绑定偏移量始终直接添加到 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 字段。 如果偏移量更改为不同的值，则新值仍会直接添加到 "描述符" 字段中的值。 新偏移量不会添加到字段值和任何以前的偏移量。<br /><br /> 有关详细信息，请参阅 [参数绑定偏移量](../../../odbc/reference/develop-app/parameter-binding-offsets.md)。<br /><br /> 设置此语句特性将在 APD 标头中设置 SQL_DESC_BIND_OFFSET_PTR 字段。|  
|SQL_ATTR_PARAM_BIND_TYPE (ODBC 3.0) |一个 SQLULEN 生成值，指示用于动态参数的绑定方向。<br /><br /> 此字段设置为 SQL_PARAM_BIND_BY_COLUMN (默认) 来选择按列绑定。<br /><br /> 若要选择按行绑定，请将此字段设置为将绑定到一组动态参数的结构或缓冲区的长度。 此长度必须包含所有绑定参数的空间以及结构或缓冲区的任何填充，以确保当绑定参数的地址递增指定的长度时，结果将指向下一组参数中的相同参数的开头。 在 ANSI C 中使用 *sizeof* 运算符时，此行为是保证的。<br /><br /> 有关详细信息，请参阅 [绑定参数数组](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md)。<br /><br /> 设置此语句特性将在 APD 标头中设置 SQL_DESC_ BIND_TYPE 字段。|  
|SQL_ATTR_PARAM_OPERATION_PTR (ODBC 3.0) |一个 SQLUSMALLINT \* 值，该值指向一个 SQLUSMALLINT 值数组，这些值用于在 SQL 语句执行过程中忽略参数。 每个值都设置为要执行的参数 SQL_PARAM_PROCEED () 或 SQL_PARAM_IGNORE () 的参数。<br /><br /> 可以在处理过程中忽略一组参数，方法是在 APD 中通过将状态值设置为 SQL_PARAM_IGNORE SQL_DESC_ARRAY_STATUS_PTR。 如果将参数的 status 值设置为 SQL_PARAM_PROCEED 或数组中没有元素，则会处理一组参数。<br /><br /> 此语句属性可以设置为 null 指针，在这种情况下，驱动程序不返回参数状态值。 此属性可随时设置，但在下一次调用 **SQLExecDirect** 或 **SQLExecute** 之前，不会使用新值。<br /><br /> 如果没有绑定参数，则忽略此属性。<br /><br /> 有关详细信息，请参阅 [使用参数数组](../../../odbc/reference/develop-app/using-arrays-of-parameters.md)。<br /><br /> 设置此语句特性将在 APD 标头中设置 SQL_DESC_ARRAY_STATUS_PTR 字段。|  
|SQL_ATTR_PARAM_STATUS_PTR (ODBC 3.0) |一个 SQLUSMALLINT \* 值，该值指向在调用 **SQLExecute** 或 **SQLExecDirect** 后包含参数值的每一行的状态信息的 SQLUSMALLINT 值的数组。 仅当 PARAMSET_SIZE 大于1时，此字段才是必需的。<br /><br /> 状态值可以包含以下值：<br /><br /> SQL_PARAM_SUCCESS：已成功为此参数集执行 SQL 语句。<br /><br /> SQL_PARAM_SUCCESS_WITH_INFO：已成功为此参数集执行 SQL 语句;但是，诊断数据结构中提供了警告信息。<br /><br /> SQL_PARAM_ERROR：处理此参数集时出错。 诊断数据结构中提供了其他错误信息。<br /><br /> SQL_PARAM_UNUSED：未使用此参数集，这可能是由于某些先前的参数集导致了导致进一步处理终止的错误，或者是因为在 SQL_ATTR_PARAM_OPERATION_PTR 指定的数组中为该参数集设置了 SQL_PARAM_IGNORE。<br /><br /> SQL_PARAM_DIAG_UNAVAILABLE：驱动程序将参数数组视为单个单元，因此不会生成此级别的错误信息。<br /><br /> 此语句属性可以设置为 null 指针，在这种情况下，驱动程序不返回参数状态值。 此属性可随时设置，但在下一次调用 **SQLExecute** 或 **SQLExecDirect** 之前，不会使用新值。 请注意，设置此属性会影响驱动程序实现的输出参数行为。<br /><br /> 有关详细信息，请参阅 [使用参数数组](../../../odbc/reference/develop-app/using-arrays-of-parameters.md)。<br /><br /> 设置此语句属性将设置 IPD 标头中的 "SQL_DESC_ARRAY_STATUS_PTR" 字段。|  
|SQL_ATTR_PARAMS_PROCESSED_PTR (ODBC 3.0) |一个 SQLULEN 生成 \* 记录字段，该字段指向一个缓冲区，该缓冲区用于返回已处理的参数集的数目，包括错误集。 如果为 null 指针，则不返回任何数字。<br /><br /> 设置此语句属性将设置 IPD 标头中的 "SQL_DESC_ROWS_PROCESSED_PTR" 字段。<br /><br /> 如果对 **SQLExecDirect** 或 **SQLExecute** 的调用，而此属性所指向的缓冲区未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则缓冲区的内容不确定。<br /><br /> 有关详细信息，请参阅 [使用参数数组](../../../odbc/reference/develop-app/using-arrays-of-parameters.md)。|  
|SQL_ATTR_PARAMSET_SIZE (ODBC 3.0) |一个 SQLULEN 生成值，该值指定每个参数的值的数目。 如果 SQL_ATTR_PARAMSET_SIZE 大于1，则将 APD 点 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 为数组。 每个数组的基数等于此字段的值。<br /><br /> 如果没有绑定参数，则忽略此属性。<br /><br /> 有关详细信息，请参阅 [使用参数数组](../../../odbc/reference/develop-app/using-arrays-of-parameters.md)。<br /><br /> 设置此语句特性将在 APD 标头中设置 SQL_DESC_ARRAY_SIZE 字段。|  
|SQL_ATTR_QUERY_TIMEOUT (ODBC 1.0) |与在返回应用程序之前等待 SQL 语句执行的秒数对应的 SQLULEN 生成值。 如果 *将 valueptr* 等于 0 (默认值) ，则没有超时。<br /><br /> 如果指定的超时值超过了数据源中的最大超时值或小于最小超时值，则 **SQLSetStmtAttr** 将替换该值并返回 SQLSTATE 01S02 (选项值已更改) 。<br /><br /> 请注意，如果 **SELECT** 语句超时，应用程序无需调用 **SQLCloseCursor** 即可重复使用该语句。<br /><br /> 此语句属性中的查询超时设置在同步和异步模式下都有效。|  
|SQL_ATTR_RETRIEVE_DATA (ODBC 2.0) |SQLULEN 生成值：<br /><br /> SQL_RD_ON = **SQLFetchScroll** ， *在 ODBC 3.X 中，* **SQLFetch** 在将游标定位到指定位置之后检索数据。 这是默认值。<br /><br /> SQL_RD_OFF = **SQLFetchScroll** ， *在 ODBC 3.X 中，* **SQLFetch** 不在定位游标后检索数据。<br /><br /> 通过将 SQL_RETRIEVE_DATA 设置为 SQL_RD_OFF，应用程序可以验证行是否存在，也可以检索行的书签，而不会产生检索行的开销。 有关详细信息，请参阅 [滚动和提取行](../../../odbc/reference/develop-app/scrolling-and-fetching-rows-odbc.md)。<br /><br /> 可以在打开的游标上设置此属性的值;但是，该设置可能不会立即生效，在这种情况下，驱动程序将返回 SQLSTATE 01S02 (选项值已更改) 并将该属性重置为其原始值。|  
|SQL_ATTR_ROW_ARRAY_SIZE (ODBC 3.0) |一个 SQLULEN 生成值，指定每次调用 **SQLFetch** 或 **SQLFetchScroll** 时返回的行数。 它也是在 **SQLBulkOperations** 中批量书签操作中使用的书签数组中的行数。 默认值为 1。<br /><br /> 如果指定的行集大小超过了数据源支持的最大行集大小，则驱动程序将替换该值，并返回 SQLSTATE 01S02 (选项值已更改) 。<br /><br /> 有关详细信息，请参阅 [行集大小](../../../odbc/reference/develop-app/rowset-size.md)。<br /><br /> 设置此语句特性将在 ARD 标头中设置 SQL_DESC_ARRAY_SIZE 字段。|  
|SQL_ATTR_ROW_BIND_OFFSET_PTR (ODBC 3.0) |一个 SQLULEN 生成 * 值，它指向添加到指针的偏移量，以更改列数据的绑定。 如果此字段为非 null，则驱动程序将取消引用指针，将取消引用的值添加到描述符记录中每个延迟字段 (SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR) ，并在绑定时使用新的指针值。 默认情况下，它设置为 null。<br /><br /> 设置此语句特性将在 ARD 标头中设置 SQL_DESC_BIND_OFFSET_PTR 字段。|  
|SQL_ATTR_ROW_BIND_TYPE (ODBC 1.0) |一个 SQLULEN 生成值，用于设置在关联语句上调用 **SQLFetch** 或 **SQLFetchScroll** 时使用的绑定方向。 通过将值设置为 SQL_BIND_BY_COLUMN 来选择按列绑定。 通过将值设置为结构的长度或结果列将绑定到的缓冲区的实例，可选择按行绑定。<br /><br /> 如果指定了长度，则它必须包含所有绑定列以及结构或缓冲区的任何填充的空间，以确保当绑定列的地址递增指定的长度时，结果将指向下一行中同一列的开头。 在 ANSI C 中将 **sizeof** 运算符与结构或联合一起使用时，此行为是保证的。<br /><br /> 按列绑定是 **SQLFetch** 和 **SQLFetchScroll** 的默认绑定方向。<br /><br /> 有关详细信息，请参阅用于 [阻止游标的绑定列](../../../odbc/reference/develop-app/binding-columns-for-use-with-block-cursors.md)。<br /><br /> 设置此语句特性将在 ARD 标头中设置 SQL_DESC_BIND_TYPE 字段。|  
|SQL_ATTR_ROW_NUMBER (ODBC 2.0) |一个 SQLULEN 生成值，该值是整个结果集中的当前行数。 如果无法确定当前行的数目或没有当前行，则驱动程序将返回0。<br /><br /> 此属性可通过调用 **SQLGetStmtAttr** 进行检索，但不能通过调用 **SQLSetStmtAttr** 进行设置。|  
|SQL_ATTR_ROW_OPERATION_PTR (ODBC 3.0) |一个 SQLUSMALLINT \* 值，该值指向 SQLUSMALLINT 值的数组，这些值用于在使用 **SQLSetPos** 时使用的大容量操作中忽略行。 每个值都设置为要包含在大容量操作中的行 SQL_ROW_PROCEED () 或 SQL_ROW_IGNORE (从大容量操作) 中排除的行。 在对 **SQLBulkOperations** 的调用期间，不能使用此数组来忽略 (行。 ) <br /><br /> 此语句属性可以设置为 null 指针，在这种情况下，驱动程序不返回行状态值。 此属性可随时设置，但在下一次调用 **SQLSetPos** 之前，不会使用新值。<br /><br /> 有关详细信息，请参阅 [通过 SQLSetPos 更新行集中的行](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md) 和 [通过 SQLSetPos 删除行集中的行](../../../odbc/reference/develop-app/deleting-rows-in-the-rowset-with-sqlsetpos.md)。<br /><br /> 设置此语句特性将在 ARD 中设置 SQL_DESC_ARRAY_STATUS_PTR 字段。|  
|SQL_ATTR_ROW_STATUS_PTR (ODBC 3.0) |一个 SQLUSMALLINT \* 值，该值指向 SQLUSMALLINT 值的数组，这些值包含调用 **SQLFetch** 或 **SQLFetchScroll** 后的行状态值。 数组中的元素数目与行集中的行数相同。<br /><br /> 此语句属性可以设置为 null 指针，在这种情况下，驱动程序不返回行状态值。 此属性可随时设置，但在下一次调用 **SQLBulkOperations**、 **SQLFetch**、 **SQLFetchScroll** 或 **SQLSetPos** 之前，不会使用新值。<br /><br /> 有关详细信息，请参阅 [提取的行数和状态](../../../odbc/reference/develop-app/number-of-rows-fetched-and-status.md)。<br /><br /> 设置此语句特性将在 IRD 标头中设置 SQL_DESC_ARRAY_STATUS_PTR 字段。<br /><br /> 在对 **SQLExtendedFetch** 的调用中，此属性 *由 ODBC 2.x* 驱动程序映射到 *rgbRowStatus* 数组。|  
|SQL_ATTR_ROWS_FETCHED_PTR (ODBC 3.0) |一个 SQLULEN 生成 \* 值，它指向一个缓冲区，返回调用 **SQLFetch** 或 **SQLFetchScroll** 后提取的行数; 由使用 SQL_REFRESH 的 *操作* 参数执行的 **SQLSetPos** 所影响的行数，或由 **SQLBulkOperations** 执行的大容量操作影响的行数。 此数字包含错误行。<br /><br /> 有关详细信息，请参阅 [提取的行数和状态](../../../odbc/reference/develop-app/number-of-rows-fetched-and-status.md)。<br /><br /> 设置此语句特性将在 IRD 标头中设置 SQL_DESC_ROWS_PROCESSED_PTR 字段。<br /><br /> 如果对 **SQLFetch** 或 **SQLFetchScroll** 的调用，而此属性所指向的缓冲区未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则缓冲区的内容不确定。|  
|SQL_ATTR_SIMULATE_CURSOR (ODBC 2.0) |一个 SQLULEN 生成值，指定模拟定位的 update 和 delete 语句的驱动程序是否保证此类语句仅影响一行。<br /><br /> 若要模拟定位的 update 和 delete 语句，大多数驱动程序将构造一个包含 **WHERE** 子句的搜索到的 **update** 或 **delete** 语句，该语句指定当前行中每列的值。 除非这些列构成唯一键，否则，此类语句会影响多个行。<br /><br /> 为了保证此类语句仅影响一行，驱动程序将确定唯一键中的列，并将这些列添加到结果集中。 如果应用程序保证结果集中的列构成唯一键，则不需要该驱动程序。 这可能会缩短执行时间。<br /><br /> SQL_SC_NON_UNIQUE = 驱动程序不保证模拟定位的 update 或 delete 语句仅影响一行;应用程序负责执行此操作。 如果语句影响多个行，则 **SQLExecute**、 **SQLExecDirect** 或 **SQLSetPos** 返回 SQLSTATE 01001 (游标操作冲突) 。<br /><br /> SQL_SC_TRY_UNIQUE = 驱动程序尝试保证模拟定位的 update 或 delete 语句仅影响一行。 驱动程序始终执行此类语句，即使它们可能会影响多个行（如没有唯一键时）也是如此。 如果语句影响多个行，则 **SQLExecute**、 **SQLExecDirect** 或 **SQLSetPos** 返回 SQLSTATE 01001 (游标操作冲突) 。<br /><br /> SQL_SC_UNIQUE = 驱动程序保证模拟定位的 update 或 delete 语句仅影响一行。 如果驱动程序无法保证给定语句的这种情况，则 **SQLExecDirect** 或 **SQLPrepare** 将返回错误。<br /><br /> 如果数据源为定位的 update 和 delete 语句提供本机 SQL 支持并且驱动程序不模拟游标，则当 SQL_SC_UNIQUE 请求 SQL_SIMULATE_CURSOR 时，将返回 SQL_SUCCESS。 如果请求 SQL_SC_TRY_UNIQUE 或 SQL_SC_NON_UNIQUE，则返回 SQL_SUCCESS_WITH_INFO。 如果数据源提供了 SQL_SC_TRY_UNIQUE 级别的支持，但驱动程序没有，则为 SQL_SC_TRY_UNIQUE 返回 SQL_SUCCESS，并为 SQL_SC_NON_UNIQUE 返回 SQL_SUCCESS_WITH_INFO。<br /><br /> 如果数据源不支持指定的游标模拟类型，则驱动程序将替换不同的模拟类型，并返回 SQLSTATE 01S02 (选项值已更改) 。 对于 SQL_SC_UNIQUE，驱动程序按顺序替换 SQL_SC_TRY_UNIQUE 或 SQL_SC_NON_UNIQUE。 对于 SQL_SC_TRY_UNIQUE，驱动程序 SQL_SC_NON_UNIQUE 替换。<br /><br /> 默认值为 SQL_SC_UNIQUE。<br /><br /> 有关详细信息，请参阅 [模拟定位的 Update 和 Delete 语句](../../../odbc/reference/develop-app/simulating-positioned-update-and-delete-statements.md)。|  
|SQL_ATTR_USE_BOOKMARKS (ODBC 2.0) |一个 SQLULEN 生成值，该值指定应用程序是否将书签与光标一起使用：<br /><br /> SQL_UB_OFF = Off (默认) <br /><br /> SQL_UB_VARIABLE = 应用程序将书签与光标一起使用，并且驱动程序将提供长度可变的书签（如果支持）。 ODBC 2.x 中已弃用 *SQL_UB_FIXED。* ODBC *2.x 应用程序* 应始终使用可变长度书签，即使在 *使用 odbc 2.x* 驱动程序时， (仅支持4个字节的固定长度书签) 。 这是因为，固定长度的书签只是可变长度书签的特例。 使用 ODBC 2.x 驱动程序时， *驱动程序管理* 器会将 SQL_UB_VARIABLE 映射到 SQL_UB_FIXED。<br /><br /> 若要在游标中使用书签，应用程序必须在打开游标前使用 SQL_UB_VARIABLE 值指定此属性。<br /><br /> 有关详细信息，请参阅 [检索书签](../../../odbc/reference/develop-app/retrieving-bookmarks.md)。|  
  
 [1] 仅当描述符是实现描述符，而不是应用程序描述符时，才能以异步方式调用这些函数。  
  
 请参阅按 [列绑定](../../../odbc/reference/develop-app/column-wise-binding.md) 和按 [行绑定](../../../odbc/reference/develop-app/row-wise-binding.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回连接属性的设置|[SQLGetConnectAttr 函数](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|返回语句特性的设置|[SQLGetStmtAttr 函数](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|设置连接属性|[SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|设置描述符的单个字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
