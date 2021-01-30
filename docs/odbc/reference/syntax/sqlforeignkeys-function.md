---
description: SQLForeignKeys 函数
title: SQLForeignKeys 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLForeignKeys
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLForeignKeys
helpviewer_keywords:
- SQLForeignKeys function [ODBC]
ms.assetid: 07f3f645-f643-4d39-9a10-70a72f24e608
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12ed4d570ac635f2de0db8388a4f6f01efbb2608
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165304"
---
# <a name="sqlforeignkeys-function"></a>SQLForeignKeys 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLForeignKeys** 可以返回：  
  
-   指定表中的外键列表， (指定表中的列，这些列引用其他表中的主键) 。  
  
-   引用指定表中的主键的其他表中的外键的列表。  
  
 驱动程序将每个列表作为指定语句的结果集返回。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLForeignKeys(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      PKCatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      PKSchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      PKTableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      FKCatalogName,  
     SQLSMALLINT    NameLength4,  
     SQLCHAR *      FKSchemaName,  
     SQLSMALLINT    NameLength5,  
     SQLCHAR *      FKTableName,  
     SQLSMALLINT    NameLength6);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *PKCatalogName*  
 送主键表目录名称。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时（例如，当驱动程序检索不同 Dbms 的数据时），则空字符串 ( "" ) 表示这些表没有目录。 *PKCatalogName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *PKCatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *PKCatalogName* 是普通参数;它按原义处理，其大小写很重要。 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。  
  
 *NameLength1*  
 送长度 **PKCatalogName*，以字符为字符。  
  
 *PKSchemaName*  
 送主键表架构名称。 如果驱动程序支持某些表的架构，而不支持其他表的架构（例如，当驱动程序从不同 Dbms 检索数据时），则空字符串 ( "" ) 表示不具有架构的那些表。 *PKSchemaName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *PKSchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *PKSchemaName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength2*  
 送长度 **PKSchemaName*，以字符为字符。  
  
 *PKTableName*  
 送主键表名称。 *PKTableName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *PKTableName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *PKTableName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength3*  
 送长度 **PKTableName*，以字符为字符。  
  
 *FKCatalogName*  
 送外键表目录名称。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时（例如，当驱动程序检索不同 Dbms 的数据时），则空字符串 ( "" ) 表示这些表没有目录。 *FKCatalogName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *FKCatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *FKCatalogName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength4*  
 送长度 **FKCatalogName*，以字符为字符。  
  
 *FKSchemaName*  
 送外键表架构名称。 如果驱动程序支持某些表的架构，而不支持其他表的架构（例如，当驱动程序从不同 Dbms 检索数据时），则空字符串 ( "" ) 表示不具有架构的那些表。 *FKSchemaName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *FKSchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *FKSchemaName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength5*  
 送长度 **FKSchemaName*，以字符为字符。  
  
 *FKTableName*  
 送外键表名。 *FKTableName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *FKTableName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *FKTableName* 是普通参数;它按原义处理，其大小写很重要。  
  
 *NameLength6*  
 送长度 **FKTableName*，以字符为字符。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLForeignKeys** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLForeignKeys** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用了该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效| (DM) 参数 *PKTableName* 和 *FKTableName* 都是 null 指针。<br /><br /> SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *FKCatalogName* 或 *PKCatalogName* 参数为 Null 指针，SQL_CATALOG_NAME *InfoType* 返回目录名称受支持。<br /><br />  (DM) SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，而 *FKSchemaName*、 *PKSchemaName*、 *FKTableName* 或 *PKTableName* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 SQLForeignKeys 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 一个名称长度参数的值小于0但不等于 SQL_NTS。|  
|||名称长度参数之一的值超出了相应名称的最大长度值。  (参阅 "注释"。) |  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|指定了目录名称，并且驱动程序或数据源不支持目录。<br /><br /> 指定了架构名称，并且驱动程序或数据源不支持架构。|  
|||驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 有关如何使用此函数返回的信息的信息，请参阅 [目录数据的使用](../../../odbc/reference/develop-app/uses-of-catalog-data.md)。  
  
 如果 \* *PKTableName* 包含表名称，则 **SQLForeignKeys** 将返回一个结果集，其中包含指定表及其引用的所有外键。 其他表中的外键列表不包含指向指定表中的唯一约束的外键。  
  
 如果 \* *FKTableName* 包含表名称，则 **SQLForeignKeys** 将返回一个结果集，该结果集包含指定表中指向其他表中的主键的所有外键，以及它们引用的其他表中的主键。 指定表中的外键列表不包含引用其他表中的唯一约束的外键。  
  
 如果 \* *PKTableName* 和 \* *FKTableName* 都包含表名，则 **SQLForeignKeys** 将返回 FKTableName 中指定的表中的外键，该外键 \* 引用在 **PKTableName* 中指定的表的主键。 最多只能有一个密钥。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 **SQLForeignKeys** 以标准结果集的形式返回结果。 如果请求与主键关联的外键，则结果集将按 FKTABLE_CAT、FKTABLE_SCHEM、FKTABLE_NAME 和 KEY_SEQ 进行排序。 如果请求与外键关联的主键，则结果集将按 PKTABLE_CAT、PKTABLE_SCHEM、PKTABLE_NAME 和 KEY_SEQ 进行排序。 下表列出了结果集中的列。  
  
 VARCHAR 列的长度不显示在表中;实际长度取决于数据源。 若要确定 PKTABLE_CAT 或 FKTABLE_CAT、PKTABLE_SCHEM 或 FKTABLE_SCHEM、PKTABLE_NAME 或 FKTABLE_NAME 以及 PKCOLUMN_NAME 或 FKCOLUMN_NAME 列的实际长度，应用程序可以使用 SQL_MAX_CATALOG_NAME_LEN、SQL_MAX_SCHEMA_NAME_LEN、SQL_MAX_TABLE_NAME_LEN 和 SQL_MAX_COLUMN_NAME_LEN 选项调用 **SQLGetInfo** 。  
  
 已为 ODBC 2.x 重命名了以下列 *。* 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC 2.x *列*|  
|---------------------|-----------------------|  
|PKTABLE_QUALIFIER|PKTABLE_CAT|  
|PKTABLE_OWNER|PKTABLE_SCHEM|  
|FKTABLE_QUALIFIER|FK_TABLE_CAT|  
|FKTABLE_OWNER|FKTABLE_SCHEM|  
  
 下表列出了结果集中的列。 列 14 (备注) 的其他列可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对特定于驱动程序的列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
|列名称|列号|数据类型|注释|  
|-----------------|-------------------|---------------|--------------|  
|PKTABLE_CAT (ODBC 1.0) |1|Varchar|主键表目录名称;如果不适用于数据源，则为 NULL。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时，它将为没有目录的那些表返回空字符串 ( "" ) 。|  
|PKTABLE_SCHEM (ODBC 1.0) |2|Varchar|主键表架构名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些表的架构，而不支持其他表的架构，例如当驱动程序从不同 Dbms 检索数据时，它将为没有架构的那些表返回空字符串 ( "" ) 。|  
|PKTABLE_NAME (ODBC 1.0) |3|Varchar not NULL|主键表名称。|  
|PKCOLUMN_NAME (ODBC 1.0) |4|Varchar not NULL|主键列名称。 对于没有名称的列，驱动程序返回空字符串。|  
|FKTABLE_CAT (ODBC 1.0) |5|Varchar|外键表目录名称;如果不适用于数据源，则为 NULL。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时，它将为没有目录的那些表返回空字符串 ( "" ) 。|  
|FKTABLE_SCHEM (ODBC 1.0) |6|Varchar|外键表架构名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些表的架构，而不支持其他表的架构，例如当驱动程序从不同 Dbms 检索数据时，它将为没有架构的那些表返回空字符串 ( "" ) 。|  
|FKTABLE_NAME (ODBC 1.0) |7|Varchar not NULL|外键表名。|  
|FKCOLUMN_NAME (ODBC 1.0) |8|Varchar not NULL|外键列名称。 对于没有名称的列，驱动程序返回空字符串。|  
|KEY_SEQ (ODBC 1.0) |9|Smallint（非 NULL）|键序列号，以 1) 开头 (。|  
|UPDATE_RULE (ODBC 1.0) |10|Smallint|要在 **更新** SQL 操作时应用于外键的操作。 可以具有以下值之一。  (引用的表是具有主键的表; 否则为。引用表是具有外键的表。 ) <br /><br /> SQL_CASCADE：更新所引用表的主键时，还会更新引用表的外键。<br /><br /> SQL_NO_ACTION：如果更新所引用表的主键将导致引用表中出现 "无关联引用" (也就是说，引用表中的行在引用的表) 中没有对应项，则将拒绝该更新。 如果对引用表的外键的更新会引入一个值，该值不作为所引用表的主键的值存在，则会拒绝该更新。  (此操作与 ODBC 2.x 中的 SQL_RESTRICT 操作相同 ) <br /><br /> SQL_SET_NULL：当引用的表中的一个或多个行以这样一种方式更新时，将更改主键的一个或多个组件，引用表中对应于主键的已更改组件的外键的组件在引用表的所有匹配行中都设置为 NULL。<br /><br /> SQL_SET_DEFAULT：当引用的表中的一个或多个行以这样一种方式进行更新时，将更改主键的一个或多个组件，引用表中对应于主键的已更改组件的外键的组件将设置为引用表的所有匹配行中相应的默认值。<br /><br /> 如果不适用于数据源，则为 NULL。|  
|DELETE_RULE (ODBC 1.0) |11|Smallint|要在 **删除** SQL 操作时应用于外键的操作。 可以具有以下值之一。  (引用的表是具有主键的表; 否则为。引用表是具有外键的表。 ) <br /><br /> SQL_CASCADE：删除被引用表中的某行时，也将删除引用表中的所有匹配行。<br /><br /> SQL_NO_ACTION：如果删除被引用表中的某行导致引用表中出现 "无关联引用" (也就是说，引用表中的行在所引用的表) 中没有对应项，则会拒绝该更新。  (此操作与 ODBC 2.x 中的 SQL_RESTRICT 操作相同 ) <br /><br /> SQL_SET_NULL：删除被引用表中的一行或多行时，引用表的所有匹配行中的引用表的外键的每个分量都将设置为 NULL。<br /><br /> SQL_SET_DEFAULT：删除被引用表中的一行或多行时，引用表的外键的每个组件都将设置为引用表中所有匹配行的适用默认值。<br /><br /> 如果不适用于数据源，则为 NULL。|  
|FK_NAME (ODBC 2.0) |12|Varchar|外键名称。 如果不适用于数据源，则为 NULL。|  
|PK_NAME (ODBC 2.0) |13|Varchar|主键名称。 如果不适用于数据源，则为 NULL。|  
|延迟 (ODBC 3.0) |14|Smallint|SQL_INITIALLY_DEFERRED、SQL_INITIALLY_IMMEDIATE SQL_NOT_DEFERRABLE。|  
  
## <a name="code-example"></a>代码示例  
 如下表中所示，此示例使用三个表： "订单"、"行" 和 "客户"。  
  
|ORDERS|行|客户|  
|------------|-----------|---------------|  
|订单|订单|CUSTID|  
|CUSTID|行|名称|  
|OPENDATE|PARTID|地址|  
|人|QUANTITY|电话|  
|状态|||  
  
 在 ORDERS 表中，CUSTID 标识了已进行销售的客户。 它是引用 CUSTOMERS 表中的 CUSTID 的外键。  
  
 在 "行" 表中，"订单 ID" 标识与行项关联的销售订单。 它是引用 ORDERS 表中的订单 ID 的外键。  
  
 此示例调用 **SQLPrimaryKeys** 以获取 ORDERS 表的主键。 结果集将包含一行;下表显示了重要的列。  
  
|TABLE_NAME|COLUMN_NAME|KEY_SEQ|  
|-----------------|------------------|--------------|  
|ORDERS|订单|1|  
  
 接下来，该示例调用 **SQLForeignKeys** ，以获取引用 ORDERS 表的主键的其他表中的外键。 结果集将包含一行;下表显示了重要的列。  
  
|PKTABLE_NAME|PKCOLUMN_NAME|FKTABLE_NAME|FKCOLUMN_NAME|KEY_SEQ|  
|-------------------|--------------------|-------------------|--------------------|--------------|  
|ORDERS|CUSTID|行|CUSTID|1|  
  
 最后，该示例调用 **SQLForeignKeys** 来获取 ORDERS 表中引用其他表的主键的外键。 结果集将包含一行;下表显示了重要的列。  
  
|PKTABLE_NAME|PKCOLUMN_NAME|FKTABLE_NAME|FKCOLUMN_NAME|KEY_SEQ|  
|-------------------|--------------------|-------------------|--------------------|--------------|  
|客户|CUSTID|ORDERS|CUSTID|1|  
  
```cpp  
#define TAB_LEN SQL_MAX_TABLE_NAME_LEN + 1  
#define COL_LEN SQL_MAX_COLUMN_NAME_LEN + 1  
  
LPSTR   szTable;              /* Table to display */  
  
UCHAR szPkTable[TAB_LEN];   /* Primary key table name */  
UCHAR szFkTable[TAB_LEN];   /* Foreign key table name */  
UCHAR szPkCol[COL_LEN];     /* Primary key column */  
UCHAR szFkCol[COL_LEN];     /* Foreign key column */  
  
SQLHSTMT      hstmt;  
SQLINTEGER    cbPkTable, cbPkCol, cbFkTable, cbFkCol, cbKeySeq;  
SQLSMALLINT   iKeySeq;  
SQLRETURN     retcode;  
  
// Bind the columns that describe the primary and foreign keys.  
// Ignore the table schema, name, and catalog for this example.  
  
SQLBindCol(hstmt, 3, SQL_C_CHAR, szPkTable, TAB_LEN, &cbPkTable);  
SQLBindCol(hstmt, 4, SQL_C_CHAR, szPkCol, COL_LEN, &cbPkCol);  
SQLBindCol(hstmt, 5, SQL_C_SSHORT, &iKeySeq, TAB_LEN, &cbKeySeq);  
SQLBindCol(hstmt, 7, SQL_C_CHAR, szFkTable, TAB_LEN, &cbFkTable);  
SQLBindCol(hstmt, 8, SQL_C_CHAR, szFkCol, COL_LEN, &cbFkCol);  
  
strcpy_s(szTable, sizeof(szTable), "ORDERS");  
  
/* Get the names of the columns in the primary key. */  
  
retcode = SQLPrimaryKeys(hstmt,  
         NULL, 0,             /* Catalog name */  
         NULL, 0,             /* Schema name */  
         szTable, SQL_NTS);   /* Table name */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL SUCCESS_WITH_INFO)) {  
  
   /* Fetch and display the result set. This will be a list of the */  
   /* columns in the primary key of the ORDERS table. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "Table: %s Column: %s Key Seq: %hd \n", szPkTable, szPkCol,  
      iKeySeq);  
}  
  
/* Close the cursor (the hstmt is still allocated). */  
  
SQLFreeStmt(hstmt, SQL_CLOSE);  
  
/* Get all the foreign keys that refer to ORDERS primary key.*/   
  
retcode = SQLForeignKeys(hstmt,  
         NULL, 0,            /* Primary catalog */  
         NULL, 0,            /* Primary schema */  
         szTable, SQL_NTS,   /* Primary table */  
         NULL, 0,            /* Foreign catalog */  
         NULL, 0,            /* Foreign schema */  
         NULL, 0);           /* Foreign table */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) {  
  
/* Fetch and display the result set. This will be all of the */  
/* foreign keys in other tables that refer to the ORDERS */  
/* primary key. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "%-s ( %-s ) <-- %-s ( %-s )\n", szPkTable,  
               szPkCol, szFkTable, szFkCol);  
}  
  
/* Close the cursor (the hstmt is still allocated). */  
  
SQLFreeStmt(hstmt, SQL_CLOSE);  
  
/* Get all the foreign keys in the ORDERS table. */  
  
retcode = SQLForeignKeys(hstmt,  
         NULL, 0,             /* Primary catalog */  
         NULL, 0,             /* Primary schema */  
         NULL, 0,             /* Primary table */  
         NULL, 0,             /* Foreign catalog */  
         NULL, 0,             /* Foreign schema */  
         szTable, SQL_NTS);   /* Foreign table */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) {  
  
/* Fetch and display the result set. This will be all of the */  
/* primary keys in other tables that are referred to by foreign */  
/* keys in the ORDERS table. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "%-s ( %-s )--> %-s ( %-s )\n", szFkTable, szFkCol, szPkTable, szPkCol);  
}  
  
/* Free the hstmt. */  
SQLFreeStmt(hstmt, SQL_DROP);  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|返回主键的列|[SQLPrimaryKeys 函数](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
|返回表统计信息和索引|[SQLStatistics 函数](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
