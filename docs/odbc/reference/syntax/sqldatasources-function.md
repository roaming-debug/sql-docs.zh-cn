---
description: SQLDataSources 函数
title: SQLDataSources 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSources
helpviewer_keywords:
- SQLDataSources function [ODBC]
ms.assetid: 3f63b1b4-e70e-44cd-96c6-6878d50d0117
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f22039a9e5f1443f828523d67a4cfa6c10496b23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206097"
---
# <a name="sqldatasources-function"></a>SQLDataSources 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLDataSources** 返回有关数据源的信息。 此函数仅由驱动程序管理器实现。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLDataSources(  
     SQLHENV          EnvironmentHandle,  
     SQLUSMALLINT     Direction,  
     SQLCHAR *        ServerName,  
     SQLSMALLINT      BufferLength1,  
     SQLSMALLINT *    NameLength1Ptr,  
     SQLCHAR *        Description,  
     SQLSMALLINT      BufferLength2,  
     SQLSMALLINT *    NameLength2Ptr);  
```  
  
## <a name="arguments"></a>参数  
 *EnvironmentHandle*  
 送环境句柄。  
  
 *方向*  
 送确定驱动程序管理器返回有关的数据源。 可以是：  
  
 SQL_FETCH_NEXT (提取列表中的下一个数据源名称) 、SQL_FETCH_FIRST (从列表的开头提取) 、SQL_FETCH_FIRST_USER (提取第一个用户 DSN) 或 SQL_FETCH_FIRST_SYSTEM (提取第一个系统 DSN) 。  
  
 如果将 "*方向*" 设置为 "SQL_FETCH_FIRST"，则对 "*方向*" 设置为 "SQL_FETCH_NEXT 的 **SQLDataSources** 的后续调用将返回用户和系统 dsn。 如果将 "*方向*" 设置为 "SQL_FETCH_FIRST_USER"，则对 "*方向*"**设置为 "SQL_FETCH_NEXT 的所有** 后续调用将仅返回用户 dsn。 如果将 "*方向*" 设置为 "SQL_FETCH_FIRST_SYSTEM"，则对 "*方向*"**设置为 "SQL_FETCH_NEXT 的所有** 后续调用将仅返回系统 dsn。  
  
 *ServerName*  
 输出指向要返回数据源名称的缓冲区的指针。  
  
 如果 *servername* 为 NULL，则 *NameLength1Ptr* 仍将返回 (排除字符数据的 NULL 终止字符的总字符数) 可在 *ServerName* 所指向的缓冲区中返回。  
  
 *BufferLength1*  
 送**ServerName* 缓冲区的长度（以字符为限）这无需长于 SQL_MAX_DSN_LENGTH 加上 null 终止字符。  
  
 *NameLength1Ptr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (不包括 null 终止字符) 可在 ServerName 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength1*，则 ServerName 中的数据源名称将 \* 被截断为 *BufferLength1* 减去 null 终止字符的长度。  
  
 *说明*  
 输出指向缓冲区的指针，将在此缓冲区中返回与数据源相关联的驱动程序的说明。 例如，dBASE 或 SQL Server。  
  
 如果 *description* 为 NULL，则 *NameLength2Ptr* 仍将返回 (排除字符数据的 NULL 终止字符的总字符数) 可在 *说明* 所指向的缓冲区中返回。  
  
 *BufferLength2*  
 送**说明* 缓冲区的长度（以字符为限）。  
  
 *NameLength2Ptr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (不包括 null 终止字符) 可在说明中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *BufferLength2*，则说明中的驱动程序说明 \* 将截断为 *BufferLength2* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLDataSources** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_ENV 和 *EnvironmentHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLDataSources** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告| (DM) 驱动程序管理器特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断| (DM) 缓冲区 \* *ServerName* 的大小不足以返回完整的数据源名称。 因此，名称被截断。 整个数据源名称的长度将在 NameLength1Ptr 中返回 \* 。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) <br /><br />  (DM) 缓冲区 \* *描述* 不够大，无法返回完整的驱动程序描述。 因此，说明已被截断。 未截断数据源说明的长度将在 **NameLength2Ptr* 中返回。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误| (DM) 出现错误，没有特定的 SQLSTATE，也没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误| (DM) 驱动程序管理器无法分配支持执行或完成该函数所需的内存。|  
|HY010|函数序列错误| (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *BufferLength1* 指定的值小于0。<br /><br />  (DM) 为参数 *BufferLength2* 指定的值小于0。|  
|HY103|检索代码无效| (DM) 为参数 *方向* 指定的值不等于 SQL_FETCH_FIRST、SQL_FETCH_FIRST_USER、SQL_FETCH_FIRST_SYSTEM 或 SQL_FETCH_NEXT。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
  
## <a name="comments"></a>注释  
 由于 **SQLDataSources** 是在驱动程序管理器中实现的，因此无论特定驱动程序的标准符合性如何，所有驱动程序都支持它。  
  
 应用程序可以多次调用 **SQLDataSources** 来检索所有数据源名称。 驱动程序管理器从系统信息中检索此信息。 如果没有更多的数据源名称，驱动程序管理器将返回 SQL_NO_DATA。 如果在 SQL_FETCH_NEXT 返回 SQL_NO_DATA 后立即调用 **SQLDataSources** ，则它将返回第一个数据源名称。 有关应用程序如何使用 **SQLDataSources** 返回的信息的信息，请参阅 [选择数据源或驱动程序](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)。  
  
 如果在第一次调用时将 SQL_FETCH_NEXT 传递给 **SQLDataSources** ，它将返回第一个数据源名称。  
  
 驱动程序确定如何将数据源名称映射到实际数据源。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|发现和列出连接到数据源所需的值|[SQLBrowseConnect 函数](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|连接到数据源|[SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|使用连接字符串或对话框连接到数据源|[SQLDriverConnect 函数](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|返回驱动程序说明和属性|[SQLDrivers 函数](../../../odbc/reference/syntax/sqldrivers-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
