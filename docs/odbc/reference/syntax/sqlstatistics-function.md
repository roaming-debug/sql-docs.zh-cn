---
description: SQLStatistics 函数
title: SQLStatistics 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLStatistics
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLStatistics
helpviewer_keywords:
- SQLStatistics function [ODBC]
ms.assetid: 45210682-cfea-4e5d-9951-bcf1cbe10f41
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 711efa4797855e42b278e519106c38d62fda1817
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191751"
---
# <a name="sqlstatistics-function"></a>SQLStatistics 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLStatistics** 检索与表相关联的单个表和索引的统计信息列表。 驱动程序将以结果集的形式返回该信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLStatistics(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CatalogName,  
     SQLSMALLINT     NameLength1,  
     SQLCHAR *       SchemaName,  
     SQLSMALLINT     NameLength2,  
     SQLCHAR *       TableName,  
     SQLSMALLINT     NameLength3,  
     SQLUSMALLINT    Unique,  
     SQLUSMALLINT    Reserved);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *CatalogName*  
 送目录名称。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时（例如，当驱动程序检索不同 Dbms 的数据时），则空字符串 ( "" ) 指示这些表没有目录。 *CatalogName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *CatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *CatalogName* 是普通参数;它按原义处理，其大小写很重要。 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。  
  
 *NameLength1*  
 送**CatalogName* 中的字符的长度。  
  
 *SchemaName*  
 送架构名称。 如果驱动程序支持某些表的架构，而不支持其他表的架构（例如，当驱动程序从不同 Dbms 检索数据时），则空字符串 ( "" ) 指示不具有架构的那些表。 *SchemaName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *SchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *SchemaName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength2*  
 送**SchemaName* 中的字符的长度。  
  
 *TableName*  
 送表名称。 此参数不能为 null 指针。 *SchemaName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *TableName* 将被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *TableName* 为普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength3*  
 送**TableName* 的长度（字符）。  
  
 *唯一*  
 送索引的类型： SQL_INDEX_UNIQUE 或 SQL_INDEX_ALL。  
  
 Reserved   
 送指示结果集中基数列和页列的重要性。 以下选项只影响基数列和页列的返回值;即使未返回基数和页，也会返回索引信息。  
  
 SQL_ENSURE 请求驱动程序无条件检索统计信息。 仅符合开放组标准且不支持 ODBC 扩展的 (驱动程序将不能支持 SQL_ENSURE。 )   
  
 SQL_QUICK 请求，驱动程序仅在服务器可轻松使用时才检索基数和页面。 在这种情况下，驱动程序不能保证是最新值。 写入开放组标准 (应用程序将始终从 *符合 ODBC 3.x* 的驱动程序获取 SQL_QUICK 行为。 )   
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLStatistics** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLStatistics** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果未 SQL_NO_DATA 返回 **SQLFetch** 或 **SQLFetchScroll** ，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已返回 SQL_NO_DATA，则由驱动程序返回。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用了该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效|*TableName* 参数为 null 指针。<br /><br /> SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE， *CatalogName* 参数为 null 指针，SQL_CATALOG_NAME 的 *InfoType* 返回支持的目录名称。<br /><br />  (DM) SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，并且 *SchemaName* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLStatistics** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 一个名称长度参数的值小于0但不等于 SQL_NTS。<br /><br /> 名称长度参数之一的值超出了相应名称的最大长度值。|  
|HY100|唯一性选项类型超出了范围| (DM) 指定了无效的 *唯一* 值。|  
|HY101|准确性选项类型超出了范围| (DM) 指定了无效的 *保留* 值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|指定了一个目录，但该驱动程序或数据源不支持目录。<br /><br /> 指定了架构，但驱动程序或数据源不支持架构。<br /><br /> 驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回请求的结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 **SQLStatistics** 以标准结果集的形式返回有关单个表的信息，按 NON_UNIQUE、类型、INDEX_QUALIFIER、INDEX_NAME 和 ORDINAL_POSITION 排序。 结果集将统计信息 (在表的结果) 集的 "基数" 和 "页" 列中，其中包含有关每个索引的信息。 有关此信息的使用方式的信息，请参阅 [目录数据的使用](../../../odbc/reference/develop-app/uses-of-catalog-data.md)。  
  
 若要确定 TABLE_CAT、TABLE_SCHEM、TABLE_NAME 和 COLUMN_NAME 列的实际长度，应用程序可以使用 SQL_MAX_CATALOG_NAME_LEN、SQL_MAX_SCHEMA_NAME_LEN、SQL_MAX_TABLE_NAME_LEN 和 SQL_MAX_COLUMN_NAME_LEN 选项调用 **SQLGetInfo** 。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 已 *为 ODBC 2.x* 重命名了以下列。 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC *2.x* 列|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|SEQ_IN_INDEX|ORDINAL_POSITION|  
|COLLATION|ASC_OR_DESC|  
  
 下表列出了结果集中的列。 列 13 (FILTER_CONDITION) 的其他列可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对特定于驱动程序的列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
|列名称|列号|数据类型|注释|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1.0) |1|Varchar|要对其应用统计信息或索引的表的目录名称;如果不适用于数据源，则为 NULL。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时，它将为没有目录的那些表返回空字符串 ( "" ) 。|  
|TABLE_SCHEM (ODBC 1.0) |2|Varchar|要应用统计信息或索引的表的架构名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些表的架构，而不支持其他表的架构，例如当驱动程序从不同 Dbms 检索数据时，它将为没有架构的那些表返回空字符串 ( "" ) 。|  
|TABLE_NAME (ODBC 1.0) |3|Varchar not NULL|要应用统计信息或索引的表的表名。|  
|NON_UNIQUE (ODBC 1.0) |4|Smallint|指示索引是否不允许重复值：<br /><br /> SQL_TRUE 索引值是否可以是唯一的。<br /><br /> 如果索引值必须是唯一的，则 SQL_FALSE。<br /><br /> 如果 SQL_TABLE_STAT 类型，则返回 NULL。|  
|INDEX_QUALIFIER (ODBC 1.0) |5|Varchar|用于限定索引名称执行 **DROP INDEX** 的标识符;如果数据源不支持索引限定符或类型为 SQL_TABLE_STAT，则返回 NULL。 如果在此列中返回非 null 值，则必须使用它来限定 **DROP INDEX** 语句的索引名称;否则，TABLE_SCHEM 应用于限定索引名称。|  
|INDEX_NAME (ODBC 1.0) |6|Varchar|索引名称;如果 SQL_TABLE_STAT 类型，则返回 NULL。|  
|类型 (ODBC 1.0) |7|Smallint（非 NULL）|要返回的信息的类型：<br /><br /> SQL_TABLE_STAT 指示) 的基数或页列中的表 (的统计信息。<br /><br /> SQL_INDEX_BTREE 指示 B 树索引。<br /><br /> SQL_INDEX_CLUSTERED 指示聚集索引。<br /><br /> SQL_INDEX_CONTENT 指示内容索引。<br /><br /> SQL_INDEX_HASHED 指示哈希索引。<br /><br /> SQL_INDEX_OTHER 指示另一种索引类型。|  
|ORDINAL_POSITION (ODBC 1.0) |8|Smallint|索引 (中以 1) 开头的列序列号;如果 SQL_TABLE_STAT 类型，则返回 NULL。|  
|COLUMN_NAME (ODBC 1.0) |9|Varchar|列名称。 如果列基于表达式，如薪金 + 收益，则返回表达式;如果无法确定表达式，则返回空字符串。 如果 SQL_TABLE_STAT 类型，则返回 NULL。|  
|ASC_OR_DESC (ODBC 1.0) |10|Char(1)|列的排序顺序： "A" 表示升序;"D" 表示降序;如果数据源不支持列排序序列，或者如果类型 SQL_TABLE_STAT，则返回 NULL。|  
|基数 (ODBC 1.0) |11|Integer|表或索引的基数;如果类型 SQL_TABLE_STAT，则为表中的行数;如果类型不 SQL_TABLE_STAT，则为索引中的唯一值的数目;如果数据源中的值不可用，则返回 NULL。|  
|ODBC 1.0)  (页面|12|Integer|用于存储索引或表的页数;如果类型 SQL_TABLE_STAT，则为表的页数。如果类型不 SQL_TABLE_STAT，则为索引的页数;如果数据源中的值不可用，或者如果不适用于数据源，则返回 NULL。|  
|FILTER_CONDITION (ODBC 2.0) |13|Varchar|如果索引是筛选索引，则这是筛选条件，如薪金 > 30000;如果无法确定筛选条件，则为空字符串。<br /><br /> 如果索引不是筛选索引，则不能确定索引是否为筛选索引，也不能确定 SQL_TABLE_STAT 类型。|  
  
 如果结果集中的行对应于一个表，则驱动程序将类型设置为 SQL_TABLE_STAT，并将 NON_UNIQUE、INDEX_QUALIFIER、INDEX_NAME、ORDINAL_POSITION、COLUMN_NAME 和 ASC_OR_DESC 设置为 NULL。 如果基数或页在数据源中不可用，则驱动程序将其设置为 NULL。  
  
## <a name="code-example"></a>代码示例  
 有关类似函数的代码示例，请参阅 [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|按只进方向提取单个行或数据块。|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|返回外键的列|[SQLForeignKeys 函数](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|  
|返回主键的列|[SQLPrimaryKeys 函数](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
