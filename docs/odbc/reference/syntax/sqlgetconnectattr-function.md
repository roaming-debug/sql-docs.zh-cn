---
description: SQLGetConnectAttr 函数
title: SQLGetConnectAttr 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectAttr
helpviewer_keywords:
- SQLGetConnectAttr function [ODBC]
ms.assetid: 2cb4ffa8-19d3-4664-8c2f-6682cdcc3f33
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 649e2dbf37770a94aae89ee43736f30e31a17573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203453"
---
# <a name="sqlgetconnectattr-function"></a>SQLGetConnectAttr 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetConnectAttr** 返回连接属性的当前设置。  
  
> [!NOTE]
>  有关 ODBC 3.x 应用程序使用 ODBC 2.x *驱动程序时*，驱动程序管理器将此函数映射到的内容的详细信息，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetConnectAttr(  
     SQLHDBC        ConnectionHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *Attribute*  
 送要检索的属性。  
  
 *ValuePtr*  
 输出一个指向内存的指针，该内存用于返回 *特性* 指定的特性的当前值。 对于整数类型属性，某些驱动程序只能写入缓冲区中的较低32位或16位，并使高阶位保持不变。 因此，在调用此函数之前，应用程序应使用 SQLULEN 生成的缓冲区并将值初始化为0。  
  
 如果 *将 valueptr* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的总字节数) 可在 *将 valueptr* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送如果 *attribute* 是一个 ODBC 定义的属性，并且 *将 valueptr* 指向某个字符串或二进制缓冲区，则此参数应为 \* *将 valueptr* 的长度。 如果 *attribute* 是一个 ODBC 定义的属性，并且 *\* 将 valueptr* 是一个整数，则将忽略 *BufferLength* 。 如果在调用 **SQLGetConnectAttrW**) 时， *\* 将 valueptr* 中的值为 Unicode 字符串 (，则 *BufferLength* 参数必须是偶数。  
  
 如果 *attribute* 是驱动程序定义的属性，则应用程序通过设置 *BufferLength* 参数来指示属性对驱动程序管理器的性质。 *BufferLength* 可具有以下值：  
  
-   如果 *\* 将 valueptr* 是指向字符串的指针，则 *BufferLength* 为字符串的长度。  
  
-   如果 *\* 将 valueptr* 是一个指向二进制缓冲区的指针，应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *BufferLength* 中。 这会在 *BufferLength* 中置入负值。  
  
-   如果 *\* 将 valueptr* 是一个指向字符串或二进制字符串以外的其他值的指针，则 *BufferLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *\* 将 valueptr* 包含固定长度的数据类型，则 *BufferLength* 根据需要 SQL_IS_INTEGER 或 SQL_IS_UINTEGER。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (排除 null 终止字符) 可在将 valueptr 中返回的总字节数 \* 。 如果属性值为字符串，并且可供返回的字节数大于 *BufferLength* 的长度，则 *\* 将 valueptr* 中的数据将被截断为 *BufferLength* 减去 null 终止字符的长度，并以 null 值终止，驱动程序。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetConnectAttr** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGETDIAGREC** ，从诊断数据结构获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetConnectAttr** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|将 valueptr 中返回的数据被 \* 截断为 *BufferLength* 减去 null 终止字符的长度。 在 *\* StringLengthPtr* 中返回未截断字符串值的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08003|连接未打开| (DM) 指定了需要打开的连接的 *属性* 值。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 通过 **SQLGetDiagField** 中的参数 *MessageText* 从诊断数据结构返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误|为 *ConnectionHandle* 调用了 (DM) **SQLBrowseConnect** ，并返回 SQL_NEED_DATA。 此函数在 **SQLBrowseConnect** 返回 SQL_SUCCESS_WITH_INFO 或 SQL_SUCCESS 之前被调用。<br /><br />  (DM) 为 *ConnectionHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) *\* 将 valueptr* 是一个字符串，而 BufferLength 小于零但不等于 SQL_NTS。|  
|HY092|无效的属性/选项标识符|为该驱动程序支持的 ODBC 版本指定 *的参数值无效。*|  
|HY114|驱动程序不支持连接级异步函数执行| (DM) 应用程序尝试使用 SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE 为不支持异步连接操作的驱动程序启用异步函数执行。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|为 argument *特性* 指定的值为驱动程序所支持的 odbc 版本的有效 odbc 连接属性，但驱动程序不支持该属性。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *ConnectionHandle* 相对应的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 有关连接属性的常规信息，请参阅 [连接属性](../../../odbc/reference/develop-app/connection-attributes.md)。  
  
 有关可设置的属性列表，请参阅 [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)。 请注意，如果 *attribute* 指定了返回字符串的属性，则 *将 valueptr* 必须是指向字符串缓冲区的指针。 返回的字符串的最大长度（包括 null 终止字符）将为 *BufferLength* 字节。  
  
 根据属性，应用程序无需在调用 **SQLGetConnectAttr** 之前建立连接。 但是，如果调用 **SQLGetConnectAttr** ，并且指定的属性没有默认值，并且以前调用过 **SQLSetConnectAttr**，则 **SQLGetConnectAttr** 将返回 SQL_NO_DATA。  
  
 如果 *特性* 是 SQL_ATTR_ TRACE 或 SQL_ATTR_ TRACEFILE，则 *ConnectionHandle* 无需有效; 如果 *ConnectionHandle* 无效，则 **SQLGetConnectAttr** 不会返回 SQL_ERROR 或 SQL_INVALID_HANDLE。 这些属性适用于所有连接。 如果另一个参数无效，则 **SQLGetConnectAttr** 将返回 SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
 尽管应用程序可以通过使用 **SQLSetConnectAttr** 来设置语句特性，但应用程序不能使用 **SQLGetConnectAttr** 来检索语句特性值;它必须调用 **SQLGetStmtAttr** 来检索语句特性的设置。  
  
 SQL_ATTR_AUTO_IPD 和 SQL_ATTR_CONNECTION_DEAD 连接特性都可以通过调用 **SQLGetConnectAttr** 返回，但不能通过调用 **SQLSetConnectAttr** 进行设置。  
  
> [!NOTE]  
>  对于 **SQLGetConnectAttr**，不提供异步支持。 实现 **SQLGetConnectAttr** 时，驱动程序可以通过最大程度地减少从服务器发送或请求信息的次数来提高性能。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|返回语句特性的设置|[SQLGetStmtAttr 函数](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|设置连接属性|[SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|设置环境属性|[SQLSetEnvAttr 函数](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
