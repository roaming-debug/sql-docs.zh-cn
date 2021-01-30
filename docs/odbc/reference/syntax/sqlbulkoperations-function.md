---
description: SQLBulkOperations 函数
title: SQLBulkOperations 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBulkOperations
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBulkOperations
helpviewer_keywords:
- SQLBulkOperations function [ODBC]
ms.assetid: 7029d0da-b0f2-44e6-9114-50bd96f47196
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a95e7701b12f9673452962c005a7bac44701e2d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181061"
---
# <a name="sqlbulkoperations-function"></a>SQLBulkOperations 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLBulkOperations** 执行批量插入和批量书签操作，包括更新、删除和按书签提取。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLBulkOperations(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Operation);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *操作*  
 送要执行的操作：  
  
 SQL_ADD SQL_UPDATE_BY_BOOKMARK SQL_DELETE_BY_BOOKMARK SQL_FETCH_BY_BOOKMARK  
  
 有关详细信息，请参阅 "注释"。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NEED_DATA、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLBulkOperations** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLBulkOperations** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
 对于可以返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR (除了 01xxx SQLSTATEs) 的所有这些 SQLSTATEs，如果一个或多个（但不是全部）多行操作行出现错误，则返回 SQL_SUCCESS_WITH_INFO，如果单行操作出现错误，则返回 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据右截断|*操作* 参数为 SQL_FETCH_BY_BOOKMARK，为数据类型为 SQL_C_CHAR 或 SQL_C_BINARY 的一列或多列返回的字符串或二进制数据导致截断非空白字符或非 NULL 二进制数据。|  
|01S01|行中的错误|*操作* 参数已 SQL_ADD，在执行该操作时，一个或多个行中出现错误，但至少已成功添加一行。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) <br /><br />  (仅当应用程序使用 ODBC 2 时才引发此错误。*x* 驱动程序。 ) |  
|01S07|小数截断|*操作* 参数已 SQL_FETCH_BY_BOOKMARK，应用程序缓冲区的数据类型未 SQL_C_CHAR 或 SQL_C_BINARY，并且为一个或多个列返回到应用程序缓冲区的数据被截断。 对于数字 C 数据类型 (，数字的小数部分被截断。 对于包含时间部分的时间、时间戳和时间间隔 C 数据类型，时间的小数部分将被截断。 ) <br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|*操作* 参数已 SQL_FETCH_BY_BOOKMARK，因此，在对 **SQLBindCol** 的调用中，结果集中列的数据值无法转换为 *TargetType* 参数指定的数据类型。<br /><br /> *操作* 参数 SQL_UPDATE_BY_BOOKMARK 或 SQL_ADD，应用程序缓冲区中的数据值无法转换为结果集中列的数据类型。|  
|07009|描述符索引无效|参数 *操作* 是 SQL_ADD 的，并且列的列号大于结果集中的列数。|  
|21S02|派生表的等级与列列表不匹配|SQL_UPDATE_BY_BOOKMARK 了参数 *操作* ;由于所有列均为未绑定或只读，或者绑定长度/指示器缓冲区中的值 SQL_COLUMN_IGNORE，因此没有可更新的列。|  
|22001|字符串数据右截断|将字符或二进制值分配到结果集中的列时，将导致空字符) 或非 null (二进制) 字符或字节的非空白 (。|  
|22003|数值超出范围|*操作* 参数是 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK，而对结果集中的某一列的数值赋值导致了整个 (，而不是要截断的数字) 部分的小数部分。<br /><br /> 参数 *操作* 是 SQL_FETCH_BY_BOOKMARK 的，返回一个或多个绑定列的数值将导致有效位丢失。|  
|22007|Datetime 格式无效|*操作* 参数是 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK，而对结果集中的某列的日期或时间戳值赋值导致年、月或日字段超出范围。<br /><br /> 参数 *操作* 是 SQL_FETCH_BY_BOOKMARK 的，并且为一个或多个绑定列返回日期或时间戳值将导致年、月或日字段超出范围。|  
|22008|日期/时间字段溢出|*操作* 参数是 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK 的，而对发送到结果集中的列的数据进行日期时间运算的性能的结果是： (年、月、日、小时、分钟或) 秒字段的日期时间字段，该字段与字段的允许值范围以外的结果或基于公历的 datetime 自然规则无效。<br /><br /> *操作* 参数已 SQL_FETCH_BY_BOOKMARK，而从结果集检索到的数据的 datetime 算法的性能导致了日期时间字段 ("年"、"月"、"日"、"小时"、"分钟" 或 ") 秒" 字段，结果为字段的允许范围外，或者根据公历的 datetime 自然规则无效。|  
|22015|间隔字段溢出|*操作* 参数 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK，并且将精确数字或间隔 C 类型分配给时间间隔 SQL 数据类型导致了有效位丢失。<br /><br /> *操作* 参数 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK;当分配给某个间隔 SQL 类型时，interval SQL 类型中不存在 C 类型的值的表示形式。<br /><br /> *操作* 参数是 SQL_FETCH_BY_BOOKMARK 的，并且从精确数值或间隔 SQL 类型分配到 interval C 类型会导致前导字段的有效位丢失。<br /><br /> *操作* 参数 SQL_FETCH_BY_BOOKMARK;当分配到某个时间间隔 C 类型时，在 "C #" 类型的 "SQL 类型" 值中没有表示形式。|  
|22018|转换规范的字符值无效|*操作* 参数 SQL_FETCH_BY_BOOKMARK;C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列中的值不是绑定 C 类型的有效文本。<br /><br /> 参数 *操作* 是 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK;SQL 类型是精确或近似的数字、日期时间或时间间隔数据类型;C 类型为 SQL_C_CHAR;列中的值不是绑定的 SQL 类型的有效文本。|  
|23000|完整性约束冲突|*操作* 参数 SQL_ADD、SQL_DELETE_BY_BOOKMARK 或 SQL_UPDATE_BY_BOOKMARK，并违反了完整性约束。<br /><br /> *操作* 参数已 SQL_ADD，且未绑定的列定义为 not NULL 且没有默认值。<br /><br /> *操作* 参数已 SQL_ADD，绑定 *StrLen_or_IndPtr* 缓冲区中指定的长度 SQL_COLUMN_IGNORE，列没有默认值。|  
|24000|无效的游标状态|*StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。|  
|40001|序列化失败|由于另一个事务发生资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|42000|语法错误或访问冲突|驱动程序无法根据需要锁定行以执行 *操作* 参数中请求的操作。|  
|44000|WITH CHECK OPTION 冲突|*操作* 参数为 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK，并对已查看的表 (或从已查看) 表（通过指定 **WITH CHECK OPTION** 创建）中派生的表执行了插入或更新，这样一来，插入或更新操作的一个或多个行将不再出现在查看的表中。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLBulkOperations** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 指定的 *StatementHandle* 未处于执行状态。 调用函数时，无需先调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 该驱动程序是一个 ODBC 2。在调用 **SQLFetchScroll** 或 **SQLFetch** 之前，对 *StatementHandle* 调用了 *x* 驱动程序和 **SQLBulkOperations** 。<br /><br /> 在 *StatementHandle* 上调用 **SQLExtendedFetch** 后调用 (DM) **SQLBulkOperations** 。|  
|HY011|现在无法设置属性| (DM) 该驱动程序是一个 ODBC 2。*x* 驱动程序，并在对 **SQLFetch** 或 **SQLFetchScroll** 和 **SQLBulkOperations** 的调用之间设置了 SQL_ATTR_ROW_STATUS_PTR 语句特性。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效|*操作* 参数 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK;数据值不是 null 指针;C 数据类型为 SQL_C_BINARY 或 SQL_C_CHAR;列长度值小于0，但不等于 SQL_DATA_AT_EXEC、SQL_COLUMN_IGNORE、SQL_NTS 或 SQL_NULL_DATA，或者小于或等于 SQL_LEN_DATA_AT_EXEC_OFFSET。<br /><br /> SQL_DATA_AT_EXEC 的长度/指示器缓冲区中的值;SQL 类型是 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或 long 特定于数据源的数据类型; **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型为 "Y"。<br /><br /> *操作* 参数已 SQL_ADD，SQL_ATTR_USE_BOOKMARK 语句特性设置为 SQL_UB_VARIABLE，而列0绑定到的缓冲区的长度不等于此结果集的书签的最大长度。  (此长度在 IRD 的 SQL_DESC_OCTET_LENGTH 字段中提供，可以通过调用 **SQLDescribeCol**、 **SQLColAttribute** 或 **SQLGetDescField** 来获取。 ) |  
|HY092|无效的属性标识符| (DM) 为 *操作* 参数指定的值无效。<br /><br /> *操作* 参数 SQL_ADD、SQL_UPDATE_BY_BOOKMARK 或 SQL_DELETE_BY_BOOKMARK，并且 SQL_ATTR_CONCURRENCY 语句特性设置为 SQL_CONCUR_READ_ONLY。<br /><br /> *操作* 参数 SQL_DELETE_BY_BOOKMARK、SQL_FETCH_BY_BOOKMARK 或 SQL_UPDATE_BY_BOOKMARK，并且书签列未绑定或 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 *操作* 参数中请求的操作。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置为 SQL_ATTR_QUERY_TIMEOUT 的 *属性* 参数。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
  
> [!CAUTION]  
>  有关可在其中调用的语句状态 **SQLBulkOperations** 以及与 ODBC 2 兼容的必须执行的操作的信息。*x* 应用程序的详细说明，请参阅附录 G：驱动程序准则中的 [块游标、可滚动游标和后向](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) 兼容性部分。  
  
 应用程序使用 **SQLBulkOperations** 对与当前查询对应的基表或视图执行以下操作：  
  
-   添加新行。  
  
-   更新一组行，其中每行由书签标识。  
  
-   删除一组行，其中每行由书签标识。  
  
-   提取一组行，其中每行由书签标识。  
  
 调用 **SQLBulkOperations** 后，不定义块光标位置。 应用程序必须调用 **SQLFetchScroll** 来设置光标位置。 应用程序仅应使用 SQL_FETCH_FIRST、SQL_FETCH_LAST、SQL_FETCH_ABSOLUTE 或 SQL_FETCH_BOOKMARK 的 *FetchOrientation* 参数调用 **SQLFetchScroll** 。 如果应用程序使用 SQL_FETCH_PRIOR、SQL_FETCH_NEXT 或 SQL_FETCH_RELATIVE 的 *FetchOrientation* 参数调用 **SQLFetch** 或 **SQLFetchScroll** ，则游标位置未定义。  
  
 通过将对 **SQLBindCol** 的调用中指定的列长度/指示器缓冲区设置为 SQL_COLUMN_IGNORE，可以在通过调用 **SQLBulkOperations** 执行的批量操作中忽略列。  
  
 应用程序在调用 **SQLBulkOperations** 时不需要设置 SQL_ATTR_ROW_OPERATION_PTR 语句特性，因为在对此函数执行大容量操作时，不能忽略行。  
  
 SQL_ATTR_ROWS_FETCHED_PTR 语句属性所指向的缓冲区包含由对 **SQLBulkOperations** 的调用影响的行数。  
  
 当 *操作* 参数为 SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK 并且与游标关联的查询规范的选择列表包含对同一列的多个引用时，无论是否生成错误或驱动程序将忽略重复的引用并执行所请求的操作，都是由驱动程序定义的。  
  
 有关如何使用 **SQLBulkOperations** 的详细信息，请参阅 [使用 SQLBulkOperations 更新数据](../../../odbc/reference/develop-app/updating-data-with-sqlbulkoperations.md)。  
  
## <a name="performing-bulk-inserts"></a>执行大容量插入  
 若要使用 **SQLBulkOperations** 插入数据，应用程序需要执行以下一系列步骤：  
  
1.  执行返回结果集的查询。  
  
2.  将 SQL_ATTR_ROW_ARRAY_SIZE 语句特性设置为要插入的行数。  
  
3.  调用 **SQLBindCol** 绑定要插入的数据。 数据绑定到大小等于 SQL_ATTR_ROW_ARRAY_SIZE 的值的数组。  
  
    > [!NOTE]  
    >  SQL_ATTR_ROW_STATUS_PTR 语句特性指向的数组大小应等于 SQL_ATTR_ROW_ARRAY_SIZE 或 SQL_ATTR_ROW_STATUS_PTR 应为 null 指针。  
  
4.  调用 **SQLBulkOperations** (*StatementHandle，* SQL_ADD) 执行插入。  
  
5.  如果应用程序已设置 SQL_ATTR_ROW_STATUS_PTR 语句特性，则它可以检查此数组以查看操作的结果。  
  
 如果应用程序在使用 SQL_ADD 的 *操作* 参数调用 **SQLBulkOperations** 之前绑定列0，则驱动程序将使用新插入行的书签值更新绑定列0缓冲区。 为此，应用程序必须先将 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，然后再执行该语句。  (这对 ODBC 2 不起作用。*x* 驱动程序。 )   
  
 使用对 SQLParamData 和 SQLPutData 的调用，可以通过 SQLBulkOperations 将长数据添加到部分。 有关详细信息，请参阅本函数引用后面的 "为大容量插入和更新提供长数据"。  
  
 在调用 **SQLBulkOperations** (之前，应用程序无需调用 **SQLFetch** 或 **SQLFetchScroll** ，除非针对 ODBC 2 进行。*x* 驱动程序;请参阅 [向后兼容性和标准符合性](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md)) 。  
  
 如果 **SQLBulkOperations** 具有 SQL_ADD 的 *操作* 参数，则对包含重复列的游标调用，此行为是由驱动程序定义的。 驱动程序可以返回驱动程序定义的 SQLSTATE，将数据添加到结果集中显示的第一列，或者执行其他驱动程序定义的行为。  
  
## <a name="performing-bulk-updates-by-using-bookmarks"></a>使用书签执行批量更新  
 若要通过 **SQLBulkOperations** 使用书签来执行批量更新，应用程序需要按顺序执行以下步骤：  
  
1.  将 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE。  
  
2.  执行返回结果集的查询。  
  
3.  将 SQL_ATTR_ROW_ARRAY_SIZE 语句特性设置为它要更新的行数。  
  
4.  调用 **SQLBindCol** 绑定要更新的数据。 数据绑定到大小等于 SQL_ATTR_ROW_ARRAY_SIZE 的值的数组。 它还会调用 **SQLBindCol** 将列 0 (书签列绑定) 。  
  
5.  将要更新的行的书签复制到绑定到列0的数组中。  
  
6.  更新绑定缓冲区中的数据。  
  
    > [!NOTE]  
    >  SQL_ATTR_ROW_STATUS_PTR 语句特性指向的数组大小应等于 SQL_ATTR_ROW_ARRAY_SIZE 或 SQL_ATTR_ROW_STATUS_PTR 应为 null 指针。  
  
7.  调用 **SQLBulkOperations** (*StatementHandle* SQL_UPDATE_BY_BOOKMARK) 。  
  
    > [!NOTE]  
    >  如果应用程序已设置 SQL_ATTR_ROW_STATUS_PTR 语句特性，则它可以检查此数组以查看操作的结果。  
  
8.  可以选择调用 **SQLBulkOperations** (*StatementHandle*，SQL_FETCH_BY_BOOKMARK) 将数据提取到绑定的应用程序缓冲区中，以验证是否已发生更新。  
  
9. 如果数据已更新，驱动程序将更改行状态数组中的值，以使相应的行 SQL_ROW_UPDATED。  
  
 通过使用对 **SQLParamData** 和 **SQLPutData** 的调用， **SQLBulkOperations** 执行的大容量更新可以包含长数据。 有关详细信息，请参阅本函数引用后面的 "为大容量插入和更新提供长数据"。  
  
 如果书签在游标之间保持不变，则该应用程序在更新书签之前无需调用 **SQLFetch** 或 **SQLFetchScroll** 。 它可以使用从上一个游标存储的书签。 如果书签不是跨游标保存的，则应用程序必须调用 **SQLFetch** 或 **SQLFetchScroll** 来检索书签。  
  
 如果 **SQLBulkOperations** 具有 SQL_UPDATE_BY_BOOKMARK 的 *操作* 参数，则对包含重复列的游标调用，此行为是由驱动程序定义的。 驱动程序可以返回驱动程序定义的 SQLSTATE，更新出现在结果集中的第一列，或者执行其他驱动程序定义的行为。  
  
## <a name="performing-bulk-fetches-using-bookmarks"></a>使用书签执行批量提取  
 若要在 **SQLBulkOperations** 中使用书签执行批量提取，应用程序需要按顺序执行以下步骤：  
  
1.  将 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE。  
  
2.  执行返回结果集的查询。  
  
3.  将 SQL_ATTR_ROW_ARRAY_SIZE 语句特性设置为它要提取的行数。  
  
4.  调用 **SQLBindCol** 绑定要提取的数据。 数据绑定到大小等于 SQL_ATTR_ROW_ARRAY_SIZE 的值的数组。 它还会调用 **SQLBindCol** 将列 0 (书签列绑定) 。  
  
5.  将感兴趣的行的书签复制到绑定到列0的数组中。  (这假定应用程序已单独获取书签。 )   
  
    > [!NOTE]  
    >  SQL_ATTR_ROW_STATUS_PTR 语句特性指向的数组大小应等于 SQL_ATTR_ROW_ARRAY_SIZE 或 SQL_ATTR_ROW_STATUS_PTR 应为 null 指针。  
  
6.  调用 **SQLBulkOperations** (*StatementHandle* SQL_FETCH_BY_BOOKMARK) 。  
  
7.  如果应用程序已设置 SQL_ATTR_ROW_STATUS_PTR 语句特性，则它可以检查此数组以查看操作的结果。  
  
 如果书签在游标之间保持不变，则在提取书签之前，应用程序无需调用 **SQLFetch** 或 **SQLFetchScroll** 。 它可以使用从上一个游标存储的书签。 如果书签不是跨游标保存的，则应用程序必须调用 **SQLFetch** 或 **SQLFetchScroll** 一次来检索书签。  
  
## <a name="performing-bulk-deletes-using-bookmarks"></a>使用书签执行批量删除  
 若要在 **SQLBulkOperations** 中使用书签执行批量删除，应用程序将按顺序执行以下步骤：  
  
1.  将 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE。  
  
2.  执行返回结果集的查询。  
  
3.  将 SQL_ATTR_ROW_ARRAY_SIZE 语句特性设置为要删除的行数。  
  
4.  调用 **SQLBindCol** 将列0绑定 (书签列) 。  
  
5.  将要删除的行的书签复制到绑定到列0的数组中。  
  
    > [!NOTE]  
    >  SQL_ATTR_ROW_STATUS_PTR 语句特性指向的数组大小应等于 SQL_ATTR_ROW_ARRAY_SIZE 或 SQL_ATTR_ROW_STATUS_PTR 应为 null 指针。  
  
6.  调用 **SQLBulkOperations** (*StatementHandle* SQL_DELETE_BY_BOOKMARK) 。  
  
7.  如果应用程序已设置 SQL_ATTR_ROW_STATUS_PTR 语句特性，则它可以检查此数组以查看操作的结果。  
  
 如果书签在游标之间保持不变，则在删除书签之前，应用程序无需调用 **SQLFetch** 或 **SQLFetchScroll** 。 它可以使用从上一个游标存储的书签。 如果书签不是跨游标保存的，则应用程序必须调用 **SQLFetch** 或 **SQLFetchScroll** 一次来检索书签。  
  
## <a name="providing-long-data-for-bulk-inserts-and-updates"></a>为大容量插入和更新提供长数据  
 可以为大容量插入和通过调用 **SQLBulkOperations** 执行的更新提供长数据。 若要插入或更新长数据，应用程序将执行以下步骤，以及本主题前面的 "执行批量插入" 和 "使用书签执行批量更新" 部分中描述的步骤。  
  
1.  当它通过使用 **SQLBindCol** 绑定数据时，应用程序会将应用程序定义的值（如列号）放置在 *\* TargetValuePtr* 缓冲区中，以执行执行时数据列。 稍后可使用该值来识别列。  
  
     应用程序将 SQL_LEN_DATA_AT_EXEC (*长度*) 宏的结果放置在 *\* StrLen_or_IndPtr* 缓冲区中。 如果列的 SQL 数据类型为 SQL_LONGVARBINARY、SQL_LONGVARCHAR 或 long 特定于数据源的数据类型，并且驱动程序为 **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型返回 "Y"，则 *length* 是要为参数发送的数据的字节数;否则，它必须为非负值并且将被忽略。  
  
2.  调用 **SQLBulkOperations** 时，如果存在执行时数据列，该函数将返回 SQL_NEED_DATA 并继续执行到步骤3，如下所示。  (如果没有执行时数据列，则该过程已完成。 )   
  
3.  应用程序调用 **SQLParamData** 来检索要处理的第一次执行时数据列的 *\* TargetValuePtr* 缓冲区地址。 **SQLParamData** 返回 SQL_NEED_DATA。 应用程序从 *\* TargetValuePtr* 缓冲区检索应用程序定义的值。  
  
    > [!NOTE]  
    >  尽管执行时数据参数与执行时数据列类似，但 **SQLParamData** 返回的值对于每个都是不同的。  
  
     执行时数据列是行集中的列，当使用 **SQLBulkOperations** 更新或插入行时，将使用 **SQLPutData** 发送数据。 它们与 **SQLBindCol** 绑定在一起。 **SQLParamData** 返回的值是正在处理的 **TargetValuePtr* 缓冲区中的行的地址。  
  
4.  应用程序会调用 **SQLPutData** 一次或多次以发送列的数据。 如果在 **SQLPutData** 中指定的 *\* TargetValuePtr* 缓冲区内不能返回所有数据值，则需要多个调用; 仅当将字符 c 数据发送到具有字符、二进制或数据源特定数据类型的列时，或者在将二进制 c 数据发送到具有字符、二进制或数据源特定数据类型的列时，才 **允许对同一** 列使用多个调用。  
  
5.  应用程序再次调用 **SQLParamData** ，以指示已发送列的所有数据。  
  
    -   如果有多个执行时数据列， **SQLParamData** 将返回 SQL_NEED_DATA，并为要处理的下一次执行时数据列返回 *TargetValuePtr* 缓冲区的地址。 应用程序重复步骤4和5。  
  
    -   如果没有其他执行时数据列，则该过程已完成。 如果语句已成功执行，则 **SQLParamData** 将返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO;如果执行失败，则它将返回 SQL_ERROR。 此时， **SQLParamData** 可以返回 **SQLBulkOperations** 可以返回的任何 SQLSTATE。  
  
 如果该操作已取消，或在 **SQLParamData** 或 **SQLPutData** 中 **SQLBulkOperations** 返回 SQL_NEED_DATA 之后以及在为所有执行时数据列发送数据之前发生错误，则该应用程序只能调用与该语句关联的 **SQLCancel**、 **SQLGetDiagField**、 **SQLGetDiagRec**、 **SQLGetFunctions**、 **SQLParamData** 或 **SQLPutData** 。 如果它调用语句的任何其他函数或与该语句关联的连接，则该函数将返回 SQL_ERROR 和 SQLSTATE HY010 (函数序列错误) 。  
  
 如果应用程序调用 **SQLCancel** ，但驱动程序仍需要数据执行时数据列，则驱动程序将取消该操作。 然后，应用程序可以再次调用 **SQLBulkOperations** ;取消不会影响游标状态或当前游标位置。  
  
## <a name="row-status-array"></a>行状态数组  
 行状态数组包含行集中的每行数据在调用 **SQLBulkOperations** 之后的状态值。 在调用 **SQLFetch**、 **SQLFetchScroll**、 **SQLSetPos** 或 **SQLBulkOperations** 之后，驱动程序将设置此数组中的状态值。 如果在 **SQLBulkOperations** 之前未调用 **SQLFetch** 或 **SQLFetchScroll** ，则此数组最初由对 **SQLBulkOperations** 的调用填充。 此数组由 SQL_ATTR_ROW_STATUS_PTR 语句特性指向。 行状态数组中的元素数必须等于行集中的行数 (由 SQL_ATTR_ROW_ARRAY_SIZE 语句特性定义) 。 有关此行状态数组的信息，请参阅 [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)。  
  
## <a name="code-example"></a>代码示例  
 下面的示例每次从 Customers 表中提取10行数据。 然后，它会提示用户执行要执行的操作。 为了减少网络流量，示例缓冲区在绑定数组中本地更新、删除和插入，但偏移量超过行集数据。 当用户选择向数据源发送更新、删除和插入数据时，代码会相应地设置绑定偏移，并调用 **SQLBulkOperations**。 为简单起见，用户无法缓冲超过10个更新、删除或插入操作。  
  
```cpp  
// SQLBulkOperations_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include "stdio.h"  
  
#define UPDATE_ROW 100  
#define DELETE_ROW 101  
#define ADD_ROW 102  
#define SEND_TO_DATA_SOURCE 103  
#define UPDATE_OFFSET 10  
#define INSERT_OFFSET 20  
#define DELETE_OFFSET 30  
  
// Define structure for customer data (assume 10 byte maximum bookmark size).  
typedef struct tagCustStruct {  
   SQLCHAR Bookmark[10];  
   SQLINTEGER BookmarkLen;  
   SQLUINTEGER CustomerID;  
   SQLINTEGER CustIDInd;  
   SQLCHAR CompanyName[51];  
   SQLINTEGER NameLenOrInd;  
   SQLCHAR Address[51];  
   SQLINTEGER AddressLenOrInd;  
   SQLCHAR Phone[11];  
   SQLINTEGER PhoneLenOrInd;  
} CustStruct;  
  
// Allocate 40 of these structures. Elements 0-9 are for the current rowset,  
// elements 10-19 are for the buffered updates, elements 20-29 are for  
// the buffered inserts, and elements 30-39 are for the buffered deletes.  
CustStruct CustArray[40];  
SQLUSMALLINT RowStatusArray[10], Action, RowNum, NumUpdates = 0, NumInserts = 0,  
NumDeletes = 0;  
SQLLEN BindOffset = 0;  
SQLRETURN retcode;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLHSTMT hstmt = NULL;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Set the following statement attributes:  
   // SQL_ATTR_CURSOR_TYPE:           Keyset-driven  
   // SQL_ATTR_ROW_BIND_TYPE:         Row-wise  
   // SQL_ATTR_ROW_ARRAY_SIZE:        10  
   // SQL_ATTR_USE_BOOKMARKS:         Use variable-length bookmarks  
   // SQL_ATTR_ROW_STATUS_PTR:        Points to RowStatusArray  
   // SQL_ATTR_ROW_BIND_OFFSET_PTR:   Points to BindOffset  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER)SQL_CURSOR_KEYSET_DRIVEN, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER)sizeof(CustStruct), 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)10, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_USE_BOOKMARKS, (SQLPOINTER)SQL_UB_VARIABLE, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_STATUS_PTR, RowStatusArray, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_BIND_OFFSET_PTR, &BindOffset, 0);  
  
   // Bind arrays to the bookmark, CustomerID, CompanyName, Address, and Phone columns.  
   retcode = SQLBindCol(hstmt, 0, SQL_C_VARBOOKMARK, CustArray[0].Bookmark, sizeof(CustArray[0].Bookmark), &CustArray[0].BookmarkLen);  
   retcode = SQLBindCol(hstmt, 1, SQL_C_ULONG, &CustArray[0].CustomerID, 0, &CustArray[0].CustIDInd);  
   retcode = SQLBindCol(hstmt, 2, SQL_C_CHAR, CustArray[0].CompanyName, sizeof(CustArray[0].CompanyName), &CustArray[0].NameLenOrInd);  
   retcode = SQLBindCol(hstmt, 3, SQL_C_CHAR, CustArray[0].Address, sizeof(CustArray[0].Address), &CustArray[0].AddressLenOrInd);  
   retcode = SQLBindCol(hstmt, 4, SQL_C_CHAR, CustArray[0].Phone, sizeof(CustArray[0].Phone), &CustArray[0].PhoneLenOrInd);  
  
   // Execute a statement to retrieve rows from the Customers table.  
   retcode = SQLExecDirect(hstmt, (SQLCHAR*)"SELECT CustomerID, CompanyName, Address, Phone FROM Customers", SQL_NTS);  
  
   // Fetch and display the first 10 rows.  
   retcode = SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
   // DisplayCustData(CustArray, 10);  
  
   // Call GetAction to get an action and a row number from the user.  
   // while (GetAction(&Action, &RowNum)) {  
   Action = SQL_FETCH_NEXT;  
   RowNum = 2;  
   switch (Action) {  
      case SQL_FETCH_NEXT:  
      case SQL_FETCH_PRIOR:  
      case SQL_FETCH_FIRST:  
      case SQL_FETCH_LAST:  
      case SQL_FETCH_ABSOLUTE:  
      case SQL_FETCH_RELATIVE:  
         // Fetch and display the requested data.  
         SQLFetchScroll(hstmt, Action, RowNum);  
         // DisplayCustData(CustArray, 10);  
         break;  
  
      case UPDATE_ROW:  
         // Check if we have reached the maximum number of buffered updates.  
         if (NumUpdates < 10) {  
            // Get the new customer data and place it in the next available element of  
            // the buffered updates section of CustArray, copy the bookmark of the row  
            // being updated to the same element, and increment the update counter.  
            // Checking to see we have not already buffered an update for this  
            // row not shown.  
            // GetNewCustData(CustArray, UPDATE_OFFSET + NumUpdates);  
            memcpy(CustArray[UPDATE_OFFSET + NumUpdates].Bookmark,  
               CustArray[RowNum - 1].Bookmark,  
               CustArray[RowNum - 1].BookmarkLen);  
            CustArray[UPDATE_OFFSET + NumUpdates].BookmarkLen =  
               CustArray[RowNum - 1].BookmarkLen;  
            NumUpdates++;  
         } else {  
            printf("Buffers full. Send buffered changes to the data source.");  
         }  
         break;  
      case DELETE_ROW:  
         // Check if we have reached the maximum number of buffered deletes.  
         if (NumDeletes < 10) {  
            // Copy the bookmark of the row being deleted to the next available element  
            // of the buffered deletes section of CustArray and increment the delete  
            // counter. Checking to see we have not already buffered an update for  
            // this row not shown.  
            memcpy(CustArray[DELETE_OFFSET + NumDeletes].Bookmark,  
               CustArray[RowNum - 1].Bookmark,  
               CustArray[RowNum - 1].BookmarkLen);  
  
            CustArray[DELETE_OFFSET + NumDeletes].BookmarkLen =  
               CustArray[RowNum - 1].BookmarkLen;  
  
            NumDeletes++;  
         } else  
            printf("Buffers full. Send buffered changes to the data source.");  
         break;  
  
      case ADD_ROW:  
         // reached maximum number of buffered inserts?  
         if (NumInserts < 10) {  
            // Get the new customer data and place it in the next available element of  
            // the buffered inserts section of CustArray and increment insert counter.  
            // GetNewCustData(CustArray, INSERT_OFFSET + NumInserts);  
            NumInserts++;  
         } else  
            printf("Buffers full. Send buffered changes to the data source.");  
         break;  
  
      case SEND_TO_DATA_SOURCE:  
         // If there are any buffered updates, inserts, or deletes, set the array size  
         // to that number, set the binding offset to use the data in the buffered  
         // update, insert, or delete part of CustArray, and call SQLBulkOperations to  
         // do the updates, inserts, or deletes. Because we will never have more than  
         // 10 updates, inserts, or deletes, we can use the same row status array.  
         if (NumUpdates) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumUpdates, 0);  
            BindOffset = UPDATE_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_UPDATE_BY_BOOKMARK);  
            NumUpdates = 0;  
         }  
  
         if (NumInserts) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumInserts, 0);  
            BindOffset = INSERT_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_ADD);  
            NumInserts = 0;  
         }  
  
         if (NumDeletes) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumDeletes, 0);  
            BindOffset = DELETE_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_DELETE_BY_BOOKMARK);  
            NumDeletes = 0;  
         }  
  
         // If there were any updates, inserts, or deletes, reset the binding offset  
         // and array size to their original values.  
         if (NumUpdates || NumInserts || NumDeletes) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)10, 0);  
            BindOffset = 0;  
         }  
         break;  
   }  
   // }  
  
   // Close the cursor.  
   SQLFreeStmt(hstmt, SQL_CLOSE);  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|获取描述符的单个字段|[SQLGetDescField 函数](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|获取描述符的多个字段|[SQLGetDescRec 函数](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|设置描述符的单个字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|设置描述符的多个字段|[SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|定位游标，刷新行集中的数据，或更新或删除行集中的数据|[SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
