---
description: SQLPrepare 函数
title: SQLPrepare 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPrepare
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPrepare
helpviewer_keywords:
- SQLPrepare function [ODBC]
ms.assetid: 332e1b4b-b0ed-4e7a-aa4d-4f35f4f4476b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce0348f2161def7dabc4941f088b883b286d2969
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192593"
---
# <a name="sqlprepare-function"></a>SQLPrepare 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLPrepare** 准备要执行的 SQL 字符串。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLPrepare(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     StatementText,  
     SQLINTEGER    TextLength);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *StatementText*  
 送SQL 文本字符串。  
  
 *TextLength*  
 送**StatementText* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLPrepare** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLPrepare** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|由于实现工作条件，指定的语句特性无效，因此临时替换相似的值。 可以调用 (**SQLGetStmtAttr** 来确定暂时替换值的值。 ) 替换值对 *StatementHandle* 有效，直到关闭游标。 可以更改的语句特性为： SQL_ATTR_CONCURRENCY SQL_ATTR_CURSOR_TYPE SQL_ATTR_KEYSET_SIZE SQL_ATTR_MAX_LENGTH SQL_ATTR_MAX_ROWS SQL_ATTR_QUERY_TIMEOUT SQL_ATTR_SIMULATE_CURSOR<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|21S01|插入值列表与列列表不匹配|\**StatementText* 包含 **INSERT** 语句，要插入的值的数量与派生表的程度不匹配。|  
|21S02|派生表的等级与列列表不匹配|\**StatementText* 包含 **CREATE VIEW** 语句，指定的名称数与查询规范定义的派生表的等级不同。|  
|22018|转换规范的字符值无效|**StatementText* 包含一个包含文本或参数的 SQL 语句，并且值与关联表列的数据类型不兼容。|  
|22019|转义符无效|自变量 *StatementText* 包含 **WHERE** 子句中包含 **转义** 的 **LIKE** 谓词，并且 **转义** 后转义字符的长度不等于1。|  
|22025|转义序列无效|参数 *StatementText* 包含 **WHERE** 子句中的 "**LIKE** _模式值_**转义**_转义符_"，并且模式值中转义符后面的字符不是 "%" 或 "_"。|  
|24000|无效的游标状态| (DM) 在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|34000|无效的游标名称|\**StatementText* 包含一个已定位的 **删除** 或定位 **更新**，并且正在准备的语句所引用的游标未打开。|  
|3D000|目录名称无效|在 *StatementText* 中指定的目录名称无效。|  
|3F000|架构名称无效|在 *StatementText* 中指定的架构名称无效。|  
|42000|语法错误或访问冲突|\**StatementText* 包含未可准备对象或包含语法错误的 SQL 语句。<br /><br /> **StatementText* 包含一个语句，该语句的用户不具有所需的权限。|  
|42S01|基表或视图已存在|\**StatementText* 包含 **CREATE TABLE** 或 **CREATE VIEW** 语句，并且指定的表名称或视图名称已存在。|  
|42S02|找不到基表或视图|\**StatementText* 包含 **删除表** 或 **drop view** 语句，指定的表名或视图名不存在。<br /><br /> \**StatementText* 包含 **ALTER table** 语句，但指定的表名不存在。<br /><br /> \**StatementText* 包含 **CREATE VIEW** 语句，而查询规范定义的表名称或视图名称不存在。<br /><br /> \**StatementText* 包含 **CREATE INDEX** 语句，但指定的表名不存在。<br /><br /> \**StatementText* 包含 **GRANT** 或 **REVOKE** 语句，指定的表名或视图名不存在。<br /><br /> \**StatementText* 包含 **SELECT** 语句，但指定的表名或视图名不存在。<br /><br /> \**StatementText* 包含 **DELETE**、 **INSERT** 或 **UPDATE** 语句，但指定的表名不存在。<br /><br /> \**StatementText* 包含 **CREATE TABLE** 语句，而在约束中指定的表 (引用) 不存在的表。|  
|42S11|索引已存在|\**StatementText* 包含 **CREATE index** 语句，而指定的索引名称已存在。|  
|42S12|找不到索引|\**StatementText* 包含 **DROP INDEX** 语句，但指定的索引名称不存在。|  
|42S21|列已存在|\**StatementText* 包含 **ALTER TABLE** 语句，在 **ADD** 子句中指定的列不是唯一的，也不标识基表中的现有列。|  
|42S22|找不到列|\**StatementText* 包含 **CREATE INDEX** 语句，但列列表中指定的一个或多个列名称不存在。<br /><br /> \**StatementText* 包含 **GRANT** 或 **REVOKE** 语句，指定的列名称不存在。<br /><br /> \**StatementText* 包含 **SELECT**、 **DELETE**、 **INSERT** 或 **UPDATE** 语句，但指定的列名称不存在。<br /><br /> \**StatementText* 包含 **CREATE TABLE** 语句，而在约束中指定的列 (引用不存在的表) 。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用了该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效| (DM) *StatementText* 是 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLPrepare** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 参数 *TextLength* 小于或等于0但不等于 SQL_NTS。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|并发设置对于定义的游标类型无效。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|超时期限已到数据源返回结果集之前过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 应用程序调用 **SQLPrepare** ，将 SQL 语句发送到数据源以便于准备。 有关已准备执行的详细信息，请参阅 [准备执行](../../../odbc/reference/develop-app/prepared-execution-odbc.md)。 应用程序可以在 SQL 语句中包括一个或多个参数标记。 若要包含参数标记，应用程序会将问号 (？ ) 嵌入到 SQL 字符串中的适当位置。 有关参数的信息，请参阅 [语句参数](../../../odbc/reference/develop-app/statement-parameters.md)。  
  
> [!NOTE]  
>  如果应用程序使用 **SQLPrepare** 做好准备，并 **SQLExecute** 提交 **提交** 或 **回滚** 语句，则它在 DBMS 产品之间将无法互操作。 若要提交或回滚事务，请调用 **SQLEndTran**。  
  
 驱动程序可以修改语句以使用数据源使用的 SQL 的形式，然后将其提交到数据源进行准备。 具体而言，驱动程序会修改用于为某些功能定义 SQL 语法的转义序列。  (有关 SQL 语句语法的说明，请参阅 [ODBC 中的转义序列](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md) 和 [附录 C： sql 语法](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md)。 ) 对于驱动程序，语句句柄类似于嵌入式 SQL 代码中的语句标识符。 如果数据源支持语句标识符，则驱动程序可以向数据源发送语句标识符和参数值。  
  
 准备好语句后，应用程序将使用语句句柄来引用后面的函数调用中的语句。 可以通过调用 **SQLExecute** 来重新执行与语句句柄关联的预定义语句，直到应用程序使用 SQL_DROP 选项释放语句并调用 **SQLFreeStmt** ，直到在对 **SQLPrepare**、 **SQLExecDirect** 或 (**SQLColumns**、 **SQLTables** 等) 的某个目录函数的调用中使用了语句句柄。 在应用程序准备好某个语句后，它可以请求有关结果集格式的信息。 对于某些实现，在 **SQLPrepare** 后调用 **SQLDescribeCol** 或 **SQLDescribeParam** 可能不如在 **SQLExecute** 或 **SQLExecDirect** 后调用函数的效率高。  
  
 当应用程序调用 **SQLPrepare** 时，某些驱动程序无法返回语法错误或访问冲突。 驱动程序可以处理语法错误和访问冲突，只处理语法错误，也不能处理语法错误或访问冲突。 因此，在调用 **SQLNumResultCols**、 **SQLDescribeCol**、 **SQLColAttribute** 和 **SQLExecute** 等后续相关函数时，应用程序必须能够处理这些情况。  
  
 根据驱动程序和数据源的功能，当语句准备就绪时，可以检查参数信息 (例如数据类型)  (如果所有参数都已绑定) 或执行时 (（如果尚未绑定所有参数）。 为了实现最大互操作性，应用程序应在对同一语句准备新的 SQL 语句之前，取消应用于旧 SQL 语句的所有参数的绑定。 这可以防止由于旧参数信息应用于新语句而导致的错误。  
  
> [!IMPORTANT]  
>  如果通过显式调用 **SQLEndTran** 或在自动提交模式下工作来提交事务，则可能会导致数据源删除连接上所有语句的访问计划。 有关详细信息，请参阅 [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) 中的 SQL_CURSOR_COMMIT_BEHAVIOR 和 SQL_CURSOR_ROLLBACK_BEHAVIOR 信息类型和在 [游标和预定义语句中的事务的效果](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md)。  
  
## <a name="code-example"></a>代码示例  
 请参阅 [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)、 [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)和 [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配语句句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|将缓冲区绑定到参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|执行提交或回滚操作|[SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|返回受语句影响的行数|[SQLRowCount Function（SQLRowCount 函数）](../../../odbc/reference/syntax/sqlrowcount-function.md)|  
|设置游标名称|[SQLSetCursorName 函数](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
