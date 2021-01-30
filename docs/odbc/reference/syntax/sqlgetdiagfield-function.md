---
description: SQLGetDiagField 函数
title: SQLGetDiagField 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagField
helpviewer_keywords:
- SQLGetDiagField function [ODBC]
ms.assetid: 1dbc4398-97a8-4585-bb77-1f7ea75e24c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 733933691297f22934fe59e0c337dfd6093ddd3b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194191"
---
# <a name="sqlgetdiagfield-function"></a>SQLGetDiagField 函数

**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetDiagField** 返回诊断数据结构的记录字段的当前值， (与包含错误、警告和状态信息的指定句柄) 相关联。  
  
## <a name="syntax"></a>语法  
  
```cpp

SQLRETURN SQLGetDiagField(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     DiagIdentifier,  
     SQLPOINTER      DiagInfoPtr,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送一个句柄类型标识符，描述需要诊断的句柄的类型。 必须是下列选项之一：  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN 句柄仅由驱动程序管理器和驱动程序使用。 应用程序不应使用此句柄类型。 有关 SQL_HANDLE_DBC_INFO_TOKEN 的详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)。  
  
 *Handle*  
 送 *HandleType* 指示的类型的诊断数据结构的句柄。 如果 SQL_HANDLE_ENV *HandleType* ，则 *句柄* 可以是共享或非共享环境句柄。  
  
 *RecNumber*  
 送指示应用程序从中查找信息的状态记录。 状态记录从1开始编号。 如果 *DiagIdentifier* 参数指示诊断标头的任何字段，则将忽略 *RecNumber* 。 如果不是，则它应大于0。  
  
 *DiagIdentifier*  
 送指示要返回其值的诊断字段。 有关详细信息，请参阅 "备注" 中的 "*DiagIdentifier* 参数" 一节。  
  
 *DiagInfoPtr*  
 输出指向要返回诊断信息的缓冲区的指针。 数据类型取决于 *DiagIdentifier* 的值。 如果 *DiagInfoPtr* 是整数类型，则在调用此函数之前，应用程序应使用 sqlulen 生成的缓冲区并将值初始化为0，因为某些驱动程序只能写入缓冲区中的较低32位或16位，并使高阶位保持不变。  
  
 如果 *DiagInfoPtr* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的总字节数) 可在 *DiagInfoPtr* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送如果 *DiagIdentifier* 是一个 ODBC 定义的诊断，并且 *DiagInfoPtr* 指向某个字符串或二进制缓冲区，则此参数的长度应为 \* *DiagInfoPtr*。 如果 *DiagIdentifier* 是一个 ODBC 定义的字段，而 \* *DiagInfoPtr* 是一个整数，则将忽略 *BufferLength* 。 如果在调用 **SQLGetDiagFieldW**) 时， *\* DiagInfoPtr* 中的值为 Unicode 字符串 (，则 *BufferLength* 参数必须是偶数。  
  
 如果 *DiagIdentifier* 是驱动程序定义的字段，应用程序会通过设置 *BufferLength* 参数来指示该字段的特性。 *BufferLength* 可具有以下值：  
  
-   如果 *DiagInfoPtr* 是指向字符串的指针，则 *BufferLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *DiagInfoPtr* 是一个指向二进制缓冲区的指针，应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *BufferLength* 中。 这会在 *BufferLength* 中置入负值。  
  
-   如果 *DiagInfoPtr* 是一个指向字符串或二进制字符串以外的其他值的指针，则 *BufferLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *\* DiagInfoPtr* 包含固定 *长度的数据类型，则相应* 地 SQL_IS_INTEGER、SQL_IS_UINTEGER、SQL_IS_SMALLINT 或 SQL_IS_USMALLINT。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，该缓冲区返回的字节总数 (不包括 null 终止字符) 可在 DiagInfoPtr 中返回的字节数 \* （对于字符数据）。 如果可返回的字节数大于或等于 *BufferLength*，则 DiagInfoPtr 中的文本将 \* 被截断为 *BufferLength* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_NO_DATA。  
  
## <a name="diagnostics"></a>诊断  
 **SQLGetDiagField** 不会为自身发布诊断记录。 它使用以下返回值报告其自身执行的结果：  
  
-   SQL_SUCCESS：函数已成功返回诊断信息。  
  
-   SQL_SUCCESS_WITH_INFO： \* *DiagInfoPtr* 太小，无法容纳请求的诊断字段。 因此，诊断字段中的数据被截断。 若要确定是否发生了截断，应用程序必须将 *BufferLength* 与可用的实际字节数（写入 **StringLengthPtr*）进行比较。  
  
-   SQL_INVALID_HANDLE： *HandleType* 和 *句柄* 指示的句柄不是有效的句柄。  
  
-   SQL_ERROR：发生以下情况之一：  
  
    -   *DiagIdentifier* 参数不是有效值之一。  
  
    -   *DiagIdentifier* 参数 SQL_DIAG_CURSOR_ROW_COUNT、SQL_DIAG_DYNAMIC_FUNCTION、SQL_DIAG_DYNAMIC_FUNCTION_CODE 或 SQL_DIAG_ROW_COUNT，但 *句柄* 不是语句句柄。  (驱动程序管理器返回此诊断。 )   
  
    -   当 *DiagIdentifier* 指示诊断记录中的字段时 *，RecNumber* 参数为负数或0。 对于标头字段，将忽略 *RecNumber* 。  
  
    -   请求的值为字符串， *BufferLength* 小于零。  
  
    -   使用异步通知时，句柄上的异步操作不完整。  
  
-   SQL_NO_DATA： *RecNumber* 大于在 handle 中指定的句柄所存在的诊断记录数 *。* 如果没有适用于 *句柄* 的诊断记录，该函数还将为任何正 *RecNumber* 返回 SQL_NO_DATA。  
  
## <a name="comments"></a>注释  
 应用程序通常会调用 **SQLGetDiagField** 来实现以下三个目标之一：  
  
1.  当函数调用返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO (或 SQL_NEED_DATA 用于 **SQLBrowseConnect** 函数) 时，获取特定的错误或警告信息。  
  
2.  若要确定在使用 SQL_DIAG_ROW_COUNT 标头) 字段中的 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** (执行插入、删除或更新操作时受影响的数据源中的行数，或者，如果驱动程序可以从 SQL_DIAG_CURSOR_ROW_COUNT 标题字段) 提供此 (信息，则为确定当前打开的游标中的行数。  
  
3.  若要确定通过调用 **SQLExecDirect** 或 **SQLExecute** (执行哪种函数 SQL_DIAG_DYNAMIC_FUNCTION 和 SQL_DIAG_DYNAMIC_FUNCTION_CODE 标头字段) 。  
  
 任何 ODBC 函数都可以在每次调用时发布零个或多个诊断记录，因此应用程序可以在任何 ODBC 函数调用之后调用 **SQLGetDiagField** 。 可以在任何时间存储的诊断记录数没有限制。 **SQLGetDiagField** 仅检索与 *Handle* 参数中指定的诊断数据结构关联的最新诊断信息。 如果应用程序调用非 **SQLGetDiagField** 或 **SQLGetDiagRec** 的 ODBC 函数，则与相同句柄的以前的调用中的任何诊断信息都将丢失。  
  
 应用程序可以通过递增 *RecNumber* 扫描所有诊断记录，只要 **SQLGetDiagField** 返回 SQL_SUCCESS。 状态记录数在 "SQL_DIAG_NUMBER 标头" 字段中指示。 对 **SQLGetDiagField** 的调用与标头和记录字段不相同。 应用程序可以稍后再次调用 **SQLGetDiagField** 来检索记录中的字段，前提是未在过渡中调用除诊断函数之外的其他函数，这会将记录发布到同一个句柄。  
  
 应用程序可以随时调用 **SQLGetDiagField** 来返回任何诊断字段，SQL_DIAG_CURSOR_ROW_COUNT 或 SQL_DIAG_ROW_COUNT 除外，如果 *句柄* 不是语句句柄，则将返回 SQL_ERROR。 如果未定义任何其他诊断字段，则对 **SQLGetDiagField** 的调用将返回 SQL_SUCCESS (如果未遇到其他诊断) ，则会为该字段返回未定义的值。  
  
 有关详细信息，请参阅 [使用 SQLGetDiagRec 和 SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) 和 [实现 SQLGetDiagRec 和 SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md)。  
  
 如果调用的 API 不是异步执行的 API，则将生成 HY010 "函数序列错误"。 但是，在异步操作完成之前，不能检索错误记录。  
  
## <a name="handletype-argument"></a>HandleType 参数  
 每个句柄类型都可以有与之关联的诊断信息。 *HandleType* 参数指示 *句* 柄的句柄类型。  
  
 不能为环境、连接、语句和描述符句柄返回某些标头和记录字段。 对于其字段不适用的句柄，将在后面的 "标头字段" 和 "记录字段" 部分中指明。  
  
 如果 SQL_HANDLE_ENV *HandleType* ，则 *句柄* 可以是共享或非共享环境句柄。  
  
 不应将驱动程序特定的标头诊断字段与环境句柄关联。  
  
 为描述符句柄定义的诊断标头字段 SQL_DIAG_NUMBER 和 SQL_DIAG_RETURNCODE。  
  
## <a name="diagidentifier-argument"></a>DiagIdentifier 参数  
 此参数指示诊断数据结构中所需字段的标识符。 如果 *RecNumber* 大于或等于1，则字段中的数据将描述函数返回的诊断信息。 如果 *RecNumber* 为0，则字段位于诊断数据结构的标头中，因此包含与返回诊断信息的函数调用相关的数据，而不是有关特定信息的数据。  
  
 驱动程序可以在诊断数据结构中定义特定于驱动程序的标头和记录字段。  
  
 使用 ODBC 2.x *驱动程序的 odbc 1.x 应用程序* 将只能使用 SQL_DIAG_CLASS_ORIGIN、SQL_DIAG_CLASS_SUBCLASS_ORIGIN、SQL_DIAG_CONNECTION_NAME、SQL_DIAG_MESSAGE_TEXT、SQL_DIAG_NATIVE、SQL_DIAG_NUMBER、SQL_DIAG_RETURNCODE、SQL_DIAG_SERVER_NAME 或 SQL_DIAG_SQLSTATE 的 *DiagIdentifier* 参数调用 **SQLGetDiagField** 。 所有其他诊断字段都将返回 SQL_ERROR。  
  
## <a name="header-fields"></a>标头字段  
 下表中列出的标头字段可包含在 *DiagIdentifier* 参数中。  
  
|DiagIdentifier|返回类型|返回|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CURSOR_ROW_COUNT|SQLLEN|此字段包含游标中的行数。 其语义取决于 **SQLGetInfo** 信息类型 SQL_DYNAMIC_CURSOR_ATTRIBUTES2、SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2、SQL_KEYSET_CURSOR_ATTRIBUTES2 和 SQL_STATIC_CURSOR_ATTRIBUTES2，指示哪些行计数可用于 (SQL_CA2_CRC_EXACT 和 SQL_CA2_CRC_APPROXIMATE 位) 中的每个游标类型。<br /><br /> 此字段的内容仅用于语句句柄，且仅在调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** 后定义。 使用不是语句句柄的 SQL_DIAG_CURSOR_ROW_COUNT 的 *DiagIdentifier* 调用 **SQLGetDiagField** 将返回 SQL_ERROR。|  
|SQL_DIAG_DYNAMIC_FUNCTION|SQLCHAR|这是描述基础函数所执行的 SQL 语句的字符串。  (参阅本部分后面的 "动态函数字段的值"，以获取特定值。 ) 此字段的内容仅用于语句句柄，且仅在调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** 后定义。 使用不是语句句柄的 SQL_DIAG_DYNAMIC_FUNCTION 的 *DiagIdentifier* 调用 **SQLGetDiagField** 将返回 SQL_ERROR。 在对 **SQLExecute** 或 **SQLExecDirect** 的调用之前，此字段的值是不确定的。|  
|SQL_DIAG_DYNAMIC_FUNCTION_CODE|SQLINTEGER|这是一个数字代码，用于描述基础函数所执行的 SQL 语句。  (参阅本部分后面的 "动态函数字段的值"，以获取特定值。 ) 此字段的内容仅用于语句句柄，且仅在调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** 后定义。 使用不是语句句柄的 SQL_DIAG_DYNAMIC_FUNCTION_CODE 的 *DiagIdentifier* 调用 **SQLGetDiagField** 将返回 SQL_ERROR。 在对 **SQLExecute** 或 **SQLExecDirect** 的调用之前，此字段的值是不确定的。|  
|SQL_DIAG_NUMBER|SQLINTEGER|可用于指定句柄的状态记录数。|  
|SQL_DIAG_RETURNCODE|SQLRETURN|返回由函数返回的代码。 有关返回代码的列表，请参阅 [返回代码](../../../odbc/reference/develop-app/return-codes-odbc.md)。 驱动程序不必实现 SQL_DIAG_RETURNCODE;它始终由驱动程序管理器实现。 如果尚未对 *句柄* 调用函数，将返回 SQL_DIAG_RETURNCODE 的 SQL_SUCCESS。|  
|SQL_DIAG_ROW_COUNT|SQLLEN|由 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 执行的插入、删除或更新所影响的行数。 它在执行 *游标规范* 后定义了驱动程序。 此字段的内容仅定义为语句句柄。 使用不是语句句柄的 SQL_DIAG_ROW_COUNT 的 *DiagIdentifier* 调用 **SQLGetDiagField** 将返回 SQL_ERROR。 此字段中的数据也会在 **SQLRowCount** 的 *RowCountPtr* 参数中返回。 此字段中的数据将在每次 nondiagnostic 函数调用后重置，而 **SQLRowCount** 返回的行计数保持不变，直到将该语句设置回准备就绪或已分配状态。|  
  
## <a name="record-fields"></a>记录字段  
 下表中列出的记录字段可包含在 *DiagIdentifier* 参数中。  
  
|DiagIdentifier|返回类型|返回|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CLASS_ORIGIN|SQLCHAR|一个字符串，指示定义此记录中 SQLSTATE 值的类部分的文档。 对于开放组和 ISO 调用级别接口定义的所有 SQLSTATEs，其值为 "ISO 9075"。 对于特定于 ODBC 的 SQLSTATEs (其 SQLSTATE 类为 "IM" ) 的所有项，其值为 "ODBC 3.0"。|  
|SQL_DIAG_COLUMN_NUMBER|SQLINTEGER|如果 SQL_DIAG_ROW_NUMBER 字段是行集或一组参数中的有效行号，则此字段包含的值表示结果集中的列号或参数集中的参数编号。 结果集列号始终从1开始;如果此状态记录与书签列有关，则字段可以为零。 参数编号从1开始。 如果状态记录未与列号或参数号关联，则它的值 SQL_NO_COLUMN_NUMBER。 如果驱动程序无法确定与此记录关联的列号或参数编号，则此字段的值为 SQL_COLUMN_NUMBER_UNKNOWN。<br /><br /> 此字段的内容仅定义为语句句柄。|  
|SQL_DIAG_CONNECTION_NAME|SQLCHAR|一个字符串，指示与诊断记录相关的连接的名称。 此字段由驱动程序定义。 对于与环境句柄关联的诊断数据结构以及与任何连接无关的诊断，此字段是长度为零的字符串。|  
|SQL_DIAG_MESSAGE_TEXT|SQLCHAR|有关错误或警告的信息性消息。 此字段的格式如 [诊断消息](../../../odbc/reference/develop-app/diagnostic-messages.md)中所述。 诊断消息文本没有最大长度。|  
|SQL_DIAG_NATIVE|SQLINTEGER|驱动程序/数据源特定的本机错误代码。 如果没有本机错误代码，驱动程序将返回0。|  
|SQL_DIAG_ROW_NUMBER|SQLLEN|此字段包含行集中的行号，或与状态记录关联的参数集中的参数编号。 行号和参数号从1开始。 如果此状态记录未与行号或参数号关联，则此字段的值 SQL_NO_ROW_NUMBER。 如果驱动程序无法确定与此记录关联的行号或参数号，则此字段的值为 SQL_ROW_NUMBER_UNKNOWN。<br /><br /> 此字段的内容仅定义为语句句柄。|  
|SQL_DIAG_SERVER_NAME|SQLCHAR|一个字符串，指示与诊断记录相关的服务器名称。 它与使用 SQL_DATA_SOURCE_NAME 选项对 **SQLGetInfo** 的调用返回的值相同。 对于与环境句柄关联的诊断数据结构以及与任何服务器无关的诊断，此字段是长度为零的字符串。|  
|SQL_DIAG_SQLSTATE|SQLCHAR|五个字符的 SQLSTATE 诊断代码。 有关详细信息，请参阅 [SQLSTATEs](../../../odbc/reference/develop-app/sqlstates.md)。|  
|SQL_DIAG_SUBCLASS_ORIGIN|SQLCHAR|具有与 SQL_DIAG_CLASS_ORIGIN 相同的格式和有效值的字符串，用于标识 SQLSTATE 代码的子类部分的定义部分。 返回 "ODBC 3.0" 的特定于 ODBC 的 SQLSTATES 包括以下内容：<br /><br /> 01S00、01S01、01S02、01S06、01S07、07S01、08S01、21S01、21S02、25S01、25S02、25S03、42S01、42S02、42S11、42S12、42S21、42S22、HY095、HY097、HY098、HY099、HY100、HY101、HY105、HY107、HY109、HY110、HY111、HYT00、HYT01、IM001、IM002、IM003、IM004、IM005、IM006、IM007、IM008、IM010、IM011、IM012。|  
  
## <a name="values-of-the-dynamic-function-fields"></a>动态函数字段的值  
 下表描述了应用于通过调用 **SQLExecute** 或 **SQLExecDirect** 执行的每种 SQL 语句的 SQL_DIAG_DYNAMIC_FUNCTION 和 SQL_DIAG_DYNAMIC_FUNCTION_CODE 的值。 驱动程序可以将驱动程序定义的值添加到列出的值。  
  
|SQL 语句<br /><br /> 已执行| 的值<br /><br /> SQL_DIAG_DYNAMIC_FUNCTION| 的值<br /><br /> SQL_DIAG_DYNAMIC_FUNCTION_CODE|  
|--------------------------------|-----------------------------------------------|-----------------------------------------------------|  
|*alter-domain 语句*|"ALTER DOMAIN"|SQL_DIAG_ALTER_DOMAIN|  
|*alter table 语句*|"ALTER TABLE"|SQL_DIAG_ALTER_TABLE|  
|*断言-定义*|"创建断言"|SQL_DIAG_CREATE_ASSERTION|  
|*字符集-定义*|"创建字符集"|SQL_DIAG_CREATE_CHARACTER_SET|  
|*排序规则定义*|"创建排序规则"|SQL_DIAG_CREATE_COLLATION|  
|*n n-定义*|"创建域"|SQL_DIAG_CREATE_DOMAIN|
|*create-index 语句*|"创建索引"|SQL_DIAG_CREATE_INDEX|  
|*create-table 语句*|"CREATE TABLE"|SQL_DIAG_CREATE_TABLE|  
|*create-view-语句*|"创建视图"|SQL_DIAG_CREATE_VIEW|  
|*游标规范*|"SELECT CURSOR"|SQL_DIAG_SELECT_CURSOR|  
|*删除-语句定位*|"动态删除游标"|SQL_DIAG_DYNAMIC_DELETE_CURSOR|  
|*delete-已搜索语句*|"DELETE WHERE"|SQL_DIAG_DELETE_WHERE|  
|*drop-assertion 语句*|"DROP ASSERTION"|SQL_DIAG_DROP_ASSERTION|  
|*drop-字符集-stmt*|"DROP 字符集"|SQL_DIAG_DROP_CHARACTER_SET|  
|*drop-排序规则语句*|"删除排序规则"|SQL_DIAG_DROP_COLLATION|  
|*drop domain 语句*|"DROP DOMAIN"|SQL_DIAG_DROP_DOMAIN|  
|*drop index 语句*|"DROP INDEX"|SQL_DIAG_DROP_INDEX|  
|*drop 架构语句*|"删除架构"|SQL_DIAG_DROP_SCHEMA|  
|*drop table 语句*|"删除表"|SQL_DIAG_DROP_TABLE|  
|*drop-平移语句*|"DROP 翻译"|SQL_DIAG_DROP_TRANSLATION|  
|*拖放-view 语句*|"DROP VIEW"|SQL_DIAG_DROP_VIEW|  
|*grantstatement*|准予|SQL_DIAG_GRANT|
|*insert 语句*|&|SQL_DIAG_INSERT|  
|*ODBC-过程扩展*|拨|SQL_DIAG_ 调用|  
|*revoke 语句*|收回|SQL_DIAG_REVOKE|  
|*架构定义*|"创建架构"|SQL_DIAG_CREATE_SCHEMA|  
|*翻译-定义*|"创建翻译"|SQL_DIAG_CREATE_TRANSLATION|  
|*更新语句定位*|"动态更新游标"|SQL_DIAG_DYNAMIC_UPDATE_CURSOR|  
|*update-搜索*|"UPDATE WHERE"|SQL_DIAG_UPDATE_WHERE|  
|Unknown|*空字符串*|SQL_DIAG_UNKNOWN_STATEMENT|  

<!--
These two malformed table rows were fixed by educated GUESS only.
Each pair starts with the original flawed row.
Flawed because treated as only two cells by HTML render,
and because missing info anyway.
Also, these flawed rows lacked '|' as their first nonWhitespace character (although markdown technically allows this omission, unfortunately).
Arguably the following SQL.H file shows the sequence of the flawed rows in the table was suboptimal also.

ftp://www.fpc.org/fpc32/VS6Disk1/VC98/INCLUDE/SQL.H

GeneMi , 2019/01/19
- - - - - - - - - - - - - -

n-definition*|"CREATE DOMAIN"|SQL_DIAG_CREATE_DOMAIN|  

|*domain-definition*|"CREATE DOMAIN"|SQL_DIAG_CREATE_DOMAIN|

-statement*|"GRANT"|SQL_DIAG_GRANT|  

|*grant-statement*|"GRANT"|SQL_DIAG_GRANT|

-->

## <a name="sequence-of-status-records"></a>状态记录的序列

 根据行号和诊断类型将状态记录定位在一个序列中。 驱动程序管理器决定返回其生成的状态记录的最终顺序。 驱动程序确定返回其生成的状态记录的最终顺序。  
  
 如果诊断记录由驱动程序管理器和驱动程序发布，则驱动程序管理器负责对它们进行排序。  
  
 如果有两个或更多状态记录，则首先按行号确定记录的顺序。 以下规则适用于按行确定诊断记录的顺序：  
  
-   不与任何行对应的记录显示在与特定行对应的记录前面，因为 SQL_NO_ROW_NUMBER 定义为-1。  
  
-   行号未知的记录将显示在所有其他记录前面，因为 SQL_ROW_NUMBER_UNKNOWN 定义为-2。  
  
-   对于属于特定行的所有记录，记录按 "SQL_DIAG_ROW_NUMBER" 字段中的值进行排序。 列出了受影响的第一行的所有错误和警告，并列出了受影响的下一行的所有错误和警告，依此类推。  
  
> [!NOTE]
>  Odbc 2.x 驱动 *程序管理* 器不会在诊断队列中对状态记录进行排序，因为) 在 odbc 2.x 驱动程序返回了 SQLSTATE 01S01 (错误，或者，如果 SQLSTATE (01S01 在) 中 **调用了** **SQLExtendedFetch** ，则 ODBC 1.x 驱动程序 *会在 odbc* 1.x *驱动程序返回 SQLSetPos 时返回* **SQLExtendedFetch。**  
  
 在每行中，或对于行号不是其行号的所有记录，或者对于行号等于 SQL_NO_ROW_NUMBER 的所有记录，将使用一组排序规则确定列出的第一条记录。 在第一条记录之后，影响行的其他记录的顺序是不确定的。 在第一条记录之后，应用程序不能假定错误之前出现警告。 应用程序应扫描完整的诊断数据结构，以获取有关函数的失败调用的完整信息。  
  
 下面的规则用于确定行中的第一条记录。 排名最高的记录是第一条记录。 对记录进行排序时，不考虑 (驱动程序管理器、驱动程序和网关等) 记录的源。  
  
-   **错误** 描述错误的状态记录的排名最高。 以下规则适用于排序错误：  
  
    -   指示事务失败或可能事务失败的记录 outrank 所有其他记录。  
  
    -   如果两个或更多记录描述相同的错误条件，则由开放式组 CLI 规范定义的 SQLSTATEs 将 (类03到 HZ) outrank ODBC 和驱动程序定义的 SQLSTATEs。  
  
-   **实现-未定义数据值** 用于描述驱动程序定义的不含数据值 (类 02) 的状态记录具有第二个最高级别。  
  
-   **警告** 描述 (类 01) 的警告的状态记录的排名最低。 如果两个或更多记录描述相同的警告条件，则由开放式组 CLI 规范定义的警告 SQLSTATEs 由 outrank ODBC 定义的 SQLSTATEs 和驱动程序定义的。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|获取诊断数据结构的多个字段|[SQLGetDiagRec 函数](sqlgetdiagrec-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
