---
description: SQLTablePrivileges Function（SQLTablePrivileges 函数）
title: SQLTablePrivileges 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLTablePrivileges
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLTablePrivileges
helpviewer_keywords:
- SQLTablePrivileges function [ODBC]
ms.assetid: 8cfdb64f-64c5-47e6-ad57-0533ac630afa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 16c813c1859e139c3d85ebf901e673f1164073e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191739"
---
# <a name="sqltableprivileges-function"></a>SQLTablePrivileges Function（SQLTablePrivileges 函数）
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLTablePrivileges** 返回表列表和与每个表关联的特权。 驱动程序将该信息作为指定语句的结果集返回。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLTablePrivileges(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *CatalogName*  
 送表目录。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时（例如，当驱动程序检索不同 Dbms 的数据时），则空字符串 ( "" ) 表示这些表没有目录。 *CatalogName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *CatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *CatalogName* 是普通参数;它按原义处理，其大小写很重要。 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。  
  
 *NameLength1*  
 送**CatalogName* 中的字符的长度。  
  
 *SchemaName*  
 送架构名称的字符串搜索模式。 如果驱动程序支持某些表的架构，而不支持其他表的架构（例如，当驱动程序从不同 Dbms 检索数据时），则空字符串 ( "" ) 表示不具有架构的那些表。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *SchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *SchemaName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength2*  
 送**SchemaName* 中的字符的长度。  
  
 *TableName*  
 送表名的字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *TableName* 将被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *TableName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength3*  
 送**TableName* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLTablePrivileges** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLTablePrivileges** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果未 SQL_NO_DATA 返回 **SQLFetch** 或 **SQLFetchScroll** ，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已返回 SQL_NO_DATA，则由驱动程序返回。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 调用 **SQLTablePrivileges** 函数，并在其完成执行之前，对 *StatementHandle* 调用 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用 **SQLTablePrivileges** 函数。<br /><br /> 调用 **SQLTablePrivileges** 函数，并在其完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效|SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE， *CatalogName* 参数为 null 指针，SQL_CATALOG_NAME 的 *InfoType* 返回支持的目录名称。<br /><br />  (DM) SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，并且 *SchemaName* 或 *TableName* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLTablePrivileges** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 一个名称长度参数的值小于0但不等于 SQL_NTS。<br /><br /> 名称长度参数之一的值超出了相应限定符或名称的最大长度值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|指定了一个目录，但该驱动程序或数据源不支持目录。<br /><br /> 指定了架构，但驱动程序或数据源不支持架构。<br /><br /> 为表架构、表名称或列名称指定了字符串搜索模式，并且数据源不支持其中一个或多个参数的搜索模式。<br /><br /> 驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 *SchemaName* 和 *TableName* 参数接受搜索模式。 有关有效搜索模式的详细信息，请参阅 [模式值参数](../../../odbc/reference/develop-app/pattern-value-arguments.md)。  
  
 **SQLTablePrivileges** 将结果以标准结果集的形式返回，并按 TABLE_CAT、TABLE_SCHEM、TABLE_NAME、权限和被授权者排序。  
  
 若要确定 TABLE_CAT、TABLE_SCHEM 和 TABLE_NAME 列的实际长度，应用程序可以使用 SQL_MAX_CATALOG_NAME_LEN、SQL_MAX_SCHEMA_NAME_LEN 和 SQL_MAX_TABLE_NAME_LEN 选项调用 **SQLGetInfo** 。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 已 *为 ODBC 2.x* 重命名了以下列。 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC *2.x* 列|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
  
 下表列出了结果集中的列。 除第7列之外的其他列 (IS_GRANTABLE) 可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对驱动程序特定列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
|列名称|列号|数据类型|注释|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1.0) |1|Varchar|目录名称;如果不适用于数据源，则为 NULL。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时，它将为没有目录的那些表返回空字符串 ( "" ) 。|  
|TABLE_SCHEM (ODBC 1.0) |2|Varchar|架构名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些表的架构，而不支持其他表的架构，例如当驱动程序从不同 Dbms 检索数据时，它将为没有架构的那些表返回空字符串 ( "" ) 。|  
|TABLE_NAME (ODBC 1.0) |3|Varchar not NULL|表名。|  
|授权者 (ODBC 1.0) |4|Varchar|授予权限的用户的名称;如果不适用于数据源，则为 NULL。<br /><br /> 对于在被授权者列中的值为对象所有者的所有行，授权者列将为 "_SYSTEM"。|  
|被授权者 (ODBC 1.0) |5|Varchar not NULL|向其授予权限的用户的名称。|  
|权限 (ODBC 1.0) |6|Varchar not NULL|表特权。 可以是下列其中一项，也可以是特定于数据源的权限。<br /><br /> SELECT：允许被授权者检索表的一列或多列的数据。<br /><br /> INSERT：允许被授权者将包含一列或多列数据的新行插入表中。<br /><br /> 更新：允许被授权者更新表的一个或多个列中的数据。<br /><br /> 删除：允许被授权者删除表中的数据行。<br /><br /> 引用：允许被授权者引用约束中的表的一个或多个列 (例如，唯一的、引用的或表检查约束) 。<br /><br /> 允许被给定表权限被授权者使用的操作的范围取决于数据源。 例如，"更新" 特权可能允许被授权者更新一个数据源的表中的所有列，并且只允许该授权者在另一个数据源上具有 "更新" 权限的那些列。|  
|IS_GRANTABLE (ODBC 1.0) |7|Varchar|指示是否允许被授权者向其他用户授予权限;"是"、"否"，如果未知或不适用于数据源，则为 NULL。<br /><br /> 权限可以是可授予或可授予，但不能同时为两者。 **SQLColumnPrivileges** 返回的结果集将永远不会包含两个行，其中除 IS_GRANTABLE 列之外的所有列都包含相同的值。|  
  
## <a name="code-example"></a>代码示例  
 有关类似函数的代码示例，请参阅 [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回一个或多个列的权限|[SQLColumnPrivileges 函数](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|返回一个或多个表中的列|[SQLColumns 函数](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|返回表统计信息和索引|[SQLStatistics 函数](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|返回数据源中的表的列表|[SQLTables 函数](../../../odbc/reference/syntax/sqltables-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
