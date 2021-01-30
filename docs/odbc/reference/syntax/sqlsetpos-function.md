---
description: SQLSetPos 函数
title: SQLSetPos 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetPos
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetPos
helpviewer_keywords:
- SQLSetPos function [ODBC]
ms.assetid: 80190ee7-ae3b-45e5-92a9-693eb558f322
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4793fe06d25b090dfa6306eb8c4e72d513abc8d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188041"
---
# <a name="sqlsetpos-function"></a>SQLSetPos 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLSetPos** 设置行集中的光标位置，并允许应用程序刷新行集中的数据，或更新或删除结果集中的数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLSetPos(  
      SQLHSTMT        StatementHandle,  
      SQLSETPOSIROW   RowNumber,  
      SQLUSMALLINT    Operation,  
      SQLUSMALLINT    LockType);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *RowNumber*  
 送要对其 *执行操作的* 行集中的行的位置。 如果 *RowNumber* 为0，则操作将应用于行集中的每一行。  
  
 有关其他信息，请参阅 "注释"。  
  
 *操作*  
 送要执行的操作：  
  
 SQL_POSITION SQL_REFRESH SQL_UPDATE SQL_DELETE  
  
> [!NOTE]
>  对于 ODBC 1.x，*操作* 参数的 SQL_ADD 值已 *弃用。* 为了向后兼容 *，ODBC 2.x* 驱动程序将需要支持 SQL_ADD。 此功能已由 SQL_ADD 的 *操作* 替换为对 **SQLBulkOperations** 的调用。 当 ODBC 1.x *应用程序**与 odbc 2.x* 驱动程序一起使用时，驱动程序管理器会将对 **SQLBulkOperations** 的调用映射 **到使用 SQL_ADD** *操作* 的 SQL_ADD *操作*。  
  
 有关详细信息，请参阅 "注释"。  
  
 *LockType*  
 送指定在执行 *操作* 参数中指定的操作后如何锁定行。  
  
 SQL_LOCK_NO_CHANGE SQL_LOCK_EXCLUSIVE SQL_LOCK_UNLOCK  
  
 有关详细信息，请参阅 "注释"。  
  
## <a name="returns"></a>返回  
  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NEED_DATA、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetPos** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSetPos** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
 对于可以返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR (除了 01xxx SQLSTATEs) 的所有这些 SQLSTATEs，如果一个或多个（但不是全部）多行操作行出现错误，则返回 SQL_SUCCESS_WITH_INFO，如果单行操作出现错误，则返回 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01001|游标操作冲突|*操作* 参数已 SQL_DELETE 或 SQL_UPDATE，并且没有删除或更新任何行或多行。  (有关多行更新的详细信息，请参阅 **SQLSetStmtAttr** 中 SQL_ATTR_SIMULATE_CURSOR *属性* 的说明。 )  (函数返回 SQL_SUCCESS_WITH_INFO。 ) <br /><br /> *操作* 参数为 SQL_DELETE 或 SQL_UPDATE，操作由于乐观并发而失败。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据右截断|*操作* 参数为 SQL_REFRESH，为数据类型为 SQL_C_CHAR 或 SQL_C_BINARY 的一列或多列返回的字符串或二进制数据导致截断非空白字符或非 NULL 二进制数据。|  
|01S01|行中的错误|*RowNumber* 参数为0，并在执行使用 *操作* 参数指定的操作时出现一个或多个行中的错误。<br /><br /> 如果一个或多个（但不是全部）多行操作行发生错误，则会返回 (SQL_SUCCESS_WITH_INFO，如果单行操作出现错误，则返回 SQL_ERROR。 ) <br /><br />  (仅当在 **SQLExtendedFetch** 后调用 **SQLSetPos** 时，才返回此 SQLSTATE，如果该 *驱动程序是 ODBC 2.x* 驱动程序，并且未使用游标库，则返回。 ) |  
|01S07|小数截断|*操作* 参数已 SQL_REFRESH，应用程序缓冲区的数据类型未 SQL_C_CHAR 或 SQL_C_BINARY，并且为一个或多个列返回到应用程序缓冲区的数据被截断。 对于数值数据类型，数值的小数部分被截断。 对于包含时间部分的时间、时间戳和间隔数据类型，时间的小数部分将被截断。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|在对 **SQLBindCol** 的调用中，结果集中的列的数据值无法转换为 *TargetType* 指定的数据类型。|  
|07009|描述符索引无效|参数 *操作* 是 SQL_REFRESH 或 SQL_UPDATE，并且列的列号大于结果集中的列数。|  
|21S02|派生表的等级与列列表不匹配|参数 *操作* 是 SQL_UPDATE 的，并且没有可更新的列，因为所有列均未绑定、为只读，或者绑定长度/指示器缓冲区中的值 SQL_COLUMN_IGNORE。|  
|22001|字符串数据，右截断|*操作* 参数已 SQL_UPDATE，并将字符或二进制值分配给某一列时导致截断非空白 (的字符) 或非 null (二进制) 字符或字节。|  
|22003|数值超出范围|参数 *操作* 是 SQL_UPDATE 的，并且为结果集中的列分配数值会导致整个 (，而不是) 小数部分的小数部分将被截断。<br /><br /> 参数 *操作* 是 SQL_REFRESH 的，返回一个或多个绑定列的数值将导致有效位丢失。|  
|22007|Datetime 格式无效|参数 *操作* 是 SQL_UPDATE 的，并且为结果集中的某列分配日期或时间戳值导致年、月或日字段超出范围。<br /><br /> 参数 *操作* 是 SQL_REFRESH 的，并且为一个或多个绑定列返回日期或时间戳值将导致年、月或日字段超出范围。|  
|22008|日期/时间字段溢出|*操作* 参数是 SQL_UPDATE 的，并且针对发送到结果集中的列的数据进行日期时间运算的性能导致了日期时间字段 (结果的年、月、日、小时、分钟或秒字段，) 结果超出了字段的允许值范围，或者根据公历的 datetime 自然规则无效。<br /><br /> *操作* 参数已 SQL_REFRESH，而从结果集检索到的数据的 datetime 算法的性能导致了日期时间字段 (结果的年、月、日、小时、分钟或秒字段) ，结果超出了字段的允许值范围，或者根据公历的 datetime 自然规则无效。|  
|22015|间隔字段溢出|*操作* 参数已 SQL_UPDATE，并将精确数值或间隔 C 类型分配给间隔 SQL 数据类型导致了有效位丢失。<br /><br /> *操作* 参数 SQL_UPDATE;当分配给某个间隔 SQL 类型时，interval SQL 类型中不存在 C 类型的值的表示形式。<br /><br /> *操作* 参数是 SQL_REFRESH 的，并且从精确数值或间隔 SQL 类型分配到 interval C 类型会导致前导字段的有效位丢失。<br /><br /> *操作* 参数 SQL_ 刷新;当分配到某个时间间隔 C 类型时，在 "C #" 类型的 "SQL 类型" 值中没有表示形式。|  
|22018|转换规范的字符值无效|*操作* 参数 SQL_REFRESH;C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列中的值不是绑定 C 类型的有效文本。<br /><br /> SQL_UPDATE 了参数 *操作* ;SQL 类型是精确或近似的数字、日期时间或时间间隔数据类型;C 类型为 SQL_C_CHAR;列中的值不是绑定的 SQL 类型的有效文本。|  
|23000|完整性约束冲突|参数 *操作* 是 SQL_DELETE 或 SQL_UPDATE，并违反了完整性约束。|  
|24000|无效的游标状态|*StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。<br /><br />  (DM) 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。<br /><br /> 在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** ，但游标位于结果集的开头之前或结果集的末尾。<br /><br /> 参数 *操作* 是 SQL_DELETE、SQL_REFRESH 或 SQL_UPDATE，并且游标定位在结果集的开头之前或结果集的结尾之后。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|42000|语法错误或访问冲突|驱动程序无法根据需要锁定行以执行在参数 *操作* 中请求的操作。<br /><br /> 驱动程序无法按参数 *LockType* 中的要求锁定该行。|  
|44000|WITH CHECK OPTION 冲突|*操作* 参数已 SQL_UPDATE，并对通过指定 **WITH CHECK OPTION** 创建的已查看表或从已查看表派生的表执行了更新，这样，受更新影响的一个或多个行将不再出现在查看的表中。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用了该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 SQLSetPos 函数时，此异步函数仍在执行。<br /><br />  (DM) 指定的 *StatementHandle* 未处于执行状态。 调用函数时，无需先调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) *该驱动程序是一个 ODBC 2.x* 驱动程序，并且调用 **SQLFetch** 后对 *StatementHandle* 调用了 **SQLSetPos** 。|  
|HY011|现在无法设置属性| (DM) 该 *驱动程序是一个 ODBC 2.x* 驱动程序;已设置 SQL_ATTR_ROW_STATUS_PTR 语句特性;然后调用 **SQLSetPos** ，然后调用 **SQLFetch**、 **SQLFetchScroll** 或 **SQLExtendedFetch** 。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效|*操作* 参数已 SQL_UPDATE，数据值为 null 指针，列长度值不是0、SQL_DATA_AT_EXEC、SQL_COLUMN_IGNORE、SQL_NULL_DATA 或小于或等于 SQL_LEN_DATA_AT_EXEC_OFFSET。<br /><br /> *操作* 参数 SQL_UPDATE;数据值不是 null 指针;C 数据类型为 SQL_C_BINARY 或 SQL_C_CHAR;列长度值小于0，但不等于 SQL_DATA_AT_EXEC、SQL_COLUMN_IGNORE、SQL_NTS 或 SQL_NULL_DATA，或者小于或等于 SQL_LEN_DATA_AT_EXEC_OFFSET。<br /><br /> SQL_DATA_AT_EXEC 的长度/指示器缓冲区中的值;SQL 类型是 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或 long 特定于数据源的数据类型; **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型为 "Y"。|  
|HY092|无效的属性标识符| (DM) 为 *操作* 参数指定的值无效。<br /><br />  (DM) 为 *LockType* 参数指定的值无效。<br /><br /> *操作* 参数已 SQL_UPDATE 或 SQL_DELETE，SQL_ATTR_CONCURRENCY 语句属性已 SQL_ATTR_CONCUR_READ_ONLY。|  
|HY107|行值超出范围|为参数 *RowNumber* 指定的值大于行集中的行数。|  
|HY109|游标位置无效|与 *StatementHandle* 关联的游标被定义为只进，因此无法将游标定位到行集中。 请参阅 **SQLSetStmtAttr** 中的 SQL_ATTR_CURSOR_TYPE 属性的说明。<br /><br /> *操作* 参数 SQL_UPDATE、SQL_DELETE 或 SQL_REFRESH，并且由 *RowNumber* 参数标识的行已被删除或尚未提取。<br /><br /> *RowNumber* 参数 (DM) 为0，*操作* 参数已 SQL_POSITION。<br /><br /> 调用 **SQLBulkOperations** 后，调用 **SQLFetchScroll** 或 **SQLFetch** 之前调用了 **SQLSetPos** 。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 *操作* 参数或 *LockType* 参数中请求的操作。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** *属性* 设置为 SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
  
> [!CAUTION]
>  有关语句的详细信息，请参阅 **SQLSetPos** 可以在中调用，以及在 *与 ODBC 2.x* 应用程序兼容时需要执行的操作，请参阅 [块游标、可滚动游标和向后兼容性](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md)。  
  
## <a name="rownumber-argument"></a>RowNumber 参数  
 *RowNumber* 参数指定行集中要 *对其执行操作参数指定* 的操作的行号。 如果 *RowNumber* 为0，则操作将应用于行集中的每一行。 *RowNumber* 的值必须介于0和行集中的行数之间。  
  
> [!NOTE]  
>  在 C 语言中，数组基于0，并且 *RowNumber* 参数是从1开始的。 例如，若要更新行集的第五行，应用程序会修改数组索引4处的行集缓冲区，但指定 *RowNumber* 为5。  
  
 所有操作都将光标置于由 *RowNumber* 指定的行上。 以下操作需要游标位置：  
  
-   定位的 update 和 delete 语句。  
  
-   对 **SQLGetData** 的调用。  
  
-   用 SQL_DELETE、SQL_REFRESH 和 SQL_UPDATE 选项调用 **SQLSetPos** 。  
  
 例如，如果使用 SQL_DELETE 的 *操作* 对 **SQLSetPos** 的调用使用 *RowNumber* ，则游标将定位到行集的第二行并被删除。 实现行状态数组中的项 (由 SQL_ATTR_ROW_STATUS_PTR 语句特性指向第二行的) 更改为 SQL_ROW_DELETED。  
  
 当应用程序调用 **SQLSetPos** 时，可以指定游标位置。 通常情况下，它通过 SQL_POSITION 或 SQL_REFRESH 操作调用 **SQLSetPos** ，以便在执行定位的 update 或 delete 语句或调用 **SQLGetData** 之前定位光标。  
  
## <a name="operation-argument"></a>操作参数  
 *操作* 参数支持以下操作。 若要确定数据源支持的选项，应用程序将使用 SQL_DYNAMIC_CURSOR_ATTRIBUTES1、SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1、SQL_KEYSET_CURSOR_ATTRIBUTES1 或 SQL_STATIC_CURSOR_ATTRIBUTES1 信息类型调用 **SQLGetInfo** ，具体取决于游标的类型 (。  
  
|*操作*<br /><br /> 参数|Operation|  
|------------------------------|---------------|  
|SQL_POSITION|驱动程序将游标定位于 *RowNumber* 指定的行。<br /><br /> SQL_POSITION *操作* 将忽略 SQL_ATTR_ROW_OPERATION_PTR 语句特性指向的行状态数组的内容。|  
|SQL_REFRESH|驱动程序将游标定位在 *RowNumber* 指定的行上，并刷新该行的行集缓冲区中的数据。 有关驱动程序如何返回行集缓冲区中的数据的详细信息，请参阅 **SQLBindCol** 中的行和列绑定的描述。<br /><br /> SQL_REFRESH *操作* 的 **SQLSetPos** 更新当前提取的行集中的行的状态和内容。 这包括刷新书签。 由于缓冲区中的数据已刷新但不制约，因此行集中的成员身份是固定的。 这不同于通过调用 **SQLFetchScroll** 进行的刷新，其 *FetchOrientation* 为 SQL_FETCH_RELATIVE，而 *RowNumber* 等于0，它在结果集中 refetches 行集，使其能够显示已添加的数据并删除已删除的数据（如果驱动程序和游标支持这些操作）。<br /><br /> 使用 **SQLSetPos** 成功刷新将不会更改行状态 SQL_ROW_DELETED。 在下一次提取之前，行集中已删除的行将继续标记为已删除。 如果游标支持打包 (（后续的 **SQLFetch** 或 **SQLFetchScroll** 不会) 返回已删除的行），则会在下一次提取时消失行。<br /><br /> 执行刷新 with **SQLSetPos** 时不会显示已添加的行。 此行为不同于 **SQLFetchScroll** 的 SQL_FETCH_RELATIVE *FetchType* ，且 *RowNumber* 等于0，这也会刷新当前行集，但会在游标支持这些操作时显示已添加记录或包删除记录。<br /><br /> 使用 **SQLSetPos** 的成功刷新将 SQL_ROW_ADDED 的行状态更改为 SQL_ROW_SUCCESS (如果行状态数组存在) 。<br /><br /> 如果行状态数组存在) ，则使用 **SQLSetPos** 成功刷新会将行状态 SQL_ROW_UPDATED 更改为行的新状态 (。<br /><br /> 如果对某行执行 **SQLSetPos** 操作时出现错误，则将行状态设置为 SQL_ROW_ERROR (如果行状态数组存在) 。<br /><br /> 对于使用 SQL_CONCUR_ROWVER 或 SQL_CONCUR_VALUES 的 SQL_ATTR_CONCURRENCY 语句特性打开的游标， **SQLSetPos** 的刷新可能会更新数据源用来检测行是否已更改的开放式并发值。 如果出现这种情况，则在从服务器刷新行集缓冲区时，将更新用于确保游标并发的行版本。 这会对每个刷新的行发生。<br /><br /> SQL_REFRESH *操作* 将忽略 SQL_ATTR_ROW_OPERATION_PTR 语句特性指向的行状态数组的内容。|  
|SQL_UPDATE|驱动程序将游标定位于 *RowNumber* 指定的行，并使用行集缓冲区中的值更新基础数据行 (**SQLBindCol**) 中的 *TargetValuePtr* 参数。 它从 **SQLBindCol**) 中 *StrLen_or_IndPtr* 参数 (的长度/指示器缓冲区检索数据的长度。 如果 SQL_COLUMN_IGNORE 任意列的长度，则不会更新列。 更新行后，驱动程序将行状态数组的相应元素更改为 SQL_ROW_UPDATED 或 SQL_ROW_SUCCESS_WITH_INFO (如果行状态数组存在) 。<br /><br /> 如果对包含重复列的游标调用了 SQL_UPDATE 的 *操作* 参数的 **SQLSetPos** ，则为驱动程序定义的行为。 驱动程序可以返回驱动程序定义的 SQLSTATE，可以更新结果集中出现的第一列，或者执行其他驱动程序定义的行为。<br /><br /> SQL_ATTR_ROW_OPERATION_PTR 语句特性指向的行操作数组可用于指示在大容量更新期间应忽略当前行集中的行。 有关详细信息，请参阅此函数引用后面的 "状态和操作数组"。|  
|SQL_DELETE|驱动程序将光标置于由 *RowNumber* 指定的行上，并删除基础数据行。 它将行状态数组的相应元素更改为 SQL_ROW_DELETED。 删除行后，以下行对行无效：已定位的 update 和 delete 语句、对 **SQLGetData** 的调用，以及对除 SQL_POSITION 以外的任何内容的 **SQLSetPos** *的调用* 。 对于支持打包的驱动程序，当从数据源中检索新数据时，将从游标中删除该行。<br /><br /> 行是否保持可见取决于游标类型。 例如，已删除的行对静态游标和由键集驱动的游标可见，但不会对动态游标可见。<br /><br /> SQL_ATTR_ROW_OPERATION_PTR 语句特性指向的行操作数组可用于指示在大容量删除期间应忽略当前行集中的行。 有关详细信息，请参阅此函数引用后面的 "状态和操作数组"。|  
  
## <a name="locktype-argument"></a>LockType 参数  
 *LockType* 参数为应用程序提供了一种控制并发的方式。 在大多数情况下，支持并发级别和事务的数据源将仅支持 *LockType* 参数的 SQL_LOCK_NO_CHANGE 值。 *LockType* 参数通常仅用于基于文件的支持。  
  
 *LockType* 参数指定 **SQLSetPos** 执行后行的锁定状态。 如果驱动程序无法将行锁定为执行请求的操作或满足 *LockType* 参数，则它将返回 SQL_ERROR 和 SQLSTATE 42000 (语法错误或访问冲突) 。  
  
 尽管为单个语句指定了 *LockType* 参数，但该锁 accords 对该连接上的所有语句都具有相同的特权。 具体而言，连接上的一个语句所获得的锁可以通过同一连接上的不同语句来解除锁定。  
  
 通过 **SQLSetPos** 锁定的行保持锁定状态，直到应用程序对 *LockType* 设置为 SQL_LOCK_UNLOCK 的行调用 **SQLSetPos** ，或直到应用程序使用 SQL_CLOSE 选项调用语句或 **SQLFreeStmt** 的 **SQLFreeHandle** 。 对于支持事务的驱动程序，当提交或回滚事务时 SQL_CURSOR_COMMIT_BEHAVIOR，如果游标是在提交或回滚事务时关闭的，则通过 **SQLSetPos** 锁定的行会解除锁定， (如果在提交或回滚事务时关闭游标，由 **SQLGetInfo**) 返回 SQL_CURSOR_ROLLBACK_BEHAVIOR 信息类型。  
  
 *LockType* 参数支持以下类型的锁。 若要确定数据源支持哪些锁，应用程序将使用 SQL_DYNAMIC_CURSOR_ATTRIBUTES1、SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1、SQL_KEYSET_CURSOR_ATTRIBUTES1 或 SQL_STATIC_CURSOR_ATTRIBUTES1 信息类型调用 **SQLGetInfo** ，具体取决于游标的类型 (。  
  
|*LockType* 参数|锁类型|  
|-------------------------|---------------|  
|SQL_LOCK_NO_CHANGE|驱动程序或数据源确保行处于调用 **SQLSetPos** 之前的锁定或解锁状态。 此值 *LockType* 允许不支持显式行级锁定的数据源使用当前并发和事务隔离级别所需的任何锁定。|  
|SQL_LOCK_EXCLUSIVE|驱动程序或数据源只锁定行。 不同连接或不同应用程序中的语句不能用于获取行的任何锁。|  
|SQL_LOCK_UNLOCK|驱动程序或数据源将锁定该行。|  
  
 如果驱动程序支持 SQL_LOCK_EXCLUSIVE 但不支持 SQL_LOCK_UNLOCK，则锁定的行将保持锁定状态，直到上一段中所述的某个函数调用发生。  
  
 如果驱动程序支持 SQL_LOCK_EXCLUSIVE 但不支持 SQL_LOCK_UNLOCK，则锁定的行将保持锁定状态，直到应用程序使用 SQL_CLOSE 选项调用语句或 **SQLFreeStmt** 的 **SQLFreeHandle** 。 如果驱动程序支持事务并在提交或回滚事务时关闭游标，则该应用程序将调用 **SQLEndTran**。  
  
 对于 **SQLSetPos** 中的更新和删除操作，应用程序将使用 *LockType* 参数，如下所示：  
  
-   为了保证行在检索后不会更改，应用程序将调用 **SQLSetPos** ，并将 *操作* 设置为 SQL_REFRESH，并将 *LockType* 设置为 SQL_LOCK_EXCLUSIVE。  
  
-   如果应用程序将 *LockType* 设置为 SQL_LOCK_NO_CHANGE，则驱动程序将保证只有在 SQL_ATTR_CONCURRENCY 语句特性指定了 SQL_CONCUR_LOCK 的应用程序时，更新或删除操作才会成功。  
  
-   如果应用程序为 SQL_ATTR_CONCURRENCY 语句特性指定 SQL_CONCUR_ROWVER 或 SQL_CONCUR_VALUES，则驱动程序会比较行版本或值，如果自应用程序提取行后行发生了更改，则会拒绝该操作。  
  
-   如果应用程序为 SQL_ATTR_CONCURRENCY 语句特性指定 SQL_CONCUR_READ_ONLY，则驱动程序将拒绝任何更新或删除操作。  
  
 有关 SQL_ATTR_CONCURRENCY 语句特性的详细信息，请参阅 [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)。  
  
## <a name="status-and-operation-arrays"></a>状态和操作阵列  
 调用 **SQLSetPos** 时，将使用以下状态和操作数组：  
  
-   行状态数组 (由 IRD 中的 SQL_DESC_ARRAY_STATUS_PTR 字段指向，SQL_ATTR_ROW_STATUS_ARRAY 语句特性) 包含行集中每个数据行的状态值。 在调用 **SQLFetch**、 **SQLFetchScroll**、 **SQLBulkOperations** 或 **SQLSetPos** 之后，驱动程序将设置此数组中的状态值。 此数组由 SQL_ATTR_ROW_STATUS_PTR 语句特性指向。  
  
-   行操作数组 (由 ARD 中的 SQL_DESC_ARRAY_STATUS_PTR 字段指向，SQL_ATTR_ROW_OPERATION_ARRAY 语句特性) 包含行集中每一行的值，该值指示是否忽略或执行针对大容量操作的 **SQLSetPos** 调用。 数组中的每个元素均设置为 SQL_ROW_PROCEED (默认) 或 SQL_ROW_IGNORE。 此数组由 SQL_ATTR_ROW_OPERATION_PTR 语句特性指向。  
  
 状态和操作数组中的元素数必须等于行集中的行数 (如 SQL_ATTR_ROW_ARRAY_SIZE 语句特性) 所定义。  
  
 有关行状态数组的信息，请参阅 [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)。 有关行操作数组的信息，请参阅本部分后面的 "在批量操作中忽略行"。  
  
## <a name="using-sqlsetpos"></a>使用 SQLSetPos  
 在应用程序调用 **SQLSetPos** 之前，必须执行以下步骤序列：  
  
1.  如果应用程序将调用 **SQLSetPos** ，并将 *操作* 设置为 SQL_UPDATE，请对每一列调用 **SQLBindCol** (或 **SQLSetDescRec**) ，以指定其数据类型，并绑定列的数据和长度的缓冲区。  
  
2.  如果应用程序将调用 **SQLSetPos** ，并将 *操作* 设置为 SQL_DELETE 或 SQL_UPDATE，请调用 **SQLColAttribute** ，以确保要删除或更新的列是可更新的。  
  
3.  调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数以创建结果集。  
  
4.  调用 **SQLFetch** 或 **SQLFetchScroll** 以检索数据。  
  
 有关使用 **SQLSetPos** 的详细信息，请参阅 [使用 SQLSetPos 更新数据](../../../odbc/reference/develop-app/updating-data-with-sqlsetpos.md)。  
  
## <a name="deleting-data-using-sqlsetpos"></a>使用 SQLSetPos 删除数据  
 若要删除 **SQLSetPos** 中的数据，应用程序将调用 **SQLSetPos** ，并将 *RowNumber* 设置为要删除的行的行号，并将 *操作* 设置为 SQL_DELETE。  
  
 删除数据后，驱动程序会将相应行的实现行状态数组中的值更改为 SQL_ROW_DELETED (或 SQL_ROW_ERROR) 。  
  
## <a name="updating-data-using-sqlsetpos"></a>使用 SQLSetPos 更新数据  
 应用程序可以在绑定的数据缓冲区中或者通过一个或多个对 **SQLPutData** 的调用传递列的值。 其数据与 **SQLPutData** 一起传递的列称为 " *执行时数据*" *列*。 这些数据通常用于为 SQL_LONGVARBINARY 和 SQL_LONGVARCHAR 列发送数据，并且可以与其他列混合使用。  
  
#### <a name="to-update-data-with-sqlsetpos-an-application"></a>若要使用 SQLSetPos 更新数据，请执行以下操作：  
  
1.  将数据和长度/指示器缓冲区中的值置于 **SQLBindCol**：  
  
    -   对于普通列，应用程序会将新列值置于 *\* TargetValuePtr* 缓冲区中，并将该值的长度置于 *\* StrLen_or_IndPtr* 缓冲区中。 如果不应更新该行，应用程序会将 SQL_ROW_IGNORE 放置在行操作数组的该行的元素中。  
  
    -   对于执行时数据列，应用程序会将应用程序定义的值（如列号）放入 *\* TargetValuePtr* 缓冲区。 稍后可使用该值来识别列。  
  
         应用程序将 SQL_LEN_DATA_AT_EXEC (*长度*) 宏的结果置于 **StrLen_or_IndPtr* 缓冲区中。 如果列的 SQL 数据类型为 SQL_LONGVARBINARY、SQL_LONGVARCHAR 或 long 特定于数据源的数据类型，并且驱动程序为 **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型返回 "Y"，则 *length* 是要为参数发送的数据的字节数;否则，该值必须为非负值，并且将被忽略。  
  
2.  调用 **SQLSetPos** 并将 *操作* 参数设置为 SQL_UPDATE 以更新数据行。  
  
    -   如果没有执行时数据列，则该过程已完成。  
  
    -   如果有任何执行时数据列，该函数将返回 SQL_NEED_DATA，并继续执行步骤3。  
  
3.  对于要处理的第一个执行时数据列，调用 **SQLParamData** 来检索 *\* TargetValuePtr* 缓冲区的地址。 **SQLParamData** 返回 SQL_NEED_DATA。 应用程序从 *\* TargetValuePtr* 缓冲区检索应用程序定义的值。  
  
    > [!NOTE]  
    >  尽管执行时数据参数与执行时数据列相似，但每个 **SQLParamData** 返回的值都是不同的。  
  
    > [!NOTE]  
    >  执行时数据参数是 SQL 语句中的参数，当使用 **SQLExecDirect** 或 **SQLExecute** 执行语句时，该语句中的数据将与 **SQLPutData** 一起发送。 它们通过 **SQLBindParameter** 或通过使用 **SQLSetDescRec** 设置描述符进行绑定。 **SQLParamData** 返回的值是在 *ParameterValuePtr* 参数中传递给 **SQLBindParameter** 的32位值。  
  
    > [!NOTE]  
    >  执行时数据列是行集中的列，当使用 **SQLSetPos** 更新行时，数据将随 **SQLPutData** 一起发送。 它们与 **SQLBindCol** 绑定在一起。 **SQLParamData** 返回的值是正在处理的 **TargetValuePtr* 缓冲区中的行的地址。  
  
4.  调用 **SQLPutData** 一次或多次以发送列的数据。 如果所有数据值都无法在 **SQLPutData** 中指定的 *\* TargetValuePtr* 缓冲区中返回，则需要多个调用; 仅当将字符 c 数据发送到具有字符、二进制或数据源特定数据类型的列时，或在将二进制 c 数据发送到具有字符、二进制或数据源特定数据类型的列时，才 **允许对同一** 列的多个调用。  
  
5.  再次调用 **SQLParamData** ，以指示已发送列的所有数据。  
  
    -   如果有多个执行时数据列， **SQLParamData** 将返回 SQL_NEED_DATA，并为要处理的下一次执行时数据列返回 *TargetValuePtr* 缓冲区的地址。 应用程序重复步骤4和5。  
  
    -   如果没有其他执行时数据列，则该过程已完成。 如果语句已成功执行，则 **SQLParamData** 将返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO;如果执行失败，则它将返回 SQL_ERROR。 此时， **SQLParamData** 可以返回 **SQLSetPos** 可以返回的任何 SQLSTATE。  
  
 如果数据已更新，驱动程序会将相应行的实现行状态数组中的值更改为 SQL_ROW_UPDATED。  
  
 如果该操作已取消或在 **SQLParamData** 或 **SQLPutData** 中发生错误，则在 **SQLSetPos** 返回 SQL_NEED_DATA 之后，在为所有执行时数据列发送数据之前，应用程序只能为语句或与该语句关联的连接调用 **SQLCancel**、 **SQLGetDiagField**、 **SQLGetDiagRec**、 **SQLGetFunctions**、 **SQLParamData** 或 **SQLPutData** 。 如果它调用语句的任何其他函数或与该语句关联的连接，则该函数将返回 SQL_ERROR 和 SQLSTATE HY010 (函数序列错误) 。  
  
 如果应用程序调用 **SQLCancel** ，但驱动程序仍需要数据执行时数据列，则驱动程序将取消该操作。 然后，应用程序可以再次调用 **SQLSetPos** ;取消不会影响游标状态或当前游标位置。  
  
 当与游标关联的查询规范的选择列表包含对同一列的多个引用时，无论是否生成错误，或者驱动程序是否忽略重复的引用并执行所请求的操作，都是由驱动程序定义的。  
  
## <a name="performing-bulk-operations"></a>执行批量操作  
 如果 *RowNumber* 参数为0，则驱动程序将执行在 *操作* 参数中为行集中的每一行指定的操作，该操作在 SQL_ATTR_ROW_OPERATION_PTR 语句特性指向的行操作数组的字段中具有 SQL_ROW_PROCEED 值。 这是 SQL_DELETE、SQL_REFRESH 或 SQL_UPDATE 的 *操作* 参数的 *RowNumber* 参数的有效值，但不 SQL_POSITION。 **SQLSetPos** *操作* 为 SQL_POSITION 并且 *RowNumber* 等于0时，将返回 SQLSTATE HY109 (无效的游标位置) 。  
  
 如果发生了与整个行集相关的错误，例如 SQLSTATE HYT00 (超时过期时间) ，则驱动程序将返回 SQL_ERROR 和相应的 SQLSTATE。 行集缓冲区的内容是未定义的，并且游标位置保持不变。  
  
 如果发生了与单个行相关的错误，则驱动程序：  
  
-   将 SQL_ATTR_ROW_STATUS_PTR 语句特性指向的行状态数组中的行的元素设置为 SQL_ROW_ERROR。  
  
-   在错误队列中为错误发送一个或多个其他 SQLSTATEs，并在诊断数据结构中设置 SQL_DIAG_ROW_NUMBER 字段。  
  
 在处理完错误或警告后，如果驱动程序为行集中的其余行完成操作，它将返回 SQL_SUCCESS_WITH_INFO。 因此，对于返回了错误的每一行，错误队列包含零个或多个其他 SQLSTATEs。 如果驱动程序在处理错误或警告后停止了该操作，它将返回 SQL_ERROR。  
  
 如果驱动程序返回任何警告，如 SQLSTATE 01004 (数据截断) ，它将在返回适用于特定行的错误信息之前，返回应用于整个行集或行集中未知行的警告。 它将返回特定行的警告以及关于这些行的任何其他错误信息。  
  
 如果 *RowNumber* 等于0，并且 *Operation* SQL_UPDATE、SQL_REFRESH 或 SQL_DELETE，则 SQL_ATTR_ROWS_FETCHED_PTR 语句特性指向 **SQLSetPos** 操作的行数。  
  
 如果 *RowNumber* 等于0并且 *操作* SQL_DELETE、SQL_REFRESH 或 SQL_UPDATE，则操作后的当前行与操作之前的当前行相同。  
  
## <a name="ignoring-a-row-in-a-bulk-operation"></a>在大容量操作中忽略行  
 使用 **SQLSetPos** 时，可以使用行操作数组指示当前行集中的行应在大容量操作中被忽略。 若要在执行大容量操作时指示驱动程序忽略一个或多个行，应用程序应执行以下步骤：  
  
1.  调用 **SQLSetStmtAttr** ，将 SQL_ATTR_ROW_OPERATION_PTR 语句特性设置为指向 SQLUSMALLINTs 的数组。 还可以通过调用 **SQLSetDescField** 来设置此字段，以设置 ARD 的 SQL_DESC_ARRAY_STATUS_PTR 标头字段，这需要应用程序获取描述符句柄。  
  
2.  将 row 运算数组的每个元素设置为以下两个值之一：  
  
    -   SQL_ROW_IGNORE，表示为大容量操作排除该行。  
  
    -   SQL_ROW_PROCEED，指示在大容量操作中包含行。 （这是默认值。）  
  
3.  调用 **SQLSetPos** 以执行批量操作。  
  
 以下规则适用于行操作数组：  
  
-   SQL_ROW_IGNORE 和 SQL_ROW_PROCEED 只影响使用 **SQLSetPos** 的操作，该 *操作* 使用 SQL_DELETE 或 SQL_UPDATE 操作。 它们不会影响对 SQL_REFRESH 或 SQL_POSITION *操作* 的 **SQLSetPos** 的调用。  
  
-   默认情况下，指针设置为 null。  
  
-   如果指针为 null，则将更新所有行，就好像所有元素都设置为 SQL_ROW_PROCEED。  
  
-   将元素设置为 SQL_ROW_PROCEED 不能保证该特定行上会发生操作。 例如，如果行集中某一行的状态为 "SQL_ROW_ERROR"，则无论是否 SQL_ROW_PROCEED 指定了应用程序，驱动程序都可能无法更新该行。 应用程序必须始终检查行状态数组，才能确定操作是否成功。  
  
-   SQL_ROW_PROCEED 在头文件中定义为0。 应用程序可将行操作数组初始化为0，以便处理所有行。  
  
-   如果行操作数组中的元素编号 "n" 设置为 SQL_ROW_IGNORE 并且调用 **SQLSetPos** 来执行大容量更新或删除操作，则行集中的第 n 行在调用 **SQLSetPos** 后将保持不变。  
  
-   应用程序应自动将只读列设置为 SQL_ROW_IGNORE。  
  
## <a name="ignoring-a-column-in-a-bulk-operation"></a>在大容量操作中忽略列  
 若要避免尝试更新到一个或多个只读列时产生不必要的处理诊断，应用程序可以将绑定的长度/指示器缓冲区中的值设置为 SQL_COLUMN_IGNORE。 有关详细信息，请参阅 [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)。  
  
## <a name="code-example"></a>代码示例  
 在下面的示例中，应用程序允许用户浏览 ORDERS 表并更新订单状态。 游标由行集大小为20的键集驱动，并使用与行版本比较的开放式并发控制。 提取每个行集后，应用程序会将其打印出来，并允许用户选择和更新订单的状态。 应用程序使用 **SQLSetPos** 将游标定位在选定行上，并执行行的定位更新。 为清楚起见，省略 (错误处理。 )   
  
```cpp  
#define ROWS 20  
#define STATUS_LEN 6  
  
SQLCHAR        szStatus[ROWS][STATUS_LEN], szReply[3];  
SQLINTEGER     cbStatus[ROWS], cbOrderID;  
SQLUSMALLINT   rgfRowStatus[ROWS];  
SQLUINTEGER    sOrderID, crow = ROWS, irow;  
SQLHSTMT       hstmtS, hstmtU;  
  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CONCURRENCY, (SQLPOINTER) SQL_CONCUR_ROWVER, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER) SQL_CURSOR_KEYSET_DRIVEN, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_STATUS_PTR, (SQLPOINTER) rgfRowStatus, 0);  
SQLSetCursorName(hstmtS, "C1", SQL_NTS);  
SQLExecDirect(hstmtS, "SELECT ORDERID, STATUS FROM ORDERS ", SQL_NTS);  
  
SQLBindCol(hstmtS, 1, SQL_C_ULONG, &sOrderID, 0, &cbOrderID);  
SQLBindCol(hstmtS, 2, SQL_C_CHAR, szStatus, STATUS_LEN, &cbStatus);  
  
while ((retcode == SQLFetchScroll(hstmtS, SQL_FETCH_NEXT, 0)) != SQL_ERROR) {  
   if (retcode == SQL_NO_DATA_FOUND)  
      break;  
   for (irow = 0; irow < crow; irow++) {  
      if (rgfRowStatus[irow] != SQL_ROW_DELETED)  
         printf("%2d %5d %*s\n", irow+1, sOrderID, NAME_LEN-1, szStatus[irow]);  
   }  
   while (TRUE) {  
      printf("\nRow number to update?");  
      gets_s(szReply, 3);  
      irow = atoi(szReply);  
      if (irow > 0 && irow <= crow) {  
         printf("\nNew status?");  
         gets_s(szStatus[irow-1], (ROWS * STATUS_LEN));  
         SQLSetPos(hstmtS, irow, SQL_POSITION, SQL_LOCK_NO_CHANGE);  
         SQLPrepare(hstmtU,  
          "UPDATE ORDERS SET STATUS=? WHERE CURRENT OF C1", SQL_NTS);  
         SQLBindParameter(hstmtU, 1, SQL_PARAM_INPUT,  
            SQL_C_CHAR, SQL_CHAR,  
            STATUS_LEN, 0, szStatus[irow], 0, NULL);  
         SQLExecute(hstmtU);  
      } else if (irow == 0) {  
         break;  
      }  
   }  
}  
```  
  
 有关更多示例，请参阅 [定位更新和删除语句](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md) ，并 [通过 SQLSetPos 更新行集中的行](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|执行不与块光标位置相关的大容量操作|[SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|获取描述符的单个字段|[SQLGetDescField 函数](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|获取描述符的多个字段|[SQLGetDescRec 函数](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|设置描述符的单个字段|[SQLSetDescField 函数](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|设置描述符的多个字段|[SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
