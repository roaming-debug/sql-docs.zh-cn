---
description: SQLGetCursorName 函数
title: SQLGetCursorName 函数 |Microsoft Docs
ms.custom: ''
ms.date: 06/12/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetCursorName
helpviewer_keywords:
- SQLGetCursorName function [ODBC]
ms.assetid: e6e92199-7bb6-447c-8987-049a4c6ce05d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f413bb9fb5a11a9e09b932725219e47177aa0c2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199211"
---
# <a name="sqlgetcursorname-function"></a>SQLGetCursorName 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetCursorName** 返回与指定语句相关联的游标名称。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetCursorName(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CursorName,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   NameLengthPtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *CursorName*  
 输出指向缓冲区的指针，将在此缓冲区中返回游标名称。  
  
 如果 *CursorName* 为 NULL，则 *NameLengthPtr* 仍返回 (字符数据的 null 终止字符以外的字符总数) 可在 *CursorName* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送\* *CursorName* 的长度，以字符为长度。 
  
 *NameLengthPtr*  
 输出一个指向内存的指针，将在此内存中返回 (不包括 null 终止字符) 可在 CursorName 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength*，则 CursorName 中的游标名称将 \* 被截断为 *BufferLength* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetCursorName** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetCursorName** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *CursorName* 不够大，无法返回整个游标名称，因此游标名称已被截断。 在 **NameLengthPtr* 中返回未截断游标名称的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLGetCursorName** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY015|没有可用的游标名称| (DM) 该驱动程序是一个 ODBC *2.x 驱动程序* ，该语句上没有打开的游标，并且没有使用 **SQLSetCursorName** 设置游标名称。|  
|HY090|字符串或缓冲区长度无效| (DM) 参数 *BufferLength* 中指定的值小于0。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 游标名称仅在定位的 update 和 delete 语句中使用 (例如，**更新**_表名称_.。。**当前**_游标名称_) 。 有关详细信息，请参阅 [定位更新和删除语句](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md)。 如果应用程序不调用 **SQLSetCursorName** 来定义游标名称，则驱动程序将生成一个名称。 此名称以字母开头 SQL_CUR。  
  
> [!NOTE]
>  在 ODBC 2.x *中，当* 没有打开的游标，并且调用 **SQLSetCursorName** 未设置任何名称时，对 **SQLGETCURSORNAME** 的调用将返回 SQLSTATE HY015 (没有) 的游标名称。 在 ODBC 3.x *中，这* 种情况不再成立;无论何时调用 **SQLGetCursorName** ，驱动程序都将返回游标名称。  
  
 无论名称是显式创建还是隐式创建， **SQLGetCursorName** 都将返回游标的名称。 如果未调用 **SQLSetCursorName** ，则会隐式生成游标名称。 可以调用 **SQLSetCursorName** 来重命名语句上的游标，只要游标处于已分配或准备好的状态。  
  
 显式或隐式设置的游标名称将在删除与之关联的 *StatementHandle* 后保持不变，并使用 **SQLFreeHandle** 和 *HandleType* 的 SQL_HANDLE_STMT。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|准备要执行的语句|[SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|设置游标名称|[SQLSetCursorName 函数](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
