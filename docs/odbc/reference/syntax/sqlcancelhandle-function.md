---
description: SQLCancelHandle 函数
title: SQLCancelHandle 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCancelHandle
helpviewer_keywords:
- SQLCancelHandle function [ODBC]
ms.assetid: 16049b5b-22a7-4640-9897-c25dd0f19d21
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 727f9c15809969560410cfa54bdf9b70ec765f53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181047"
---
# <a name="sqlcancelhandle-function"></a>SQLCancelHandle 函数
**度**  
 引入的版本： ODBC 3.8 标准符合性：无  
  
 预计大多数 ODBC 3.8 (和更高版本的) 驱动程序都将实现此功能。 如果某个驱动程序不存在，则在 *handle* 参数中对 **SQLCancelHandle** 的连接调用的对 SQLSTATE 的调用将返回 SQL_ERROR，其为 IM001，消息 "driver 不支持此函数" "。通过使用语句句柄对 **SQLCancelHandle** 的调用将被映射到驱动程序管理器调用 **SQLCancel** ，如果驱动程序实现 **SQLCancel**，则可以 *对其进行* 处理。 应用程序可以使用 **SQLGetFunctions** 来确定驱动程序是否支持 **SQLCancelHandle**。  
  
 **摘要**  
 **SQLCancelHandle** 取消对连接或语句的处理。 *当 SQL_HANDLE_STMT 时，* 驱动程序管理器会将对 **SQLCancelHandle** 的调用映射到对 **SQLCancel** 的调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLCancelHandle(  
      SQLSMALLINT  HandleType,  
      SQLHANDLE    Handle);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送要对其进行 cacel 处理的句柄的类型。 有效值为 SQL_HANDLE_DBC 或 SQL_HANDLE_STMT。  
  
 *Handle*  
 送要取消处理的句柄。  
  
 如果 *句柄* 不是 *HandleType* 指定的类型的有效句柄，则 **SQLCancelHandle** 将返回 SQL_INVALID_HANDLE。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLCancelHandle** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 HandleType 的 SQL_HANDLE_STMT 和 SQL_HANDLE_DBC 和连接句柄 *句**柄的* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。  
  
 下表列出了通常由 **SQLCancelHandle** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 参数 *\* MessageText* 缓冲区中的 [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误|为与 *句柄* 关联的其中一个语句句柄调用了异步执行的与语句相关的函数，并且 *HandleType* 设置为 SQL_HANDLE_DBC。 调用 **SQLCancelHandle** 时，仍在执行异步函数。<br /><br />  (DM) 已 SQL_HANDLE_STMT *HandleType* 参数;在关联的连接句柄上调用了异步执行的函数;调用此函数时，该函数仍在执行。<br /><br />  (DM) 为与 *句柄* 关联的某个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并将 *HandleType* 设置为 SQL_HANDLE_DBC，并 SQL_PARAM_DATA_AVAILABLE 返回。 在检索所有流式处理参数的数据之前调用此函数。<br /><br /> 已为 *ConnectionHandle* 调用 **SQLBrowseConnect** ，并返回 SQL_NEED_DATA。 此函数是在浏览进程完成之前调用的。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY092|无效的属性/选项标识符|*HandleType* 设置为 SQL_HANDLE_ENV 或 SQL_HANDLE_DESC。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与该 *句柄* 关联的驱动程序不支持该函数。|  
  
 如果在将 *HandleType* 设置为 SQL_HANDLE_STMT 的情况下调用 **SQLCancelHandle** ，则它可以返回函数 **SQLCancel** 返回的任何 SQLSTATE。  
  
## <a name="comments"></a>注释  
 此函数类似于 **SQLCancel** ，但可能采用连接或语句句柄作为参数，而不只是语句句柄。 *当 SQL_HANDLE_STMT 时，* 驱动程序管理器会将对 **SQLCancelHandle** 的调用映射到对 **SQLCancel** 的调用。 这允许应用程序使用 **SQLCancelHandle** 来取消语句操作，即使驱动程序未实现 **SQLCancelHandle**。  
  
 有关取消语句操作的详细信息，请参阅 [SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)。  
  
 如果在 *处理* 对 **SQLCancelHandle** 的调用时没有正在进行的操作，则不会产生任何影响。  
  
 连接句柄上的 **SQLCancelHandle** 可以取消以下类型的处理：  
  
-   在连接上异步运行的函数。  
  
-   在另一线程上的连接句柄上运行的函数。  
  
 当调用 **SQLCancelHandle** 来取消在连接中以异步方式运行的函数时，由 **SQLCancelHandle** 发布的诊断记录将追加到被取消的操作返回的状态;但在取消其他线程上的连接上运行的函数时， **SQLCancelHandle** 不返回诊断记录。  
  
 使用 **SQLCancelHandle** 取消 **SQLEndTran** 可能会将连接置于挂起状态。 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。  
  
> [!NOTE]  
>  有关如何在将部署在早于 Windows 7 的 Windows 操作系统上的应用程序中使用 **SQLCancelHandle** 的信息，请参阅 [兼容性矩阵](../../../odbc/reference/develop-app/compatibility-matrix.md)。  
  
#### <a name="canceling-connection-related-asynchronous-processing"></a>取消 Connection-Related 异步处理  
 如果函数返回 SQL_STILL_EXECUTING，应用程序可以调用 **SQLCancelHandle** 来取消操作。 如果取消请求成功，则 **SQLCancelHandle** 将返回 SQL_SUCCESS。 这并不意味着原始函数已取消;它指示已处理取消请求。 驱动程序和数据源确定操作何时取消。 在不 SQL_STILL_EXECUTING 返回代码之前，应用程序必须继续调用原始函数。 如果原始函数已取消，则返回代码为 SQL_ERROR (操作取消) ，则为 SQLSTATE HY008。 如果原始函数完成了其正常处理 () ，则返回代码为 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，或者 SQL_ERROR，而 HY008 (操作) 和非操作（如果原始函数失败）。  
  
#### <a name="canceling-functions-executing-on-another-thread"></a>取消在另一个线程上执行的函数  
 在多线程应用程序中，应用程序可以取消在另一个线程上运行的操作。 若要取消该操作，应用程序将使用函数使用的句柄（但在不同的线程上）调用 **SQLCancelHandle** 。 驱动程序和操作系统确定如何取消该操作。 **SQLCancelHandle** 返回代码指示驱动程序是否处理了请求，同时返回 SQL_SUCCESS 或 SQL_ERROR (不) 返回任何诊断信息。 如果取消对原始函数的处理，则原始函数将返回 SQL_ERROR 和 SQLSTATE HY008 (操作取消) 。  
  
 如果在另一个线程上调用 **SQLCancelHandle** 来取消函数时执行函数，则函数可能成功并返回 SQL_SUCCESS，然后取消才能生效。 如果在 **SQLCancelHandle** 能够取消操作之前完成了操作，则对 **SQLCancelHandle** 的调用不起作用。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|取消在语句句柄上异步运行的函数、取消对需要数据的语句的函数或取消在另一个线程上对语句运行的函数。|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [异步执行（轮询方法）](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
