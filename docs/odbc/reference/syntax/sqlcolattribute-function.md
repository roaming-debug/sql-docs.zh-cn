---
description: SQLColAttribute 函数
title: SQLColAttribute 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce0bd87f2a4e2a9306bb5fa94add7fb1ee583dfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208510"
---
# <a name="sqlcolattribute-function"></a>SQLColAttribute 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLColAttribute** 返回结果集中列的描述符信息。 说明符信息以字符串、描述符相关值或整数值的形式返回。  
  
> [!NOTE]  
>  有关在 ODBC 3 时驱动程序管理器将此函数映射到的内容的详细信息。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 ColumnNumber   
 送要从中检索字段值的 IRD 中的记录数。 此参数对应于结果数据的列数，按升序顺序排序（从1开始）。 可以按任意顺序描述列。  
  
 可以在此参数中指定列0，但是除 SQL_DESC_TYPE 和 SQL_DESC_OCTET_LENGTH 以外的所有值都将返回未定义的值。  
  
 *FieldIdentifier*  
 送描述符句柄。 此句柄定义应该在 IRD 中查询的字段 (例如 SQL_COLUMN_TABLE_NAME) 。  
  
 *CharacterAttributePtr*  
 输出指向缓冲区的指针，该缓冲区用于在 IRD 的 *ColumnNumber* 行的 *FieldIdentifier* 字段中返回值（如果该字段是一个字符串）。 否则，将不使用该字段。  
  
 如果 *CharacterAttributePtr* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的总字节数) 可在 *CharacterAttributePtr* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送如果 *FieldIdentifier* 是一个 ODBC 定义的字段，而 *CharacterAttributePtr* 指向某个字符串或二进制缓冲区，则此参数应为 CharacterAttributePtr 的长度 \* 。 如果 *FieldIdentifier* 是 ODBC 定义的字段，而 \* *CharacterAttribute* Ptr 是整数，则忽略此字段。 如果在调用 **SQLColAttributeW**) 时， *\* CharacterAttributePtr* 是 Unicode 字符串 (，则 *BufferLength* 参数必须是偶数。 如果 *FieldIdentifier* 是驱动程序定义的字段，应用程序会通过设置 *BufferLength* 参数来指示该字段的特性。 *BufferLength* 可具有以下值：  
  
-   如果 *CharacterAttributePtr* 是指向指针的指针，则 *BufferLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *CharacterAttributePtr* 是指向字符串的指针，则 *BufferLength* 是缓冲区的长度。  
  
-   如果 *CharacterAttributePtr* 是一个指向二进制缓冲区的指针，应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *BufferLength* 中。 这会在 *BufferLength* 中置入负值。  
  
-   如果 *CharacterAttributePtr* 是指向固定长度数据类型的指针，则 *BufferLength* 必须是以下类型之一： SQL_IS_INTEGER、SQL_IS_UINTEGER、SQL_IS_SMALLINT 或 SQL_IS_USMALLINT。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，该缓冲区返回的字节总数 (排除字符数据的 null 终止字节数) 可在 **CharacterAttributePtr* 中返回。  
  
 对于字符数据，如果可返回的字节数大于或等于 *BufferLength*，则 CharacterAttributePtr 中的描述符信息将 \* 截断为 *BufferLength* 减去 null 终止字符的长度，并以 null 值终止于驱动程序。  
  
 对于所有其他类型的数据， *BufferLength* 的值将被忽略，驱动程序假定大小 **CharacterAttributePtr* 为32位。  
  
 *NumericAttributePtr*  
 输出一个指针，指向要在 IRD 的 *ColumnNumber* 行的 *FieldIdentifier* 字段中返回值的整数缓冲区（如果该字段是数值描述符类型，如 SQL_DESC_COLUMN_LENGTH。 否则，将不使用该字段。 请注意，某些驱动程序只能写入缓冲区中的较低32位或16位，并使高阶位保持不变。 因此，在调用此函数之前，应用程序应将值初始化为0。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLColAttribute** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLColAttribute** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *CharacterAttributePtr* 不够大，无法返回整个字符串值，因此字符串值已截断。 在 **StringLengthPtr* 中返回未截断字符串值的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07005|预定义语句不是 *游标规范*|与 *StatementHandle* 关联的语句未返回结果集， *FieldIdentifier* 未 SQL_DESC_COUNT。 没有要描述的列。|  
|07009|描述符索引无效| (DM) 为 *ColumnNumber* 指定的值等于0，SQL_ATTR_USE_BOOKMARKS 语句属性已 SQL_UB_OFF。<br /><br /> 为参数 *ColumnNumber* 指定的值大于结果集中的列数。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 **SQLGetDiagField** 从诊断数据结构返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 SQLColAttribute 时，仍在执行此 aynchronous 函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 在调用 **SQLPrepare**、 **SQLExecDirect** 或 *StatementHandle* 的目录函数之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) *\* CharacterAttributePtr* 是一个字符串，而 *BufferLength* 小于0但不等于 SQL_NTS。|  
|HY091|描述符字段标识符无效|为参数 *FieldIdentifier* 指定的值不是定义的值之一，并且该值不是实现定义的值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|驱动程序不支持|驱动程序不支持为参数 *FieldIdentifier* 指定的值。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
 当在 **SQLPrepare** 之后和 **SQLExecute** 之前调用时， **SQLColAttribute** 可以返回可由 **SQLPrepare** 或 **SQLExecute** 返回的任何 SQLSTATE，具体取决于数据源评估与 *StatementHandle* 关联的 SQL 语句的时间。  
  
 出于性能方面的考虑，应用程序在执行语句之前不应调用 **SQLColAttribute** 。  
  
## <a name="comments"></a>注释  
 有关应用程序如何使用 **SQLColAttribute** 返回的信息的信息，请参阅 [结果集元数据](../../../odbc/reference/develop-app/result-set-metadata.md)。  
  
 **SQLColAttribute** 在 \* *NumericAttributePtr* 或 \* *CharacterAttributePtr* 中返回信息。 整数信息在 \* *NumericAttributePtr* 中作为 SQLLEN 值返回; 所有其他格式的信息在 CharacterAttributePtr 中返回 \* 。 当在 NumericAttributePtr 中返回信息时 \* ，驱动程序将忽略 *CharacterAttributePtr*、 *BufferLength* 和 *StringLengthPtr*。 当在 CharacterAttributePtr 中返回信息时 \* ，驱动程序将忽略 *NumericAttributePtr*。  
  
 **SQLColAttribute** 从 IRD 的描述符字段返回值。 使用语句句柄而不是描述符句柄调用函数。 此部分后面列出的 *FieldIdentifier* 值的 **SQLColAttribute** 返回的值还可以通过使用相应的 IRD 句柄调用 **SQLGetDescField** 来检索。  
  
 本部分后面显示了当前定义的描述符字段、引入这些字段的 ODBC 的版本，以及为它们返回信息的参数。驱动程序可以定义更多的描述符类型以利用不同的数据源。  
  
 ODBC 3。*x* 驱动程序必须为每个描述符字段返回一个值。 如果描述符字段不适用于驱动程序或数据源，除非另有说明，否则，驱动程序将在 StringLengthPtr 中返回0， \* 或在 **CharacterAttributePtr* 中返回空字符串。  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 ODBC 3。*x* 函数 **SQLColAttribute** 取代了弃用的 ODBC 2。*x* 函数 **SQLColAttributes**。 当 ODBC 2 时，将 **SQLColAttributes** 映射到 **SQLColAttribute** (。*x* 应用程序正在使用 ODBC 3。*x* 驱动程序) ，或在 ODBC 3 时将 **SQLColAttribute** 映射到 **SQLColAttributes** (。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序) ，驱动程序管理器将 *FieldIdentifier* 的值传递到，将其映射到新值或返回错误，如下所示：  
  
> [!NOTE]  
>  ODBC 3 的 *FieldIdentifier* 值中使用的前缀。*x* 已从 ODBC 2 中使用的进行了更改。*x*。 新前缀为 "SQL_DESC";旧前缀为 "SQL_COLUMN"。  
  
-   如果为，则为 ODBC 2 的 **#define** 值。*x* *FieldIdentifier* 与 ODBC 3 的 **#define** 值相同。*x* *FieldIdentifier*，函数调用中的值只通过传递。  
  
-   ODBC 2 的 **#define** 值。*x* *FieldIdentifiers* SQL_COLUMN_LENGTH、SQL_COLUMN_PRECISION 和 SQL_COLUMN_SCALE 与 ODBC 3 的 **#define** 值不同。*x* *FieldIdentifiers* SQL_DESC_PRECISION、SQL_DESC_SCALE 和 SQL_DESC_LENGTH。 ODBC 2。*x* 驱动程序只需支持 ODBC 2。*x* 值。 ODBC 3。*x* 驱动程序必须支持这三个 *FieldIdentifiers* 的 "SQL_COLUMN" 和 "SQL_DESC" 值。 这些值是不同的，因为在 ODBC 3 中以不同的方式定义精度、小数位数和长度。*x* 比在 ODBC 2 中一样。*x*。 有关详细信息，请参阅 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。  
  
-   如果为，则为 ODBC 2 的 **#define** 值。*x* *FIELDIDENTIFIER* 不同于 ODBC 3 的 **#define** 值。*x* *FIELDIDENTIFIER*，与计数、名称和可以为 null 的值一样，函数调用中的值会映射到相应的值。 例如，SQL_COLUMN_COUNT 映射到 SQL_DESC_COUNT，SQL_DESC_COUNT 映射到 SQL_COLUMN_COUNT，具体取决于映射的方向。  
  
-   如果 *FieldIdentifier* 是 ODBC 3 中的新值，则为。*x*，在 ODBC 2 中没有相应的值。*x*，在 ODBC 3 时不会映射。*x* 应用程序在 ODBC 2 中对 **SQLColAttribute** 的调用中使用它。*x* 驱动程序，调用将返回 SQLSTATE HY091 (无效的描述符字段标识符) 。  
  
 下表列出了 **SQLColAttribute** 返回的描述符类型。 *NumericAttributePtr* 值的类型为 **SQLLEN \** _。  
  
|_FieldIdentifier *|信息<br /><br /> 返回的|说明|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC 1.0) |*NumericAttributePtr*|SQL_TRUE 列是否为自动递增列。<br /><br /> 如果列不是自动递增列或者不是数字，则 SQL_FALSE。<br /><br /> 此字段仅对数值数据类型列有效。 应用程序可以将值插入到包含自动增量列的行中，但通常不能更新列中的值。<br /><br /> 当在自动增量列中插入内容时，会在插入时将唯一值插入列。 增量未定义，但特定于数据源。 应用程序不应假定自动增量列以任何特定的点启动或按任何特定值递增。|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3.0) |*CharacterAttributePtr*|结果集列的基本列名称。 如果基列名称不存在 (与) 表达式的列的情况相同），则此变量包含空字符串。<br /><br /> 此信息从 IRD 的 "SQL_DESC_BASE_COLUMN_NAME 记录" 字段返回，这是一个只读字段。|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3.0) |*CharacterAttributePtr*|包含列的基表的名称。 如果基表名称不能定义或不适用，则此变量包含空字符串。<br /><br /> 此信息从 IRD 的 "SQL_DESC_BASE_TABLE_NAME 记录" 字段返回，这是一个只读字段。|  
|SQL_DESC_CASE_SENSITIVE (ODBC 1.0) |*NumericAttributePtr*|SQL_TRUE 如果将列视为排序规则和比较区分大小写，则为。<br /><br /> SQL_FALSE 如果未将列视为排序规则和比较区分大小写，则为非字符。|  
|SQL_DESC_CATALOG_NAME (ODBC 2.0) |*CharacterAttributePtr*|包含列的表的目录。 如果列是表达式或者列是视图的一部分，则返回的值是实现定义的。 如果数据源不支持目录或无法确定目录名称，则返回空字符串。 此 VARCHAR record 字段不能超过128个字符。|  
|SQL_DESC_CONCISE_TYPE (ODBC 1.0) |*NumericAttributePtr*|简洁的数据类型。<br /><br /> 对于 datetime 和 interval 数据类型，此字段返回简洁的数据类型;例如，SQL_TYPE_TIME 或 SQL_INTERVAL_YEAR。  (有关详细信息，请参阅附录 D：数据类型中的 [数据类型标识符和描述符](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) ) <br /><br /> 此信息从 IRD 的 "SQL_DESC_CONCISE_TYPE 记录" 字段返回。|  
|SQL_DESC_COUNT (ODBC 1.0) |*NumericAttributePtr*|结果集中可用的列数。 如果结果集中没有列，则返回0。 *ColumnNumber* 参数中的值将被忽略。<br /><br /> 此信息从 IRD 的 SQL_DESC_COUNT 标头字段返回。|  
|SQL_DESC_DISPLAY_SIZE (ODBC 1.0) |*NumericAttributePtr*|显示列中的数据所需的最大字符数。 有关显示大小的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC 1.0) |*NumericAttributePtr*|如果列具有特定于数据源的固定精度和非零刻度，则 SQL_TRUE。<br /><br /> 如果列没有特定于数据源的固定精度和非零刻度，则 SQL_FALSE。|  
|SQL_DESC_LABEL (ODBC 2.0) |*CharacterAttributePtr*|列标签或标题。 例如，名为 EmpName 的列可能是名为 "雇员姓名" 的列，或者标记有别名。<br /><br /> 如果列没有标签，则返回列名称。 如果该列未标记且未命名，则返回空字符串。|  
|SQL_DESC_LENGTH (ODBC 3.0) |*NumericAttributePtr*|一个数值，它是字符串或二进制数据类型的最大字符长度或实际字符长度。 它是固定长度的数据类型的最大字符长度，或可变长度数据类型的实际字符长度。 其值始终排除以字符串结尾的 null 终止字节。<br /><br /> 此信息从 IRD 的 "SQL_DESC_LENGTH 记录" 字段返回。<br /><br /> 有关长度的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3.0) |*CharacterAttributePtr*|此 VARCHAR (128) 记录字段包含驱动程序将其识别为此数据类型的文本前缀的一个或多个字符。 对于不适用文本前缀的数据类型，此字段包含空字符串。 有关详细信息，请参阅 [文本前缀和后缀](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md)。|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3.0) |*CharacterAttributePtr*|此 VARCHAR (128) 记录字段包含驱动程序识别为此数据类型的文本后缀的一个或多个字符。 对于不适用文本后缀的数据类型，此字段包含空字符串。 有关详细信息，请参阅 [文本前缀和后缀](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md)。|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3.0) |*CharacterAttributePtr*|此 VARCHAR (128) 记录字段包含与数据类型的常规名称不同的任何本地化 (本机语言) 名称。 如果没有本地化名称，则返回空字符串。 此字段仅用于显示目的。 字符串的字符集与区域设置相关，通常是服务器的默认字符集。|  
|SQL_DESC_NAME (ODBC 3.0) |*CharacterAttributePtr*|列别名（如果适用）。 如果列别名不适用，则返回列名称。 在任一情况下，SQL_DESC_UNNAMED 设置为 SQL_NAMED。 如果没有列名称或列别名，则返回空字符串，并将 SQL_DESC_UNNAMED 设置为 SQL_UNNAMED。<br /><br /> 此信息从 IRD 的 "SQL_DESC_NAME 记录" 字段返回。|  
|SQL_DESC_NULLABLE (ODBC 3.0) |*NumericAttributePtr*|如果列可以包含 NULL 值，则 SQL_ 可以为 null;如果列没有 NULL 值，则 SQL_NO_NULLS;如果不知道列是否接受空值，则为或 SQL_NULLABLE_UNKNOWN。<br /><br /> 此信息从 IRD 的 "SQL_DESC_NULLABLE 记录" 字段返回。|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3.0) |*NumericAttributePtr*|如果 SQL_DESC_TYPE 字段中的数据类型是近似数值数据类型，则此 SQLINTEGER 字段包含值2，因为 SQL_DESC_PRECISION 字段包含位数。 如果 SQL_DESC_TYPE 字段中的数据类型是精确数值数据类型，则此字段包含值10，因为 SQL_DESC_PRECISION 字段包含十进制数字的位数。 对于所有非数值数据类型，此字段都设置为0。|  
|SQL_DESC_OCTET_LENGTH (ODBC 3.0) |*NumericAttributePtr*|字符串或二进制数据类型的长度（以字节为单位）。 对于固定长度的字符或二进制类型，这是实际的长度（以字节为单位）。 对于长度可变的字符或二进制类型，这是最大长度（以字节为单位）。 此值不包括 null 终止符。<br /><br /> 此信息从 IRD 的 "SQL_DESC_OCTET_LENGTH 记录" 字段返回。<br /><br /> 有关长度的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|SQL_DESC_PRECISION (ODBC 3.0) |*NumericAttributePtr*|数值数据类型的数值表示适用的精度。 对于数据类型 SQL_TYPE_TIME、SQL_TYPE_TIMESTAMP 以及表示时间间隔的所有时间间隔数据类型，其值为秒的小数部分的适用精度。<br /><br /> 此信息从 IRD 的 "SQL_DESC_PRECISION 记录" 字段返回。|  
|SQL_DESC_SCALE (ODBC 3.0) |*NumericAttributePtr*|一个数值，它是数值数据类型适用的小数位数。 对于 DECIMAL 和 NUMERIC 数据类型，这是定义的小数位数。 对于所有其他数据类型，它是不确定的。<br /><br /> 此信息从 IRD 的 "缩放记录" 字段返回。|  
|SQL_DESC_SCHEMA_NAME (ODBC 2.0) |*CharacterAttributePtr*|包含列的表的架构。 如果列是表达式或者列是视图的一部分，则返回的值是实现定义的。 如果数据源不支持架构，或者无法确定架构名称，则返回空字符串。 此 VARCHAR record 字段不能超过128个字符。|  
|SQL_DESC_SEARCHABLE (ODBC 1.0) |*NumericAttributePtr*|如果列不能用于 WHERE 子句，则 SQL_PRED_NONE。  (这与 ODBC 2 中的 SQL_UNSEARCHABLE 值相同。*x.x.x.x*) <br /><br /> 如果列可以在 WHERE 子句中使用，则 SQL_PRED_CHAR （仅适用于 LIKE 谓词）。  (这与 ODBC 2 中的 SQL_LIKE_ONLY 值相同。*x.x.x.x*) <br /><br /> 如果列可用于包含所有比较运算符（如除外）的 WHERE 子句中，则 SQL_PRED_BASIC。  (这与 ODBC 2 中的 SQL_EXCEPT_LIKE 值相同。*x.x.x.x*) <br /><br /> 如果可以在 WHERE 子句中将列用于任何比较运算符，则 SQL_PRED_SEARCHABLE。<br /><br /> SQL_LONGVARCHAR 和 SQL_LONGVARBINARY 类型的列通常返回 SQL_PRED_CHAR。|  
|SQL_DESC_TABLE_NAME (ODBC 2.0) |*CharacterAttributePtr*|包含该列的表的名称。 如果列是表达式或者列是视图的一部分，则返回的值是实现定义的。<br /><br /> 如果表名不能确定，则返回空字符串。|  
|SQL_DESC_TYPE (ODBC 3.0) |*NumericAttributePtr*|指定 SQL 数据类型的数值。<br /><br /> 当 *ColumnNumber* 等于0时，将返回可变长度书签的 SQL_BINARY，并为固定长度书签返回 SQL_INTEGER。<br /><br /> 对于 datetime 和 interval 数据类型，此字段返回详细数据类型： SQL_DATETIME 或 SQL_INTERVAL。 有关详细信息，请参阅附录 D：数据类型中的 [数据类型标识符和描述符](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) (。<br /><br /> 此信息从 IRD 的 "SQL_DESC_TYPE 记录" 字段返回。 **注意：**  针对 ODBC 2 工作。*x* 驱动程序，请改用 SQL_DESC_CONCISE_TYPE。|  
|SQL_DESC_TYPE_NAME (ODBC 1.0) |*CharacterAttributePtr*|依赖于数据源的数据类型名称;例如，"CHAR"、"VARCHAR"、"MONEY"、"LONG VARBINARY" 或 "CHAR ( ) 用于位数据"。<br /><br /> 如果类型未知，则返回空字符串。|  
|SQL_DESC_UNNAMED (ODBC 3.0) |*NumericAttributePtr*|SQL_NAMED 或 SQL_UNNAMED。 如果 IRD 的 SQL_DESC_NAME 字段包含列别名或列名，则返回 SQL_NAMED。 如果没有列名称或列别名，则返回 SQL_UNNAMED。<br /><br /> 此信息从 IRD 的 "SQL_DESC_UNNAMED 记录" 字段返回。|  
|SQL_DESC_UNSIGNED (ODBC 1.0) |*NumericAttributePtr*|如果列未签名 (或非数字) ，则 SQL_TRUE。<br /><br /> SQL_FALSE 是否对列进行了签名。|  
|SQL_DESC_UPDATABLE (ODBC 1.0) |*NumericAttributePtr*|列由定义的常量的值描述：<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE 描述结果集中的列的更新，而不是基表中的列。 结果集列所基于的基本列的可更新性可能与此字段中的值不同。 列是否可更新可以基于数据类型、用户权限和结果集本身的定义。 如果某列是否可更新，则应返回 SQL_ATTR_READWRITE_UNKNOWN。|  
  
 **SQLColAttribute** 是 **SQLDescribeCol** 的可扩展替代方法。 **SQLDescribeCol** 基于 ANSI-89 SQL 返回一组固定的描述符信息。 **SQLColAttribute** 允许访问 ANSI SQL-92 和 DBMS 供应商扩展中提供的更广泛的描述符信息。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回有关结果集中的列的信息|[SQLDescribeCol 函数](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|提取多行数据|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>示例  
 下面的示例代码不释放句柄和连接。 有关释放句柄和语句的代码示例，请参阅 [SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)、 [示例 ODBC 程序](../../../odbc/reference/sample-odbc-program.md)和 [SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md) 。  
  
```cpp  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;
   
   retCode = SQLNumResultCols(hstmt, &numColumn);  
   
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [示例 ODBC 程序](../../../odbc/reference/sample-odbc-program.md)
