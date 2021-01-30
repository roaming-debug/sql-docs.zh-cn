---
description: SQLExecDirect 函数
title: SQLExecDirect 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExecDirect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLExecDirect
helpviewer_keywords:
- SQLExecDirect function [ODBC]
ms.assetid: 985fcee1-f204-425c-bdd1-deb0e7d7bbd9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 938d2aa1631041e9dbb7a51ea952ff7d60811fb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179882"
---
# <a name="sqlexecdirect-function"></a>SQLExecDirect 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 如果语句中存在任何参数， **SQLExecDirect** 将使用参数标记变量的当前值执行可准备对象语句。 **SQLExecDirect** 是一种最快的方法，可用于一次性执行 SQL 语句。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLExecDirect(  
     SQLHSTMT     StatementHandle,  
     SQLCHAR *    StatementText,  
     SQLINTEGER   TextLength);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *StatementText*  
 送要执行的 SQL 语句。  
  
 *TextLength*  
 送**StatementText* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NEED_DATA、SQL_STILL_EXECUTING、SQL_ERROR、SQL_NO_DATA、SQL_INVALID_HANDLE 或 SQL_PARAM_DATA_AVAILABLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLExecDirect** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLExecDirect** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01001|游标操作冲突|\**StatementText* 包含定位的 update 或 delete 语句，并且未更新或删除任何行或多行。  (有关多行更新的详细信息，请参阅 **SQLSetStmtAttr** 中 SQL_ATTR_SIMULATE_CURSOR *属性* 的说明。 ) <br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01003|Set 函数中消除了 NULL 值|自变量 *StatementText* 包含一个 set 函数 (例如 **AVG**、 **MAX**、 **MIN** 等) ，但不包含 **COUNT** 集函数，并且在应用函数之前消除了 NULL 参数值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|为输入/输出或输出参数返回的字符串或二进制数据导致截断非空白字符或非空的二进制数据。 如果它是一个字符串值，则它将被右截断。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01006|未撤消特权|\**StatementText* 包含 **REVOKE** 语句，而用户没有指定的权限。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01007|未授予特权|*\* StatementText* 是 **GRANT** 语句，因此无法向用户授予指定的权限。|  
|01S02|选项值已更改|由于实现工作条件，指定的语句特性无效，因此临时替换相似的值。 可以调用 (**SQLGetStmtAttr** 来确定暂时替换的值是什么。 ) 替换值对 *StatementHandle* 有效，直到游标关闭，此时语句特性会恢复为其以前的值。 可以更改的语句特性如下：<br /><br /> SQL_ ATTR_CONCURRENCY SQL_ ATTR_CURSOR_TYPE SQL_ ATTR_KEYSET_SIZE SQL_ ATTR_MAX_LENGTH SQL_ ATTR_MAX_ROWS SQL_ ATTR_QUERY_TIMEOUT SQL_ ATTR_SIMULATE_CURSOR<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S07|小数截断|为输入/输出参数或输出参数返回的数据被截断，以使数值数据类型的小数部分被截断，或时间、时间戳或间隔数据类型的时间部分的小数部分被截断。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07002|COUNT 字段不正确|**SQLBindParameter** 中指定的参数数目小于 StatementText 中包含的 SQL 语句中的参数数量 \* 。<br /><br /> 调用 **SQLBindParameter** 时， *ParameterValuePtr* 设置为 null 指针， *StrLen_or_IndPtr* 未设置为 SQL_NULL_DATA 或 SQL_DATA_AT_EXEC，并且 *InputOutputType* 未设置为 SQL_PARAM_OUTPUT，因此 **SQLBindParameter** 中指定的参数数目大于 **StatementText* 中包含的 SQL 语句中的参数数量。|  
|07006|受限制的数据类型属性冲突|绑定参数的 **SQLBindParameter** 中的 *ValueType* 参数标识的数据值无法转换为 **SQLBindParameter** 中 *ParameterType* 参数所标识的数据类型。<br /><br /> 为绑定为 SQL_PARAM_INPUT_OUTPUT 或 SQL_PARAM_OUTPUT 的参数返回的数据值无法转换为 **SQLBindParameter** 中 *ValueType* 参数标识的数据类型。<br /><br />  (如果一个或多个行的数据值无法转换，但已成功返回一个或多个行，则此函数将返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07007|受限制的参数值冲突|参数类型 SQL_PARAM_INPUT_OUTPUT_STREAM 仅用于在部分中发送和接收数据的参数。 此参数类型不允许输入绑定缓冲区。<br /><br /> 当参数类型 SQL_PARAM_INPUT_OUTPUT，且 \* 在 **SQLBindParameter** 中指定的 *StrLen_or_IndPtr* 不等于 SQL_NULL_DATA、SQL_DEFAULT_PARAM、SQL_LEN_DATA_AT_EXEC (LEN) 或 SQL_DATA_AT_EXEC 时，将发生此错误。|  
|07S01|默认参数的使用无效|使用 **SQLBindParameter** 设置的参数值是 SQL_DEFAULT_PARAM 的，并且相应的参数没有默认值。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|21S01|插入值列表与列列表不匹配|\**StatementText* 包含 **INSERT** 语句，要插入的值的数量与派生表的程度不匹配。|  
|21S02|派生表的等级与列列表不匹配|\**StatementText* 包含 **CREATE VIEW** 语句，而非限定列列表 (sql 语句的 *列标识符* 参数中为视图指定的列数) 包含的名称多于由 sql 语句的 *查询规范* 参数定义的派生表中的列数。|  
|22001|字符串数据，右截断|将字符或二进制值分配给列会导致截断非空白字符数据或非空的二进制数据。|  
|22002|需要指示器变量，但未提供|NULL 数据已绑定到一个输出参数，该参数的 *StrLen_or_IndPtr* 由 **SQLBindParameter** 设置为 null 指针。|  
|22003|数值超出范围|**StatementText* 包含一个 SQL 语句，该语句包含绑定的数值参数或文本，而值导致整个 (，而不是分配给关联表列时要截断的数字) 部分的小数部分。<br /><br /> 对于一个或多个输入/输出参数 (，返回数值或字符串) 的数值将导致整个 (，而不是将被截断的数字) 部分的小数部分。|  
|22007|Datetime 格式无效|**StatementText* 包含一个 SQL 语句，该语句包含一个作为绑定参数的日期、时间或时间戳结构，并且参数分别为无效的日期、时间或时间戳。<br /><br /> 输入/输出或输出参数被绑定到日期、时间或时间戳 C 结构，返回的参数中的值分别为无效的日期、时间或时间戳。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|22008|日期时间字段溢出|**StatementText* 包含一个 SQL 语句，该语句包含一个 datetime 表达式，在计算该表达式时，将导致日期、时间或时间戳结构无效。<br /><br /> 为输入/输出或输出参数计算的日期时间表达式导致日期、时间或时间戳 C 结构无效。|  
|22012|被零除|**StatementText* 包含一个 SQL 语句，该语句包含导致被零除的算术表达式。<br /><br /> 计算出的输入/输出参数或输出参数的算术表达式导致被零除。|  
|22015|间隔字段溢出|*\* StatementText* 包含一个精确数值或间隔参数，该参数在转换为间隔 SQL 数据类型时导致了有效位丢失。<br /><br /> *\* StatementText* 包含多个字段的间隔参数，转换为列中的数值数据类型时，该参数没有数值数据类型的表示形式。<br /><br /> *\* StatementText* 包含分配给某个间隔 sql 类型的参数数据，且 interval sql 类型中不存在 C 类型的值的表示形式。<br /><br /> 将是精确数值或间隔 SQL 类型的输入/输出或输出参数分配给 interval C 类型会导致有效位丢失。<br /><br /> 如果将输入/输出参数或输出参数分配给 interval C 结构，则间隔数据结构中没有数据表示形式。|  
|22018|转换规范的字符值无效|*\* StatementText* 包含一个是精确或近似数值、日期时间或时间间隔数据类型的 C 类型; 该列的 SQL 类型是字符数据类型; 并且列中的值不是绑定 C 类型的有效文本。<br /><br /> 返回 input/output 或 output 参数时，SQL 类型是精确或近似数值、日期时间或间隔数据类型;C 类型为 SQL_C_CHAR;列中的值不是绑定的 SQL 类型的有效文本。|  
|22019|转义符无效|\**StatementText* 包含一个 SQL 语句，该语句包含 **WHERE** 子句中 **带有 ESCAPE** 的 **LIKE** 谓词，并且 **转义** 字符后面的转义符的长度不等于1。|  
|22025|转义序列无效|\**StatementText* 包含一个 SQL 语句，该语句包含 **WHERE** 子句中的 "**LIKE** _模式值_**转义**_转义符_"，并且模式值中转义符后面的字符不是 "%" 或 "_"。|  
|23000|完整性约束冲突|**StatementText* 包含一个包含参数或文本的 SQL 语句。 对于在关联的表列中定义为 NOT NULL 的列，参数值为 NULL，为约束为仅包含唯一值的列提供了一个重复值，或违反了某个其他完整性约束。|  
|24000|无效的游标状态|游标由 **SQLFetch** 或 **SQLFetchScroll** 定位在 *StatementHandle* 上。 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。<br /><br /> 游标已打开，但未定位在 *StatementHandle* 上。<br /><br /> **StatementText* 包含定位的 update 或 delete 语句，并且游标位于结果集的开头之前或结果集的结尾之后。|  
|34000|无效的游标名称|**StatementText* 包含定位的 update 或 delete 语句，并且所执行的语句所引用的游标未打开。|  
|3D000|目录名称无效|在 *StatementText* 中指定的目录名称无效。|  
|3F000|架构名称无效|在 *StatementText* 中指定的架构名称无效。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|42000|语法错误或访问冲突|\**StatementText* 包含未可准备对象或包含语法错误的 SQL 语句。<br /><br /> 用户无权执行 **StatementText* 中包含的 SQL 语句。|  
|42S01|基表或视图已存在|\**StatementText* 包含 **CREATE TABLE** 或 **CREATE VIEW** 语句，并且指定的表名称或视图名称已存在。|  
|42S02|找不到基表或视图|\**StatementText* 包含 **删除表** 或 **drop view** 语句，指定的表名或视图名不存在。<br /><br /> \**StatementText* 包含 **ALTER table** 语句，但指定的表名不存在。<br /><br /> \**StatementText* 包含 **CREATE VIEW** 语句，而查询规范定义的表名称或视图名称不存在。<br /><br /> \**StatementText* 包含 **CREATE INDEX** 语句，但指定的表名不存在。<br /><br /> \**StatementText* 包含 **GRANT** 或 **REVOKE** 语句，指定的表名或视图名不存在。<br /><br /> \**StatementText* 包含 **SELECT** 语句，但指定的表名或视图名不存在。<br /><br /> \**StatementText* 包含 **DELETE**、 **INSERT** 或 **UPDATE** 语句，但指定的表名不存在。<br /><br /> \**StatementText* 包含 **CREATE TABLE** 语句，而在约束中指定的表 (引用) 不存在的表。<br /><br /> \**StatementText* 包含 **CREATE SCHEMA** 语句，但指定的表名或视图名不存在。|  
|42S11|索引已存在|\**StatementText* 包含 **CREATE index** 语句，而指定的索引名称已存在。<br /><br /> \**StatementText* 包含 **CREATE SCHEMA** 语句，而指定的索引名称已存在。|  
|42S12|找不到索引|\**StatementText* 包含 **DROP INDEX** 语句，但指定的索引名称不存在。|  
|42S21|列已存在|\**StatementText* 包含 **ALTER TABLE** 语句，在 **ADD** 子句中指定的列不是唯一的，也不标识基表中的现有列。|  
|42S22|找不到列|\**StatementText* 包含 **CREATE INDEX** 语句，但列列表中指定的一个或多个列名称不存在。<br /><br /> \**StatementText* 包含 **GRANT** 或 **REVOKE** 语句，指定的列名称不存在。<br /><br /> \**StatementText* 包含 **SELECT**、 **DELETE**、 **INSERT** 或 **UPDATE** 语句，但指定的列名称不存在。<br /><br /> \**StatementText* 包含 **CREATE TABLE** 语句，而在约束中指定的列 (引用不存在的表) 。<br /><br /> \**StatementText* 包含 **CREATE SCHEMA** 语句，但指定的列名称不存在。|  
|44000|WITH CHECK OPTION 冲突|自变量 *StatementText* 包含对已查看的表或从已创建的表（通过指定 **WITH CHECK OPTION** 创建）执行的 **INSERT** 语句，以使该 **insert** 语句影响的一个或多个行将不再出现在查看的表中。<br /><br /> 自变量 *StatementText* 包含对已查看的表或从已查看表派生的表的 **UPDATE** 语句，该语句是通过指定 **WITH CHECK OPTION** 创建的，因此，由 **UPDATE** 语句影响的一个或多个行将不再出现在已查看的表中。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效| (DM) **StatementText* 为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLExecDirect** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 参数 *TextLength* 小于或等于0但不等于 SQL_NTS。<br /><br /> 使用 **SQLBindParameter** 设置的参数值为 null 指针，并且参数的长度值不是0、SQL_NULL_DATA、SQL_DATA_AT_EXEC、SQL_DEFAULT_PARAM 或小于或等于 SQL_LEN_DATA_AT_EXEC_OFFSET。<br /><br /> 使用 **SQLBindParameter** 设置的参数值不是 null 指针;C 数据类型为 SQL_C_BINARY 或 SQL_C_CHAR;参数长度值小于0，但未 SQL_NTS、SQL_NULL_DATA、SQL_DATA_AT_EXEC、SQL_DEFAULT_PARAM 或小于或等于 SQL_LEN_DATA_AT_EXEC_OFFSET。<br /><br /> **SQLBindParameter** 绑定的参数长度值设置为 SQL_DATA_AT_EXEC;SQL 类型是 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或 long 特定于数据源的数据类型;**SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型为 "Y"。|  
|HY105|无效的参数类型|为 **SQLBindParameter** 中的参数 *InputOutputType* 指定的值 SQL_PARAM_OUTPUT，并且该参数是一个输入参数。|  
|HY109|游标位置无效|\**StatementText* 包含定位的 update 或 delete 语句，并且游标被定位在已被删除或无法提取的行上，由 **SQLSetPos** 或 **SQLFetchScroll**)  (。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 应用程序调用 **SQLExecDirect** 将 SQL 语句发送到数据源。 有关直接执行的详细信息，请参阅 [直接执行](../../../odbc/reference/develop-app/direct-execution-odbc.md)。 驱动程序修改语句以使用数据源使用的 SQL 的形式，然后将其提交到数据源。 具体而言，驱动程序会修改用于定义 SQL 中某些功能的转义序列。 有关转义序列的语法，请参阅 [ODBC 中的转义序列](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md)。  
  
 应用程序可以在 SQL 语句中包括一个或多个参数标记。 若要包含参数标记，应用程序会将问号 (？ ) 嵌入到 SQL 语句中的适当位置。 有关参数的信息，请参阅 [语句参数](../../../odbc/reference/develop-app/statement-parameters.md)。  
  
 如果 SQL 语句为 **SELECT** 语句，并且如果应用程序调用 **SQLSetCursorName** 将游标与语句相关联，则驱动程序将使用指定的游标。 否则，驱动程序会生成一个游标名称。  
  
 如果数据源处于手动提交模式 (需要显式事务启动) 并且尚未启动事务，则驱动程序会在发送 SQL 语句之前启动事务。 有关详细信息，请参阅 [手动提交模式](../../../odbc/reference/develop-app/manual-commit-mode.md)。  
  
 如果应用程序使用 **SQLExecDirect** 提交 **COMMIT** 或 **ROLLBACK** 语句，则在 DBMS 产品之间将无法互操作。 若要提交或回滚事务，应用程序将调用 **SQLEndTran**。  
  
 如果 **SQLExecDirect** 遇到执行时数据参数，则将返回 SQL_NEED_DATA。 应用程序使用 **SQLParamData** 和 **SQLPutData** 发送数据。 请参阅 [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)、 [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)、 [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)和 [发送长数据](../../../odbc/reference/develop-app/sending-long-data.md)。  
  
 如果 **SQLExecDirect** 执行的搜索的 update、insert 或 delete 语句不影响数据源中的任何行，则对 **SQLExecDirect** 的调用将返回 SQL_NO_DATA。  
  
 如果 SQL_ATTR_PARAMSET_SIZE 语句特性的值大于1且 SQL 语句包含至少一个参数标记，则在调用 **SQLBindParameter** 时， **SQLExecDirect** 将对 *ParameterValuePointer* 参数指向的数组中的每组参数值执行一次 SQL 语句。 有关详细信息，请参阅 [参数值的数组](../../../odbc/reference/develop-app/arrays-of-parameter-values.md)。  
  
 如果已启用书签并且执行的查询无法支持书签，则驱动程序应尝试将环境强制转换为支持书签的环境，方法是更改属性值并返回更改)  (选项值。 如果无法更改该属性，则驱动程序应返回 SQLSTATE HY024 (无效属性值) 。  
  
> [!NOTE]  
>  使用连接池时，应用程序不能执行更改数据库或数据库上下文的 SQL 语句，例如 SQL Server 中 **使用** 的 _database_ 语句，这将更改数据源所使用的目录。  
  
## <a name="code-example"></a>代码示例  
 请参阅 [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)、 [SQLGETDATA](../../../odbc/reference/syntax/sqlgetdata-function.md)和 [示例 ODBC Program](../../../odbc/reference/sample-odbc-program.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|执行提交或回滚操作|[SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|提取多行数据|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|返回游标名称|[SQLGetCursorName 函数](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|提取部分或全部数据列|[SQLGetData 函数](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|返回要为其发送数据的下一个参数|[SQLParamData 函数](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|准备要执行的语句|[SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|在执行时发送参数数据|[SQLPutData 函数](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|设置游标名称|[SQLSetCursorName 函数](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
