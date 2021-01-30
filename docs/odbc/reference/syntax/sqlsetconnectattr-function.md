---
description: SQLSetConnectAttr 函数
title: SQLSetConnectAttr 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConnectAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConnectAttr
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC]
ms.assetid: 97fc7445-5a66-4eb9-8e77-10990b5fd685
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06935fe7eca6f19a8c6fcc052cd3a9503144a296
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192451"
---
# <a name="sqlsetconnectattr-function"></a>SQLSetConnectAttr 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLSetConnectAttr** 设置控制连接各个方面的属性。  
  
> [!NOTE]
>  有关 ODBC 3.x 应用程序使用 ODBC 2.x *驱动程序时*，驱动程序管理器将此函数映射到的内容的详细信息，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLSetConnectAttr(  
     SQLHDBC       ConnectionHandle,  
     SQLINTEGER    Attribute,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    StringLength);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *Attribute*  
 送要设置的属性，在 "注释" 中列出。  
  
 *ValuePtr*  
 送指向要与 *属性* 关联的值的指针。 根据 *属性* 的值， *将 valueptr* 将是无符号整数值，或者将指向以 null 值结束的字符串。 请注意， *特性* 自变量的整数类型不能固定长度，请参阅注释部分了解详细信息。  
  
 *StringLength*  
 送如果 *attribute* 是 ODBC 定义的属性，并且 *将 valueptr* 指向字符串或二进制缓冲区，则此参数应为 **将 valueptr* 的长度。 对于字符串数据，此参数应包含字符串中的字节数。  
  
 如果 *attribute* 是一个 ODBC 定义的属性，并且 *将 valueptr* 是一个整数，则将忽略 *StringLength* 。  
  
 如果 *attribute* 是驱动程序定义的属性，则应用程序通过设置 *StringLength* 参数来指示属性对驱动程序管理器的性质。 *StringLength* 可具有以下值：  
  
-   如果 *将 valueptr* 是指向字符串的指针，则 *StringLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *将 valueptr* 是一个指向二进制缓冲区的指针，则该应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *StringLength* 中。 这会在 *StringLength* 中置入负值。  
  
-   如果 *将 valueptr* 是一个指向字符串或二进制字符串以外的值的指针，则 *StringLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *将 valueptr* 包含固定长度的值，则 *StringLength* 根据需要 SQL_IS_INTEGER 或 SQL_IS_UINTEGER。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_STILL_EXECUTING。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetConnectAttr** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSetConnectAttr** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
 驱动程序可以返回 SQL_SUCCESS_WITH_INFO 以提供有关设置选项的结果的信息。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|该驱动程序不支持在 *将 valueptr* 中指定的值，并将其替换为类似值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08002|连接名称正在使用中|*特性* 参数已 SQL_ATTR_ODBC_CURSORS，驱动程序已连接到数据源。|  
|08003|连接未打开| (DM) 指定的 *属性* 值需要打开的连接，但 *ConnectionHandle* 未处于连接状态。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|*特性* 参数已 SQL_ATTR_CURRENT_CATALOG，结果集处于挂起状态。|  
|25000|在本地事务中的操作非法|尝试通过将连接属性设置为 SQL_ATTR_ENLIST_IN_DTC 在 (DTC) 中登记分布式事务连接时，在本地事务中建立连接。<br /><br /> 已在 DTC 中登记连接。<br /><br /> 已在分布式事务连接中登记了一个连接，并且通过将 SQL_ATTR_AUTOCOMMIT 设置为 SQL_AUTOCOMMIT_OFF 启动了本地事务。|  
|3D000|目录名称无效|*特性* 参数 SQL_CURRENT_CATALOG，指定的目录名称无效。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *ConnectionHandle* 启用异步处理。 调用 **SQLSetConnectAttr** 函数，并在其完成执行之前，在 *ConnectionHandle* 上调用 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)，然后在 *ConnectionHandle* 上再次调用 **SQLSetConnectAttr** 函数。<br /><br /> 或者，调用 **SQLSetConnectAttr** 函数，并在其完成执行之前，从多线程应用程序中的另一个线程调用 *ConnectionHandle* 上的 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效|*特性* 参数标识了需要字符串值的连接属性，而 *将 valueptr* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *ConnectionHandle* 关联的 *StatementHandle* 调用了异步执行的函数，并且在调用 **SQLSetConnectAttr** 时仍在执行。<br /><br />  (DM) 异步执行的函数 (不是为 *ConnectionHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为与 *ConnectionHandle* 关联的其中一个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br /> 为与 *StatementHandle* 关联的 *ConnectionHandle* 调用了 (DM) **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br /> 为 *ConnectionHandle* 调用了 (DM) **SQLBrowseConnect** ，并返回 SQL_NEED_DATA。 此函数在 **SQLBrowseConnect** 返回 SQL_SUCCESS_WITH_INFO 或 SQL_SUCCESS 之前被调用。|  
|HY011|现在无法设置属性|*特性* 参数 SQL_ATTR_TXN_ISOLATION，而事务已打开。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY024|属性值无效|给定指定的 *属性* 值， *将 valueptr* 中指定的值无效。  (，驱动程序管理器只为接受一组离散值（如 SQL_ATTR_ACCESS_MODE 或 SQL_ATTR_ASYNC_ENABLE）的连接和语句返回此 SQLSTATE。 对于所有其他连接和语句属性，驱动程序必须验证 *将 valueptr* 中指定的值。 ) <br /><br /> *特性* 参数为 SQL_ATTR_TRACEFILE 或 SQL_ATTR_TRANSLATE_LIB，*将 valueptr* 为空字符串。|  
|HY090|字符串或缓冲区长度无效|*(DM) \*将 valueptr* 是一个字符串， *StringLength* 参数小于0，但未 SQL_NTS。|  
|HY092|无效的属性/选项标识符| (DM) 为参数 *属性* 指定的值对于该驱动程序支持的 ODBC 版本无效。<br /><br />  (DM) 为参数 *属性* 指定的值为只读属性。|  
|HY114|驱动程序不支持连接级异步函数执行| (DM) 应用程序尝试使用 SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE 为不支持异步连接操作的驱动程序启用异步函数执行。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HY121|不能同时启用游标库和 Driver-Aware 池|有关详细信息，请参阅 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)。|  
|HYC00|未实现的可选功能|为 argument *特性* 指定的值为驱动程序支持的 odbc 版本的有效 odbc 连接或语句属性，但驱动程序不支持该属性。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *ConnectionHandle* 关联的驱动程序不支持该函数。|  
|IM009|无法加载转换 DLL|驱动程序无法加载为连接指定的转换 DLL。 仅当 *属性* SQL_ATTR_TRANSLATE_LIB 时，才能返回此错误。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
|S1118|驱动程序不支持异步通知|SQL_ATTR_ASYNC_DBC_EVENT 在建立连接后 (设置) 但驱动程序不支持异步通知。|  
  
 当 *Attribute* 是语句特性时， **SQLSetConnectAttr** 可以返回 **SQLSetStmtAttr** 返回的任何 SQLSTATEs。  
  
## <a name="comments"></a>注释  
 有关连接属性的常规信息，请参阅 [连接属性](../../../odbc/reference/develop-app/connection-attributes.md)。  
  
 本部分后面的表中显示了当前定义的属性及其引入的 ODBC 版本。应该会定义更多的属性以利用不同的数据源。 ODBC 保留了一系列属性;驱动程序开发人员必须在开放组中保留其自己的特定于驱动程序的使用值。  
  
> [!NOTE]
>  通过调用 **SQLSetConnectAttr** 在连接级别设置语句属性的功能已在 ODBC 2.x 中 *弃用。* ODBC 2.x *应用程序* 决不应在连接级别设置语句属性。 ODBC 2.x *语句特性* 不能在连接级别设置，但 SQL_ATTR_METADATA_ID 和 SQL_ATTR_ASYNC_ENABLE 属性除外，它们都是连接特性和语句特性，可以在连接级别或语句级别设置。  
> 
>  如果 ODBC *2.x 驱动程序* 应使用在连接级别设置 odbc *2.x 语句选项* 的 odbc 2.x 应用程序，则只需支持此功能。 有关详细信息，请参阅附录 G：驱动程序准则中的 [SQLSetConnectOption 映射](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md) ，以实现向后兼容性。  
  
 应用程序可以在每次分配和释放连接之间随时调用 **SQLSetConnectAttr** 。 连接的应用程序成功设置的所有连接和语句属性都将保留，直到在连接上调用 **SQLFreeHandle** 。 例如，如果应用程序在连接到数据源之前调用 **SQLSetConnectAttr** ，则即使在应用程序连接到数据源时，驱动程序中的 **SQLSetConnectAttr** 失败，该属性仍将保持不变;如果应用程序设置了特定于驱动程序的属性，即使应用程序连接到连接上的其他驱动程序，该属性仍将保持不变。  
  
 某些连接属性只能在建立连接之前设置;只有在建立连接后才能设置其他设置。 下表指示在建立连接之前或之后必须设置的连接属性。 *指示属性* 可以在连接之前或之后设置。  
  
|Attribute|是否在连接之前或之后设置？|  
|---------------|-------------------------------------|  
|SQL_ATTR_ACCESS_MODE|[1]|  
|SQL_ATTR_ASYNC_DBC_EVENT|任一个|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE|[4]|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK|任一个|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT|任一个|  
|SQL_ATTR_ASYNC_ENABLE|[2]|  
|SQL_ATTR_AUTO_IPD|任一个|  
|SQL_ATTR_AUTOCOMMIT|[5]|  
|SQL_ATTR_CONNECTION_DEAD|之后|  
|SQL_ATTR_CONNECTION_TIMEOUT|任一个|  
|SQL_ATTR_CURRENT_CATALOG|[1]|  
|SQL_ATTR_DBC_INFO_TOKEN|之后|  
|SQL_ATTR_ENLIST_IN_DTC|之后|  
|SQL_ATTR_LOGIN_TIMEOUT|之前|  
|SQL_ATTR_METADATA_ID|任一个|  
|SQL_ATTR_ODBC_CURSORS|之前|  
|SQL_ATTR_PACKET_SIZE|之前|  
|SQL_ATTR_QUIET_MODE|任一个|  
|SQL_ATTR_TRACE|任一个|  
|SQL_ATTR_TRACEFILE|任一个|  
|SQL_ATTR_TRANSLATE_LIB|之后|  
|SQL_ATTR_TRANSLATE_OPTION|之后|  
|SQL_ATTR_TXN_ISOLATION|[3]|  
  
 [1] 可以在连接之前或之后设置 SQL_ATTR_ACCESS_MODE 和 SQL_ATTR_CURRENT_CATALOG，具体取决于驱动程序。 但是，在连接之前，可互操作的应用程序将其设置，因为某些驱动程序不支持在连接后更改它们。  
  
 必须先设置 [2] SQL_ATTR_ASYNC_ENABLE，才能存在活动语句。  
  
 [3] 仅当连接上没有打开的事务时，才可以设置 SQL_ATTR_TXN_ISOLATION。 如果数据源不支持将 valueptr 中指定的值，某些连接属性支持替换相似的值 \* 。 在这种情况下，驱动程序将返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01S02 (选项值) 更改。 例如，如果 *属性* 为 SQL_ATTR_PACKET_SIZE 并且 \* *将 valueptr* 超出最大数据包大小，则驱动程序将替换最大大小。 若要确定替换值，应用程序将调用 **SQLGetConnectAttr**。  
  
 [4] 如果在打开连接之前设置 SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE，驱动程序管理器将在调用 **SQLBrowseConnect**、 **SQLConnect** 或 **SQLDriverConnect** 的过程中加载驱动程序的属性。 在调用 **SQLBrowseConnect**、 **SQLConnect** 或 **SQLDriverConnect** 之前，驱动程序管理器不知道要连接到哪个驱动程序，也不知道该驱动程序是否支持异步连接操作。 因此，驱动程序管理器始终返回 SQL_SUCCESS。 但如果该驱动程序不支持异步连接操作，则对 **SQLBrowseConnect**、 **SQLConnect** 或 **SQLDriverConnect** 的调用将失败。  
  
 [5] 将 SQL_ATTR_AUTOCOMMIT 设置为 FALSE 时，如果任何 API 返回 SQL_ERROR 以确保事务一致性，应用程序应 (SQL_ROLLBACK) 调用 SQLEndTran。  
  
 将 valueptr 缓冲区中的信息集格式 \* 取决于指定的 *属性*。 **SQLSetConnectAttr** 将接受以下两种不同格式中的一种：以 null 结尾的字符串或整数值。 每个的格式记录在属性的说明中。 **SQLSetConnectAttr** 的 *将 valueptr* 参数指向的字符串的长度为 *StringLength* 个字节。  
  
 如果长度由属性定义，则会忽略 *StringLength* 参数，这与 ODBC *2.x 或更* 早版本中引入的所有属性的情况相同。  
  
|*Attribute*|*将 valueptr* 内容|  
|-----------------|-------------------------|  
|SQL_ATTR_ACCESS_MODE (ODBC 1.0) |SQLUINTEGER 值。 驱动程序或数据源使用 SQL_MODE_READ_ONLY，以指示该连接不是支持导致更新发生的 SQL 语句所必需的。 此模式可用于根据驱动程序或数据源优化锁定策略、事务管理或其他区域。 无需驱动程序即可阻止将此类语句提交到数据源。 当请求在只读连接期间处理不是只读的 SQL 语句时，驱动程序和数据源的行为是由实现定义的。 默认值为 SQL_MODE_READ_WRITE。|  
|SQL_ATTR_ASYNC_DBC_EVENT (ODBC 3.8) |作为事件句柄的 SQLPOINTER 值。<br /><br /> 通过使用 SQL_ATTR_ASYNC_STMT_EVENT 特性调用 **SQLSetConnectAttr** 并指定事件句柄，启用异步函数的完成通知。 **注意：**  游标库不支持通知方法。 如果启用了通知方法，则应用程序将在尝试通过 SQLSetConnectAttr 启用游标库时收到错误消息。|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE (ODBC 3.8) |SQLUINTEGER 值，用于启用或禁用连接句柄上所选函数的异步执行。 有关详细信息，请参阅 [异步执行 (轮询方法) ](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)。<br /><br /> SQL_ASYNC_DBC_ENABLE_ON = 为指定的连接相关函数启用异步操作。<br /><br /> SQL_ASYNC_DBC_ENABLE_OFF = (默认) 禁用指定连接相关函数的异步操作。<br /><br /> 设置 SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE 始终同步 (也就是说，它绝不会返回 SQL_STILL_EXECUTING) 。<br /><br /> 通过 SQL_ATTR_ASYNC_ENABLE 启用语句操作的异步执行。|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK (ODBC 3.8) |指向上下文结构的 SQLPOINTER 值。<br /><br /> 只有驱动程序管理器才能使用此属性调用驱动程序的 **SQLSetStmtAttr** 函数。|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT (ODBC 3.8) |指向上下文结构的 SQLPOINTER 值。<br /><br /> 只有驱动程序管理器才能使用此属性调用驱动程序的 **SQLSetStmtAttr** 函数。|  
|SQL_ATTR_ASYNC_ENABLE (ODBC 3.0) |一个 SQLULEN 生成值，该值指定是否以异步方式执行使用指定连接上的语句调用的函数：<br /><br /> SQL_ASYNC_ENABLE_OFF = 禁用连接级别对语句操作的异步执行支持 (默认) 。<br /><br /> SQL_ASYNC_ENABLE_ON = 启用连接级别对语句操作的异步执行支持。<br /><br /> 此属性可设置是否 SQLGetInfo 信息类型 SQL_ASYNC_MODE 的返回 SQL_AM_CONNECTION 或 SQL_AM_STATEMENT。|  
|SQL_ATTR_AUTO_IPD (ODBC 3.0) |一个只读的 SQLUINTEGER 值，该值指定是否支持对 **SQLPrepare** 的调用后自动填充 IPD：<br /><br /> SQL_TRUE = 驱动程序支持对 **SQLPrepare** 的调用后的自动填充。<br /><br /> SQL_FALSE = 驱动程序不支持对 **SQLPrepare** 的调用后自动填充的。 不支持预定义语句的服务器将不能自动填充 IPD。<br /><br /> 如果为 SQL_ATTR_AUTO_IPD 连接属性返回 SQL_TRUE，则可以将语句属性 SQL_ATTR_ENABLE_AUTO_IPD 设置为启用或禁用 IPD 的自动填充。 如果 SQL_FALSE SQL_ATTR_AUTO_IPD，则 SQL_ATTR_ENABLE_AUTO_IPD 不能设置为 SQL_TRUE。 SQL_ATTR_ENABLE_AUTO_IPD 的默认值等于 SQL_ATTR_AUTO_IPD 的值。<br /><br /> 此连接属性可由 **SQLGetConnectAttr** 返回，但不能由 **SQLSetConnectAttr** 进行设置。|  
|SQL_ATTR_AUTOCOMMIT (ODBC 1.0) |一个 SQLUINTEGER 值，该值指定是否使用自动提交模式或手动提交模式：<br /><br /> SQL_AUTOCOMMIT_OFF = 驱动程序使用手动提交模式，应用程序必须使用 **SQLEndTran** 显式提交或回滚事务。<br /><br /> SQL_AUTOCOMMIT_ON = 驱动程序使用自动提交模式。 每个语句在执行后立即提交。 这是默认值。 将 SQL_ATTR_AUTOCOMMIT 设置为时，将提交连接上的任何打开的事务，SQL_AUTOCOMMIT_ON 从手动提交模式更改为自动提交模式。<br /><br /> 有关详细信息，请参阅 [提交模式](../../../odbc/reference/develop-app/commit-mode.md)。 **重要提示：**  某些数据源会在每次提交语句时删除访问计划并关闭连接上的所有语句的游标;自动提交模式可能会导致在执行每个 nonquery 语句之后或在关闭查询的游标时出现这种情况。 有关详细信息，请参阅 [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) 中的 SQL_CURSOR_COMMIT_BEHAVIOR 和 SQL_CURSOR_ROLLBACK_BEHAVIOR 信息类型和在 [游标和预定义语句中的事务的效果](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md)。 <br /><br /> 当批处理以自动提交模式执行时，可以执行两项任务。 整个批处理可以被视为 autocommitable 单元，或批处理中的每个语句都被视为 autocommitable 单元。 某些数据源可以同时支持这两种行为，并可提供一种方法来选择其中一种。 它是由驱动程序定义的，无论批处理是被视为 autocommitable 单元，还是批处理中的每个单独的语句都是 autocommitable。|  
|SQL_ATTR_CONNECTION_DEAD<br /><br />  (ODBC 3.5) |指示连接状态的只读 SQLUINTEGER 值。 如果 SQL_CD_TRUE，则连接已断开。 如果 SQL_CD_FALSE，则连接仍处于活动状态。|  
|SQL_ATTR_CONNECTION_TIMEOUT (ODBC 3.0) |与在返回应用程序之前等待连接完成的请求数的 SQLUINTEGER 值。 驱动程序应在不与查询执行或登录关联的情况下，返回 SQLSTATE HYT00 (超时过期) 。<br /><br /> 如果 *将 valueptr* 等于 0 (默认值) ，则不会超时。|  
|SQL_ATTR_CURRENT_CATALOG (ODBC 2.0) |一个字符串，该字符串包含数据源要使用的目录的名称。 例如，在 SQL Server 中，目录是一个数据库，因此驱动程序将 **USE** _database_ 语句发送到数据源，其中 *数据库* 是将 valueptr 中指定的数据库 \* 。 对于单层驱动程序，目录可能是目录，因此驱动程序将其当前目录更改为 **将 valueptr* 中指定的目录。|  
|SQL_ATTR_DBC_INFO_TOKEN (ODBC 3。8|SQLPOINTER 值，用于在 [SQLRateConnection](../../../odbc/reference/syntax/sqlrateconnection-function.md)的 (\* *pRating*) 参数不等于100时将连接信息标记设置回 DBC 句柄。<br /><br /> SQL_ATTR_DBC_INFO_TOKEN 仅设置。 不能使用 **SQLGetConnectAttr** 或 **SQLGetConnectOption** 来检索此值。 驱动程序管理器的 **SQLSetConnectAttr** 不会接受 SQL_ATTR_DBC_INFO_TOKEN，因为应用程序不应设置此属性。<br /><br /> 如果在设置 SQL_ATTR_DBC_INFO_TOKEN 之后，驱动程序返回 SQL_ERROR，则将释放刚从池中获取的连接。 然后，驱动程序管理器将尝试从池中获取另一个连接。 有关详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md) 。|  
|SQL_ATTR_ENLIST_IN_DTC (ODBC 3.0) |一个 SQLPOINTER 值，指定是否在由 Microsoft 组件服务协调的分布式事务中使用 ODBC 驱动程序。<br /><br /> 传递一个 DTC OLE transaction 对象，该对象指定要导出到 SQL Server 的事务，或 SQL_DTC_DONE 结束连接的 DTC 关联。<br /><br /> 客户端调用 Microsoft 分布式事务处理协调器 (MS DTC) OLE ITransactionDispenser：： BeginTransaction 方法来开始 MS DTC 事务，并创建表示该事务的 MS DTC 事务对象。 然后，应用程序调用带有 SQL_ATTR_ENLIST_IN_DTC 选项的 SQLSetConnectAttr，将 transaction 对象与 ODBC 连接相关联。 将在 MS DTC 事务的保护下执行所有相关的数据库活动。 应用程序使用 SQL_DTC_DONE 调用 SQLSetConnectAttr 以结束连接的 DTC 关联。 有关详细信息，请参阅 MS DTC 文档。|  
|SQL_ATTR_LOGIN_TIMEOUT (ODBC 1.0) |与在返回应用程序之前等待登录请求完成的秒数对应的 SQLUINTEGER 值。 默认值取决于驱动程序。 如果 *将 valueptr* 为0，则会禁用超时，且连接尝试会无限期等待。<br /><br /> 如果指定的超时值超过了数据源中的最大登录超时时间，则驱动程序将替换该值，并返回 SQLSTATE 01S02 (选项值已更改) 。|  
|SQL_ATTR_METADATA_ID (ODBC 3.0) |一个 SQLUINTEGER 值，确定如何处理目录函数的字符串参数。<br /><br /> 如果 SQL_TRUE，则将目录函数的字符串参数作为标识符处理。 这种情况并不重要。 对于 nondelimited 字符串，驱动程序将删除任何尾随空格，并将该字符串折叠为大写。 对于带分隔符的字符串，驱动程序将删除任何前导空格或尾随空格，并按字面区分分隔符。 如果将其中一个参数设置为 null 指针，则该函数将返回 SQL_ERROR 和 SQLSTATE HY009 (使用 null 指针) 无效。<br /><br /> 如果 SQL_FALSE，则不会将目录函数的字符串自变量作为标识符处理。 这种情况很重要。 它们可以包含字符串搜索模式，具体取决于参数。<br /><br /> 默认值为 SQL_FALSE。<br /><br /> 使用值列表的 **SQLTables** 的 *TableType* 参数不受此特性的影响。<br /><br /> 还可以对语句级别设置 SQL_ATTR_METADATA_ID。  (它是唯一一个也是语句属性的连接属性。 ) <br /><br /> 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。|  
|SQL_ATTR_ODBC_CURSORS (ODBC 2.0) |指定驱动程序管理器如何使用 ODBC 游标库的 SQLULEN 生成值：<br /><br /> SQL_CUR_USE_IF_NEEDED = 驱动程序管理器仅在需要时才使用 ODBC 游标库。 如果驱动程序支持 **SQLFetchScroll** 中的 SQL_FETCH_PRIOR 选项，驱动程序管理器将使用驱动程序的滚动功能。 否则，它将使用 ODBC 游标库。<br /><br /> SQL_CUR_USE_ODBC = 驱动程序管理器使用 ODBC 游标库。<br /><br /> SQL_CUR_USE_DRIVER = 驱动程序管理器使用驱动程序的滚动功能。 这是默认设置。<br /><br /> 有关 ODBC 游标库的详细信息，请参阅 [附录 F： ODBC 游标库](../../../odbc/reference/appendixes/appendix-f-odbc-cursor-library.md)。 **警告：**  在 Windows 的未来版本中将删除游标库。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。|  
|SQL_ATTR_PACKET_SIZE (ODBC 2.0) |指定网络数据包大小（以字节为单位）的 SQLUINTEGER 值。 **注意：**  许多数据源不支持此选项，或者仅可以返回而不是设置网络数据包大小。 <br /><br /> 如果指定的大小超过最大数据包大小或小于最小数据包大小，则驱动程序将替换该值，并返回 SQLSTATE 01S02 (选项值已更改) 。<br /><br /> 如果在建立连接后，应用程序设置数据包大小，则驱动程序将返回 SQLSTATE HY011 (属性 "现在无法设置") 。|  
|SQL_ATTR_QUIET_MODE (ODBC 2.0) | (HWND) 的窗口句柄。<br /><br /> 如果窗口句柄为空指针，则驱动程序将不会显示任何对话框。<br /><br /> 如果窗口句柄不是 null 指针，则它应为应用程序的父窗口句柄。 这是默认值。 驱动程序使用此句柄来显示对话框。 **注意：**  SQL_ATTR_QUIET_MODE 连接属性不适用于 **SQLDriverConnect** 显示的对话框。|  
|SQL_ATTR_TRACE (ODBC 1.0) |一个 SQLUINTEGER 值，指示驱动程序管理器是否执行跟踪：<br /><br /> SQL_OPT_TRACE_OFF = 跟踪 (默认) <br /><br /> SQL_OPT_TRACE_ON = 跟踪<br /><br /> 当启用跟踪时，驱动程序管理器会将每个 ODBC 函数调用写入到跟踪文件中。 **注意：**  启用跟踪时，驱动程序管理器可以从任何函数返回 SQLSTATE IM013 (Trace file 错误) 。 <br /><br /> 应用程序使用 SQL_ATTR_TRACEFILE 选项指定跟踪文件。 如果文件已存在，驱动程序管理器会将追加到该文件。 否则，将创建该文件。 如果跟踪已打开且未指定任何跟踪文件，则驱动程序管理器会将写入文件 SQL。在根目录中登录。<br /><br /> 应用程序可以将变量 **ODBCSharedTraceFlag** 设置为动态启用跟踪。 然后，将为当前正在运行的所有 ODBC 应用程序启用跟踪。 如果某个应用程序关闭了跟踪功能，则该应用程序只会关闭该应用程序。<br /><br /> 如果在应用程序调用具有 SQL_HANDLE_ENV 的 *HandleType* 的 **SQLAllocHandle** 时，系统信息中的 **Trace** 关键字设置为1，则将为所有句柄启用跟踪。 仅对调用 **SQLAllocHandle** 的应用程序启用此功能。<br /><br /> 使用 SQL_ATTR_TRACE 的 *属性* 调用 **SQLSetConnectAttr** 不要求 *ConnectionHandle* 参数有效，如果 *ConnectionHandle* 为 NULL，将不会返回 SQL_ERROR。 此属性适用于所有连接。|  
|SQL_ATTR_TRACEFILE (ODBC 1.0) |一个以 null 结尾的字符串，其中包含跟踪文件的名称。<br /><br /> SQL_ATTR_TRACEFILE 属性的默认值是在系统信息中通过 **TRACEFILE** 关键字指定的。 有关详细信息，请参阅 [ODBC 子项](../../../odbc/reference/install/odbc-subkey.md)。<br /><br /> 使用 SQL_ATTR_TRACEFILE 的 *属性* 调用 **SQLSetConnectAttr** 不需要 *ConnectionHandle* 参数有效，如果 *ConnectionHandle* 无效，则不会返回 SQL_ERROR。 此属性适用于所有连接。|  
|SQL_ATTR_TRANSLATE_LIB (ODBC 1.0) |一个以 null 结尾的字符串，其中包含的库的名称包含驱动程序访问以执行字符集转换等任务的 **SQLDriverToDataSource** 和 **SQLDataSourceToDriver** 函数。 仅当驱动程序已连接到数据源时，才能指定此选项。 此属性的设置将在连接之间保持不变。 有关转换数据的详细信息，请参阅 [翻译 dll](../../../odbc/reference/develop-app/translation-dlls.md) 和 [翻译 dll 函数引用](../../../odbc/reference/syntax/translation-dll-api-reference.md)。|  
|SQL_ATTR_TRANSLATE_OPTION (ODBC 1.0) |传递给转换 DLL 的32位标志值。 仅当驱动程序已连接到数据源时，才能指定此属性。 有关转换数据的信息，请参阅 [翻译 dll](../../../odbc/reference/develop-app/translation-dlls.md)。|  
|SQL_ATTR_TXN_ISOLATION (ODBC 1.0) |为当前连接设置事务隔离级别的32位位掩码。 在使用此选项调用 **SQLSetConnectAttr** 之前，应用程序必须调用 **SQLEndTran** 来提交或回滚连接上的所有打开的事务。<br /><br /> 可以通过调用 *InfoType* 等于 SQL_TXN_ISOLATION_OPTIONS 的 **SQLGetInfo** 来确定 *将 valueptr* 的有效值。<br /><br /> 有关事务隔离级别的说明，请参阅 [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) 和 [事务隔离级别](../../../odbc/reference/develop-app/transaction-isolation-levels.md)中 SQL_DEFAULT_TXN_ISOLATION 信息类型的描述。|  
  
 [1] 仅当描述符是实现描述符，而不是应用程序描述符时，才能以异步方式调用这些函数。  
  
## <a name="code-example"></a>代码示例  
 请参阅 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|返回连接属性的设置|[SQLGetConnectAttr 函数](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
