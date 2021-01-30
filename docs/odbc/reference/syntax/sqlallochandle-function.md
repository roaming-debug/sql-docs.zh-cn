---
description: SQLAllocHandle 函数
title: SQLAllocHandle 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLAllocHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLAllocHandle
helpviewer_keywords:
- SQLAllocHandle function [ODBC]
ms.assetid: 6e7fe420-8cf4-4e72-8dad-212affaff317
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d546ddfa5f7e18365fec4bdff08032a8bba749f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184334"
---
# <a name="sqlallochandle-function"></a>SQLAllocHandle 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLAllocHandle** 分配一个环境、连接、语句或描述符句柄。  
  
> [!NOTE]  
>  此函数是一个泛型函数，用于分配替换 ODBC 2.0 函数 **SQLAllocConnect**、 **SQLAllocEnv** 和 **SQLAllocStmt** 的句柄。 允许应用程序调用 **SQLAllocHandle** 与 ODBC 2 一起使用。*x* 驱动程序调用 **SQLAllocHandle** 时，会根据需要在驱动程序管理器中映射到 **SQLAllocConnect**、 **SQLAllocEnv** 或 **SQLAllocStmt**。 有关详细信息，请参阅 "注释"。 有关在 ODBC 3 时驱动程序管理器将此函数映射到的内容的详细信息。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLAllocHandle(  
      SQLSMALLINT   HandleType,  
      SQLHANDLE     InputHandle,  
      SQLHANDLE *   OutputHandlePtr);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送要由 **SQLAllocHandle** 分配的句柄的类型。 必须是以下值之一：  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN 句柄仅由驱动程序管理器和驱动程序使用。 应用程序不应使用此句柄类型。 有关 SQL_HANDLE_DBC_INFO_TOKEN 的详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)。  
  
 *将 inputhandle*  
 送在其上下文中要分配新句柄的输入句柄。 如果 SQL_HANDLE_ENV *HandleType* ，则 SQL_NULL_HANDLE。 如果 SQL_HANDLE_DBC *HandleType* ，则它必须是环境句柄，如果 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC，则它必须是连接句柄。  
  
 *OutputHandlePtr*  
 输出指向缓冲区的指针，该缓冲区用于将句柄返回到新分配的数据结构。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_INVALID_HANDLE 或 SQL_ERROR。  
  
 当分配非环境句柄的句柄时，如果 **SQLAllocHandle** 返回 SQL_ERROR，则它会根据 *HandleType* 的值将 *OutputHandlePtr* 设置为 SQL_NULL_HDBC、SQL_NULL_HSTMT 或 SQL_NULL_HDESC，除非输出参数为 NULL 指针。 然后，应用程序可以从与 *将 inputhandle* 参数中的句柄关联的诊断数据结构获取其他信息。  
  
## <a name="environment-handle-allocation-errors"></a>环境句柄分配错误  
 环境分配在驱动程序管理器和每个驱动程序中都发生。 **SQLAllocHandle** 返回的 *HandleType* SQL_HANDLE_ENV 的错误取决于错误发生的级别。  
  
 如果调用了具有 SQL_HANDLE_ENV 的 *HandleType* 的 **SQLAllocHandle** 时，驱动程序管理器无法为 *\* OutputHandlePtr* 分配内存，或者应用程序为 *OutputHandlePtr* 提供 null 指针， **SQLAllocHandle** 将返回 SQL_ERROR。 驱动程序管理器将 **OutputHandlePtr* 设置为 SQL_NULL_HENV (，除非应用程序提供了一个返回 SQL_ERROR) 的 NULL 指针。 没有用于关联其他诊断信息的句柄。  
  
 在应用程序调用 **SQLConnect**、 **SQLBrowseConnect** 或 **SQLDriverConnect** 之前，驱动程序管理器不会调用驱动程序级环境句柄分配函数。 如果驱动程序级别的 **SQLAllocHandle** 函数中出现错误，则驱动程序管理器级别的 **SQLConnect**、 **SQLBrowseConnect** 或 **SQLDriverConnect** 函数将返回 SQL_ERROR。 诊断数据结构包含 SQLSTATE IM004 (驱动程序的 **SQLAllocHandle** 失败) 。 连接句柄上返回错误。  
  
 有关驱动程序管理器和驱动程序之间的函数调用流的详细信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLAllocHandle** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过调用 **SQLGetDiagRec** 并将相应的 *HandleType* 和 *句柄* 设置为 *将 inputhandle* 的值来获取关联的 SQLSTATE 值。 SQL_SUCCESS_WITH_INFO (，但不能为 *OutputHandle* 参数返回 SQL_ERROR) 。 下表列出了通常由 **SQLAllocHandle** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08003|连接未打开| (DM) *HandleType* 参数已 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC，但 *将 inputhandle* 参数指定的连接未打开。 必须 (成功完成连接过程，并且必须打开连接) ，驱动程序才能分配语句或描述符句柄。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 **MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误| (DM) 驱动程序管理器无法为指定的句柄分配内存。<br /><br /> 驱动程序无法为指定的句柄分配内存。|  
|HY009|空值指针的使用无效| (DM) *OutputHandlePtr* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) *HandleType* 参数已 SQL_HANDLE_DBC，并且尚未调用 **SQLSetEnvAttr** 来设置 SQL_ODBC_VERSION 环境特性。<br /><br />  (DM) 为 **将 inputhandle** 调用了异步执行函数，并且在 **HandleType** 设置为 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC 时调用 **SQLAllocHandle** 函数时仍在执行。|  
|HY013|内存管理错误|*HandleType* 参数 SQL_HANDLE_DBC、SQL_HANDLE_STMT 或 SQL_HANDLE_DESC;未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY014|超过了句柄数的限制|已达到可为 *HandleType* 参数指示的句柄类型分配的句柄数的驱动程序定义限制。|  
|HY092|无效的属性/选项标识符| (DM) *HandleType* 参数不是： SQL_HANDLE_ENV、SQL_HANDLE_DBC、SQL_HANDLE_STMT 或 SQL_HANDLE_DESC。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|*HandleType* 参数 SQL_HANDLE_DESC，驱动程序为 ODBC 2。*x* 驱动程序。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) *HandleType* 参数已 SQL_HANDLE_STMT，驱动程序不是有效的 ODBC 驱动程序。<br /><br />  (DM) *HandleType* 参数已 SQL_HANDLE_DESC，驱动程序不支持分配描述符句柄。|  
  
## <a name="comments"></a>注释  
 **SQLAllocHandle** 用于为环境、连接、语句和描述符分配句柄，如以下各节所述。 有关句柄的一般信息，请参阅 [句柄](../../../odbc/reference/develop-app/handles.md)。  
  
 如果驱动程序支持多个分配，则一个应用程序一次可以分配多个环境、连接或语句句柄。 在 ODBC 中，没有对可在任何时候分配的环境、连接、语句或描述符句柄的数量定义限制。 驱动程序可能对一次可以分配的特定类型的句柄的数量施加限制;有关详细信息，请参阅驱动程序文档。  
  
 如果应用程序调用 **SQLAllocHandle** ，并将 *\* OutputHandlePtr* 设置为已存在的环境、连接、语句或描述符句柄，则驱动程序将覆盖与 *句柄* 关联的信息，除非应用程序使用连接池 (参阅本部分后面的 "为连接池分配环境属性") 。 驱动程序管理器不会检查是否正在使用 *\* OutputHandlePtr* 中输入的 *句柄*，也不会在覆盖它们之前检查句柄的以前内容。  
  
> [!NOTE]  
>  这是不正确的 ODBC 应用程序编程，用为 *\* OutputHandlePtr* 定义的同一个应用程序变量来调用 **SQLAllocHandle** 两次，而不调用 **SQLFreeHandle** 以在重新分配该句柄之前释放句柄。 以这种方式覆盖 ODBC 句柄可能会导致 ODBC 驱动程序部分出现不一致的行为或错误。  
  
 在支持多个线程的操作系统上，应用程序可以在不同的线程上使用相同的环境、连接、语句或描述符句柄。 因此，驱动程序必须支持对此信息进行安全的多线程访问;例如，实现此目的的一种方法是使用临界区或信号量。 有关线程处理的详细信息，请参阅 [多线程处理](../../../odbc/reference/develop-app/multithreading.md)。  
  
 当调用来分配环境句柄时， **SQLAllocHandle** 不会设置 SQL_ATTR_ODBC_VERSION 环境特性;环境特性必须由应用程序设置，或者在调用 **SQLAllocHandle** 来分配连接句柄时，将返回 SQLSTATE HY010 (函数序列) 错误。  
  
 对于符合标准的应用程序， **SQLAllocHandle** 将在编译时映射到 **SQLAllocHandleStd** 。 这两个函数之间的区别在于，当调用 *HandleType* 参数设置为 SQL_HANDLE_ENV 时， **SQLAllocHandleStd** 会将 SQL_ATTR_ODBC_VERSION 环境特性设置为 SQL_OV_ODBC3。 这样做是因为符合标准的应用程序始终是 ODBC 3。*x* 应用程序。 此外，这些标准不需要注册应用程序版本。 这是这两个函数之间唯一的区别;否则，它们是相同的。 **SQLAllocHandleStd** 映射到驱动程序管理器中的 **SQLAllocHandle** 。 因此，第三方驱动程序无需实现 **SQLAllocHandleStd**。  
  
 ODBC 3.8 应用程序应使用：  
  
-   **SQLAllocHandle 和 Not SQLAllocHandleStd** ，用于分配环境句柄。  
  
-   **SQLSetEnvAttr** ，用于将 SQL_ATTR_ODBC_VERSION 环境特性设置为 SQL_OV_ODBC3_80。  
  
## <a name="allocating-an-environment-handle"></a>分配环境句柄  
 环境句柄提供对全局信息（如有效连接句柄和活动连接句柄）的访问。 有关环境句柄的一般信息，请参阅 [环境句柄](../../../odbc/reference/develop-app/environment-handles.md)。  
  
 若要请求环境句柄，应用程序将调用 **SQLAllocHandle** 和 *HandleType* 的 SQL_HANDLE_ENV 和 *将 inputhandle* 的 SQL_NULL_HANDLE。 驱动程序为环境信息分配内存，并在 *\* OutputHandlePtr* 参数中传递回关联句柄的值。 应用程序在所有需要环境句柄参数的后续调用中传递 *\* OutputHandle* 值。 有关详细信息，请参阅 [分配环境句柄](../../../odbc/reference/develop-app/allocating-the-environment-handle.md)。  
  
 在驱动程序管理器的环境句柄下，如果已存在驱动程序的环境句柄，则在建立连接时，将不会在该驱动程序中调用 **SQLAllocHandle** 的 SQL_HANDLE_ENV *HandleType* ，而只会在 *HandleType 为* SQL_HANDLE_DBC 的 **SQLAllocHandle** 中调用。 如果驱动程序管理器的环境句柄中不存在驱动程序的环境句柄，则在该环境的第一个连接句柄连接到该驱动程序时，将在该驱动程序中调用 SQLAllocHandle HandleType 为 SQL_HANDLE_ENV 和 SQL_HANDLE_DBC SQLAllocHandle 的。  
  
 当驱动程序管理器处理 SQL_HANDLE_ENV 的 *HandleType* 的 **SQLAllocHandle** 函数时，它会检查系统信息的 [ODBC] 部分中的 **Trace** 关键字。 如果将其设置为1，则驱动程序管理器将为当前应用程序启用跟踪。 如果设置了跟踪标志，则会在分配第一个环境句柄时开始跟踪，并在最后一个环境句柄释放时结束。 有关详细信息，请参阅 [配置数据源](../../../odbc/reference/install/configuring-data-sources.md)。  
  
 分配环境句柄之后，应用程序必须在环境句柄上调用 **SQLSetEnvAttr** 来设置 SQL_ATTR_ODBC_VERSION 环境特性。 如果在调用 **SQLAllocHandle** 以在环境中分配连接句柄之前未设置此属性，则分配连接的调用将返回 SQLSTATE HY010 (函数序列错误) 。 有关详细信息，请参阅 [声明应用程序的 ODBC 版本](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md)。  
  
## <a name="allocating-shared-environments-for-connection-pooling"></a>为连接池分配共享环境  
 可在单个进程上的多个组件之间共享环境。 一个共享环境可以同时由多个组件使用。 当组件使用共享环境时，它可以使用共用连接，这允许它分配和使用现有连接，而无需重新创建该连接。  
  
 在分配可用于连接池的共享环境之前，应用程序必须调用 **SQLSetEnvAttr** ，将 SQL_ATTR_CONNECTION_POOLING 环境特性设置为 SQL_CP_ONE_PER_DRIVER 或 SQL_CP_ONE_PER_HENV。 在这种情况下，将调用 **SQLSetEnvAttr** ，并将 *EnvironmentHandle* 设置为 null，这会使该属性成为进程级特性。  
  
 启用连接池后，应用程序将调用 **SQLAllocHandle** ，并将 *HandleType* 参数设置为 SQL_HANDLE_ENV。 由于已启用连接池，因此此调用分配的环境将为隐式共享环境。  
  
 分配共享环境时，在调用 **SQLAllocHandle** 的 SQL_HANDLE_DBC *HandleType* 之前，将不会确定要使用的环境。 此时，驱动程序管理器将尝试查找与应用程序所请求的环境属性匹配的现有环境。 如果不存在这样的环境，则会创建一个共享环境。 驱动程序管理器为每个共享环境维护引用计数;第一次创建环境时，计数设置为1。 如果找到匹配的环境，则会将该环境的句柄返回到应用程序，并且引用计数将递增。 以这种方式分配的环境句柄可用于任何接受环境句柄作为输入参数的 ODBC 函数。  
  
## <a name="allocating-a-connection-handle"></a>分配连接句柄  
 连接句柄提供对信息的访问，例如连接上的有效语句和描述符句柄以及事务当前是否打开。 有关连接句柄的一般信息，请参阅 [连接句柄](../../../odbc/reference/develop-app/connection-handles.md)。  
  
 若要请求连接句柄，应用程序将使用 SQL_HANDLE_DBC 的 *HandleType* 调用 **SQLAllocHandle** 。 *将 inputhandle* 参数设置为调用分配该句柄的 **SQLAllocHandle** 时所返回的环境句柄。 驱动程序为连接信息分配内存，并在 *\* OutputHandlePtr* 中向后传递关联句柄的值。 应用程序在需要连接句柄的所有后续调用中传递 *\* OutputHandlePtr* 值。 有关详细信息，请参阅 [分配连接句柄](../../../odbc/reference/develop-app/allocating-a-connection-handle-odbc.md)。  
  
 当应用程序调用 **SQLConnect**、 **SQLBrowseConnect** 或 **SQLDriverConnect** 时，驱动程序管理器将处理 **SQLAllocHandle** 函数并调用驱动程序的 **SQLAllocHandle** 函数。  (有关详细信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)。 )   
  
 如果在调用 **SQLAllocHandle** 之前未设置 SQL_ATTR_ODBC_VERSION 环境特性以在环境中分配连接句柄，则分配连接的调用将返回 SQLSTATE HY010 (函数序列错误) 。  
  
 当应用程序调用 **SQLAllocHandle** 并将 *将 inputhandle* 参数设置为 SQL_HANDLE_DBC 并将其设置为共享环境句柄时，驱动程序管理器将尝试查找与应用程序所设置的环境特性相匹配的现有共享环境。 如果不存在这样的环境，则将创建一个，并在驱动程序管理器) 为 1 (维护引用计数。 如果找到匹配的共享环境，则会将该句柄返回到应用程序，并递增其引用计数。  
  
 在调用 **SQLConnect** 或 **SQLDriverConnect** 之前，驱动程序管理器不会确定要使用的实际连接。 驱动程序管理器使用对 **SQLConnect** 的调用中的连接选项 (或) **SQLDriverConnect** 调用中的连接关键字，在连接分配后设置连接属性，以确定应使用池中的连接。 有关详细信息，请参阅 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
## <a name="allocating-a-statement-handle"></a>分配语句句柄  
 语句句柄提供对语句信息（如错误消息、游标名称和 SQL 语句处理的状态信息）的访问。 有关语句句柄的一般信息，请参阅 [语句句柄](../../../odbc/reference/develop-app/statement-handles.md)。  
  
 若要请求语句句柄，应用程序将连接到数据源，然后在提交 SQL 语句之前调用 **SQLAllocHandle** 。 在此调用中，应将 *HandleType* 设置为 SQL_HANDLE_STMT，并将 *将 inputhandle* 设置为调用分配该句柄的 **SQLAllocHandle** 所返回的连接句柄。 驱动程序为语句信息分配内存，将语句句柄与指定连接相关联，并在 *\* OutputHandlePtr* 中传递回关联句柄的值。 应用程序在所有需要语句句柄的后续调用中传递 *\* OutputHandlePtr* 值。 有关详细信息，请参阅 [分配语句句柄](../../../odbc/reference/develop-app/allocating-a-statement-handle-odbc.md)。  
  
 当分配语句句柄时，驱动程序会自动分配一组四个说明符，并将这些说明符的句柄分配给 SQL_ATTR_APP_ROW_DESC、SQL_ATTR_APP_PARAM_DESC、SQL_ATTR_IMP_ROW_DESC 和 SQL_ATTR_IMP_PARAM_DESC 语句特性。 这称为 *隐式* 分配的描述符。 若要显式分配应用程序描述符，请参阅下一节 "分配描述符句柄"。  
  
## <a name="allocating-a-descriptor-handle"></a>分配描述符句柄  
 当应用程序使用 SQL_HANDLE_DESC 的 *HandleType* 调用 **SQLAllocHandle** 时，驱动程序会分配应用程序描述符。 这称为 *显式* 分配的描述符。 应用程序通过调用带有 SQL_ATTR_APP_ROW_DESC 或 SQL_ATTR_APP_PARAM_DESC 属性的 **SQLSetStmtAttr** 函数，指导驱动程序使用显式分配的应用程序描述符，而不是自动分配给给定语句句柄的应用程序描述符。 实现说明符不能显式分配，也不能在 **SQLSetStmtAttr** 函数调用中指定实现说明符。  
  
 显式分配的描述符与连接句柄而不是语句句柄相关联 (因为) 自动分配的描述符。 仅当应用程序实际连接到数据库时，才能保持分配。 由于显式分配的描述符与连接句柄相关联，因此，应用程序可以将显式分配的描述符与连接内的多个语句相关联。 另一方面，隐式分配的应用程序描述符不能与多个语句句柄关联。  (不能将其与为其分配的任何语句句柄相关联。 ) 显式分配的描述符句柄可由应用程序显式释放，或通过 *HandleType* 的 SQL_HANDLE_DESC 调用 **SQLFreeHandle** ，或在关闭连接时隐式释放。  
  
 释放显式分配的描述符后，隐式分配的描述符再次与语句相关联。  (该语句的 SQL_ATTR_APP_ROW_DESC 或 SQL_ATTR_APP_PARAM_DESC 特性再次设置为隐式分配的描述符句柄。 ) 这对于与连接上的显式分配的描述符关联的所有语句都是如此。  
  
 有关描述符的详细信息，请参阅 [描述符](../../../odbc/reference/develop-app/descriptors.md)。  
  
## <a name="code-example"></a>代码示例  
 请参阅 [示例 ODBC Program](../../../odbc/reference/sample-odbc-program.md)、 [SQLBrowseConnect 函数](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)、 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)和 [SQLSetCursorName 函数](../../../odbc/reference/syntax/sqlsetcursorname-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|释放环境、连接、语句或描述符句柄|[SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|准备要执行的语句|[SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|设置连接属性|[SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|设置描述符字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|设置环境属性|[SQLSetEnvAttr 函数](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
