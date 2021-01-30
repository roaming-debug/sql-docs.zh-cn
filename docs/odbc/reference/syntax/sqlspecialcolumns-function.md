---
description: SQLSpecialColumns 函数
title: SQLSpecialColumns 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSpecialColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSpecialColumns
helpviewer_keywords:
- SQLSpecialColumns function [ODBC]
ms.assetid: bb2d9f21-bda0-4e50-a8be-f710db660034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cf4f810499c4a3409d134806ead739096c5b1b89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191761"
---
# <a name="sqlspecialcolumns-function"></a>SQLSpecialColumns 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性：打开组  
  
 **摘要**  
 **SQLSpecialColumns** 检索有关指定表中的列的下列信息：  
  
-   唯一标识表中的行的一组最佳列。  
  
-   当事务更新行中的任何值时，自动更新的列。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLSpecialColumns(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   IdentifierType,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLSMALLINT   Scope,  
     SQLSMALLINT   Nullable);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *IdentifierType*  
 送要返回的列的类型。 必须是以下值之一：  
  
 SQL_BEST_ROWID：通过从一列或多列检索值来返回最优列或一组列，允许唯一标识指定表中的任何行。 列可以是专门为此目的而设计的伪列 (如 Oracle ROWID 或 Ingres TID) 或表的任何唯一索引的一个或多个列。  
  
 SQL_ROWVER：返回指定表中的一个或多个列（如果有），当任何事务更新行中的任何值时，该列或列将由数据源自动更新， (如 SQLBase ROWID 或 Sybase TIMESTAMP) 中所示。  
  
 *CatalogName*  
 送表的目录名称。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时（例如，当驱动程序检索不同 Dbms 的数据时），则空字符串 ( "" ) 表示这些表没有目录。 *CatalogName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *CatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *CatalogName* 是普通参数;它按原义处理，其大小写很重要。 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。  
  
 *NameLength1*  
 送**CatalogName* 中的字符的长度。  
  
 *SchemaName*  
 送表的架构名称。 如果驱动程序支持某些表的架构，而不支持其他表的架构（例如，当驱动程序从不同 Dbms 检索数据时），则空字符串 ( "" ) 表示不具有架构的那些表。 *SchemaName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *SchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *SchemaName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength2*  
 送**SchemaName* 中的字符的长度。  
  
 *TableName*  
 送表名称。 此参数不能为 null 指针。 *TableName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *TableName* 将被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *TableName* 为普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength3*  
 送**TableName* 的长度（字符）。  
  
 *范围*  
 送Rowid 所需的最小范围。 返回的 rowid 的作用域可能更大。 必须是下列选项之一：  
  
 SQL_SCOPE_CURROW：仅当定位在该行上时，rowid 才能保证有效。 如果行已由其他事务更新或删除，则以后使用 rowid 重新选择的可能不会返回一行。  
  
 SQL_SCOPE_TRANSACTION：保证 rowid 在当前事务的持续时间内有效。  
  
 SQL_SCOPE_SESSION：保证 rowid 在会话的持续时间内保持有效 () 的事务边界。  
  
 *可以为 Null*  
 送确定是否返回可以具有 NULL 值的特殊列。 必须是下列选项之一：  
  
 SQL_NO_NULLS：排除可以具有 NULL 值的特殊列。 某些驱动程序无法支持 SQL_NO_NULLS，如果指定 SQL_NO_NULLS，则这些驱动程序将返回空结果集。 应用程序应为此情况做好准备，并且仅当绝对需要时才请求 SQL_NO_NULLS。  
  
 SQL_NULLABLE：返回特殊列，即使它们可以具有 NULL 值也是如此。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSpecialColumns** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSpecialColumns** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果未 SQL_NO_DATA 返回 **SQLFetch** 或 **SQLFetchScroll** ，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已返回 SQL_NO_DATA，则由驱动程序返回。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效|*TableName* 参数为 null 指针。<br /><br /> SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE， *CatalogName* 参数为 null 指针，SQL_CATALOG_NAME 的 *InfoType* 返回支持的目录名称。<br /><br />  (DM) SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，并且 *SchemaName* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLSpecialColumns** 时仍在执行此函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 一个长度参数的值小于0但不等于 SQL_NTS。<br /><br /> 其中一个长度参数的值超过了对应名称的最大长度值。 可以通过使用 *InfoType* 值调用 **SQLGetInfo** 来获取每个名称的最大长度： SQL_MAX_CATALOG_NAME_LEN、SQL_MAX_SCHEMA_NAME_LEN 或 SQL_MAX_TABLE_NAME_LEN。|  
|HY097|列类型超出范围| (DM) 指定了无效的 *IdentifierType* 值。|  
|HY098|范围类型超出范围| (DM) 指定的 *范围* 值无效。|  
|HY099|可以为 null 的类型超出范围| (DM) 指定了无效的 *可以为 Null* 的值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|指定了一个目录，但该驱动程序或数据源不支持目录。<br /><br /> 指定了架构，但驱动程序或数据源不支持架构。<br /><br /> 驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回请求的结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 如果 SQL_BEST_ROWID *IdentifierType* 参数，则 **SQLSpecialColumns** 将返回唯一标识表中每行的一列或多列。 这些列始终可在 *选择列表* 或 **WHERE** 子句中使用。 **SQLColumns** 用于返回表中各列的信息，不一定返回唯一标识每行的列，或在某一事务更新行中的任何值时自动更新的列。 例如， **SQLColumns** 可能不会返回 Oracle 伪列 ROWID。 这就是使用 **SQLSpecialColumns** 来返回这些列的原因。 有关详细信息，请参阅 [目录数据的使用](../../../odbc/reference/develop-app/uses-of-catalog-data.md)。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 如果没有列唯一标识表中的每一行，则 **SQLSpecialColumns** 将返回没有行的行集;对该语句的 **SQLFetch** 或 **SQLFetchScroll** 的后续调用将返回 SQL_NO_DATA。  
  
 如果 *IdentifierType*、 *范围* 或 *可以为 null* 的参数指定数据源不支持的特性，则 **SQLSpecialColumns** 将返回一个空结果集。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *CatalogName*、 *SchemaName* 和 *TableName* 参数被视为标识符，因此在某些情况下它们不能设置为 null 指针。  (有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。 )   
  
 **SQLSpecialColumns** 以标准结果集的形式返回结果，并按作用域排序。  
  
 已 *为 ODBC 2.x* 重命名了以下列。 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC *2.x* 列|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
  
 若要确定 COLUMN_NAME 列的实际长度，应用程序可以使用 SQL_MAX_COLUMN_NAME_LEN 选项调用 **SQLGetInfo** 。  
  
 下表列出了结果集中的列。 列8以外的其他列 (PSEUDO_COLUMN) 可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对驱动程序特定列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
|列名称|列号|数据类型|注释|  
|-----------------|-------------------|---------------|--------------|  
|范围 (ODBC 1.0) |1|Smallint|Rowid 的实际作用域。 包含以下值之一：<br /><br /> SQL_SCOPE_CURROW SQL_SCOPE_TRANSACTION SQL_SCOPE_SESSION<br /><br /> SQL_ROWVER *IdentifierType* 时，将返回 NULL。 有关每个值的说明，请参阅本节前面部分中的 "语法" 的 *范围* 说明。|  
|COLUMN_NAME (ODBC 1.0) |2|Varchar not NULL|列名称。 对于没有名称的列，驱动程序返回空字符串。|  
|DATA_TYPE (ODBC 1.0) |3|Smallint（非 NULL）|SQL 数据类型。 这可以是 ODBC SQL 数据类型，也可以是特定于驱动程序的 SQL 数据类型。 有关有效 ODBC SQL 数据类型的列表，请参阅 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md)。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。|  
|TYPE_NAME (ODBC 1.0) |4|Varchar not NULL|依赖于数据源的数据类型名称;例如，"CHAR"、"VARCHAR"、"MONEY"、"LONG VARBINARY" 或 "CHAR ( ) 用于位数据"。|  
|COLUMN_SIZE (ODBC 1.0) |5|Integer|数据源中列的大小。 有关列大小的详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。|  
|BUFFER_LENGTH (ODBC 1.0) |6|Integer|当指定 SQL_C_DEFAULT 时，在 **SQLGetData** 或 **SQLFetch** 操作上传输的数据的长度（以字节为单位）。 对于数值数据，此大小可能不同于数据源中存储的数据的大小。 此值与字符或二进制数据的 COLUMN_SIZE 列相同。 有关详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。|  
|DECIMAL_DIGITS (ODBC 1.0) |7|Smallint|数据源中列的小数位数。 对于不适用十进制数字的数据类型，将返回 NULL。 有关十进制数字的详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。|  
|PSEUDO_COLUMN (ODBC 2.0) |8|Smallint|指示列是否为伪列，如 Oracle ROWID：<br /><br /> SQL_PC_UNKNOWN SQL_PC_NOT_PSEUDO SQL_PC_PSEUDO **注意：**  为了获得最大互操作性，不应使用 **SQLGetInfo** 返回的标识符引号字符将伪列括起来。|  
  
 在应用程序检索 SQL_BEST_ROWID 的值之后，应用程序可以使用这些值在定义的范围内重新选择该行。 **SELECT** 语句保证返回无行或一行。  
  
 如果应用程序 reselects 基于 rowid 列或列的行，但找不到该行，则应用程序可以假定该行已被删除或 rowid 列已被修改。 相反的情况并不是这样：即使 rowid 未更改，该行中的其他列也可能已更改。  
  
 对于需要在结果集内向前和向后滚动以检索一组行中的最新数据的应用程序，列类型 SQL_BEST_ROWID 返回的列很有用。 在定位在该行上时，可保证 rowid 的一列或多列不发生变化。  
  
 即使游标未定位在行上，rowid 的一列或多列仍可能仍然有效;应用程序可以通过检查结果集中的 "范围" 列来确定这一点。  
  
 对于列类型 SQL_ROWVER 返回的列对于需要能够检查是否已更新给定行中的任何列是否已在使用 rowid 只有行时进行了更新的应用程序很有用。 例如，在使用 rowid 重新选择行后，应用程序可以将 SQL_ROWVER 列中以前的值与刚刚提取的值进行比较。 如果 SQL_ROWVER 列中的值与以前的值不同，则应用程序可以警告用户显示的数据已更改。  
  
## <a name="code-example"></a>代码示例  
 有关类似函数的代码示例，请参阅 [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回一个或多个表中的列|[SQLColumns 函数](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|返回主键的列|[SQLPrimaryKeys 函数](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
