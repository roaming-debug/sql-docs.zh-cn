---
description: SQLBindCol 函数
title: SQLBindCol 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBindCol
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindCol
helpviewer_keywords:
- SQLBindCol function [ODBC]
ms.assetid: 41a37655-84cd-423f-9daa-e0b47b88dc54
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d057b3e7b5842cafba2be18afa21f2d528175ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181089"
---
# <a name="sqlbindcol-function"></a>SQLBindCol 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLBindCol** 将应用程序数据缓冲区绑定到结果集中的列。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLBindCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 ColumnNumber   
 送要绑定的结果集列的编号。 列按递增的列顺序编号，从0开始，其中列0是书签列。 如果未使用书签（即，SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF，则列号从1开始。  
  
 *TargetType*  
 送TargetValuePtr 缓冲区的 C 数据类型的标识符 \*  。 当从具有 **SQLFetch**、 **SQLFetchScroll**、 **SQLBulkOperations** 或 **SQLSetPos** 的数据源检索数据时，驱动程序会将数据转换为此类型;当它通过 **SQLBulkOperations** 或 **SQLSetPos** 将数据发送到数据源时，驱动程序将转换此类型的数据。 有关有效 C 数据类型和类型标识符的列表，请参阅附录 D：数据类型中的 [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md) 部分。  
  
 如果 *TargetType* 参数是 interval 数据类型，则默认间隔的默认间隔 (2) 和默认间隔秒精度 (6) ，分别在 ARD 的 "SQL_DESC_DATETIME_INTERVAL_PRECISION" 和 "SQL_DESC_PRECISION" 字段中进行设置，用于数据。 如果使用了 *TargetType* 参数 SQL_C_NUMERIC，则默认精度 (驱动程序定义) 和默认小数位数 (0) ，这是在 ARD 的 SQL_DESC_PRECISION 和 SQL_DESC_SCALE 字段中设置的，可用于数据。 如果任何默认的精度或小数位数不合适，则应用程序应通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 显式设置相应的描述符字段。  
  
 还可以指定扩展的 C 数据类型。 有关详细信息，请参阅 [ODBC 中的 C 数据类型](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)。  
  
 *TargetValuePtr*  
 [延迟的输入/输出]指向要绑定到列的数据缓冲区的指针。 **SQLFetch** 和 **SQLFetchScroll** 返回此缓冲区中的数据。 当 SQL_FETCH_BY_BOOKMARK *操作* 时， **SQLBulkOperations** 将返回此缓冲区中的数据;当 *操作* SQL_ADD 或 SQL_UPDATE_BY_BOOKMARK 时，它将从此缓冲区检索数据。 当 SQL_REFRESH *操作* 时， **SQLSetPos** 将返回此缓冲区中的数据;当 SQL_UPDATE *操作* 时，它将从该缓冲区检索数据。  
  
 如果 *TargetValuePtr* 为 null 指针，则驱动程序将为列解除数据缓冲区的绑定。 应用程序可以通过使用 SQL_UNBIND 选项调用 **SQLFreeStmt** 来解除所有列的绑定。 如果对 **SQLBindCol** 的调用中的 *TargetValuePtr* 参数为 null 指针，但 *StrLen_or_IndPtr* 参数是有效的值，则应用程序可以解除列的数据缓冲区的绑定，但仍具有绑定到列的长度/指示器缓冲区。  
  
 *BufferLength*  
 送\* *TargetValuePtr* 缓冲区的长度（以字节为单位）。  
  
 当驱动程序 \* 返回长度可变的数据（如字符或二进制数据）时，驱动程序使用 BufferLength 来避免写入超过 *TargetValuePtr* 缓冲区的末尾。 请注意，在将字符数据返回到 TargetValuePtr 时，驱动程序会对 null 终止字符进行计数 \* 。 \*因此， *TargetValuePtr* 必须包含 null 终止字符的空间，否则，驱动程序会截断数据。  
  
 当驱动程序返回固定长度的数据（如整数或日期结构）时，驱动程序将忽略 *BufferLength* ，并假定缓冲区足以容纳数据。 因此，应用程序必须为固定长度的数据分配足够大的缓冲区，否则驱动程序将写入超过缓冲区的末尾。  
  
 当 *BufferLength* 小于0但当 *BufferLength* 为0时， **SQLBindCol** 将返回 SQLSTATE HY090 (字符串或缓冲区长度无效) 。 但是，如果 *TargetType* 指定了字符类型，则应用程序不应将 *BufferLength* 设置为0，因为符合 ISO CLI 的驱动程序将返回 SQLSTATE HY090 (无效的字符串或缓冲区长度) 在这种情况下。  
  
 *StrLen_or_IndPtr*  
 [延迟的输入/输出]指向要绑定到列的长度/指示器缓冲区的指针。 **SQLFetch** 和 **SQLFetchScroll** 返回此缓冲区中的值。 当 *操作* SQL_ADD、SQL_UPDATE_BY_BOOKMARK 或 SQL_DELETE_BY_BOOKMARK 时， **SQLBulkOperations** 从该缓冲区检索值。 当 SQL_FETCH_BY_BOOKMARK *操作* 时， **SQLBulkOperations** 将在此缓冲区中返回一个值。 当 SQL_REFRESH *操作* 时， **SQLSetPos** 将在此缓冲区中返回值;当 SQL_UPDATE *操作* 时，它将从此缓冲区检索值。  
  
 **SQLFetch**、 **SQLFetchScroll**、 **SQLBulkOperations** 和 **SQLSetPos** 可以返回长度/指示器缓冲区中的以下值：  
  
-   可供返回的数据的长度  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 应用程序可以在长度/指示器缓冲区中放置以下值，以便与 **SQLBulkOperations** 或 **SQLSetPos** 一起使用：  
  
-   正在发送的数据的长度  
  
-   SQL_NTS  
  
-   SQL_NULL_DATA  
  
-   SQL_DATA_AT_EXEC  
  
-   SQL_LEN_DATA_AT_EXEC 宏的结果  
  
-   SQL_COLUMN_IGNORE  
  
 如果指示器缓冲区和长度缓冲区是单独的缓冲区，则指示器缓冲区只能返回 SQL_NULL_DATA，而长度缓冲区可以返回所有其他值。  
  
 有关详细信息，请参阅 [SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)、 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)、 [SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)和 [使用长度/指示器值](../../../odbc/reference/develop-app/using-length-and-indicator-values.md)。  
  
 如果 *StrLen_or_IndPtr* 为 null 指针，则不使用长度或指示器值。 这是提取数据时出错，数据为 NULL。  
  
 如果你的应用程序将在64位操作系统上运行，请参阅 [ODBC 64 位信息](../../../odbc/reference/odbc-64-bit-information.md)。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLBindCol** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLBindCol** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突| (DM) *ColumnNumber* 参数为0，但 *TargetType* 参数不是 SQL_C_BOOKMARK 或 SQL_C_VARBOOKMARK。|  
|07009|描述符索引无效|为参数 *ColumnNumber* 指定的值超出了结果集中的最大列数。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY003|应用程序缓冲区类型无效|参数 *TargetType* 既不是有效的数据类型，也不是 SQL_C_DEFAULT。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLBindCol** 时仍在执行此异步函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *BufferLength* 指定的值小于0。<br /><br />  (DM) 该驱动程序是一个 ODBC 2。*x* 驱动程序， *ColumnNumber* 参数设置为0，为参数 *BufferLength* 指定的值不等于4。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持通过 *TargetType* 参数和相应列的特定于驱动程序的 SQL 数据类型组合指定的转换。<br /><br /> 参数 *ColumnNumber* 为0，驱动程序不支持书签。<br /><br /> 驱动程序仅支持 ODBC 2。*x* 和参数 *TargetType* 为以下其中一项：<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> 在附录 D：数据类型中， [c 数据类型](../../../odbc/reference/appendixes/c-data-types.md) 中列出的任何时间间隔 c 数据类型。<br /><br /> 驱动程序仅支持3.50 之前的 ODBC 版本，并 SQL_C_GUID 参数 *TargetType* 。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 **SQLBindCol** 用于将结果集中的列关联或 *绑定* 到应用程序中的数据缓冲区和长度/指示器缓冲区。 当应用程序调用 **SQLFetch**、 **SQLFetchScroll** 或 **SQLSetPos** 获取数据时，驱动程序将为指定缓冲区中的绑定列返回数据;有关详细信息，请参阅 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)。 当应用程序调用 **SQLBulkOperations** 更新或插入行或 **SQLSetPos** 以更新行时，驱动程序将从指定的缓冲区中检索绑定列的数据;有关详细信息，请参阅 [SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md) 或 [SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)。 有关绑定的详细信息，请参阅 [ (基本) 中检索结果 ](../../../odbc/reference/develop-app/retrieving-results-basic.md)。  
  
 请注意，不需要绑定列来从它们中检索数据。 应用程序还可以调用 **SQLGetData** 来检索列中的数据。 尽管可以绑定行中的某些列并为其他列调用 **SQLGetData** ，但这可能会受到某些限制。 有关详细信息，请参阅 [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)。  
  
## <a name="binding-unbinding-and-rebinding-columns"></a>绑定、取消绑定和重新绑定列  
 即使已从结果集中提取数据，也可以随时绑定、取消绑定或重新绑定列。 新绑定将在下一次调用使用绑定的函数时生效。 例如，假设应用程序绑定结果集中的列并调用 **SQLFetch**。 驱动程序将返回绑定缓冲区中的数据。 现在，假设应用程序将列绑定到一组不同的缓冲区。 该驱动程序不会将提取行的数据放入新绑定的缓冲区中。 相反，它会一直等待，直到再次调用 **SQLFetch** ，然后将数据放置到新绑定的缓冲区中的下一行。  
  
> [!NOTE]  
>  应始终在将列绑定到列0之前设置语句属性 SQL_ATTR_USE_BOOKMARKS。 这不是必需的，但强烈建议这样做。  
  
## <a name="binding-columns"></a>绑定列  
 若要绑定列，应用程序将调用 **SQLBindCol** 并传递列号、类型、地址和数据缓冲区的长度以及长度/指示器缓冲区的地址。 有关如何使用这些地址的信息，请参阅本部分后面的 "缓冲区地址"。 有关绑定列的详细信息，请参阅 [Using SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md)。  
  
 这些缓冲区的使用会延迟;也就是说，应用程序将其绑定到 **SQLBindCol** 中，但驱动程序将从其他函数（即 **SQLBulkOperations**、 **SQLFetch**、 **SQLFetchScroll** 或 **SQLSetPos**）进行访问。 应用程序负责确保只要绑定保持有效， **SQLBindCol** 中指定的指针仍然有效。 如果应用程序允许这些指针变为无效（例如，它释放缓冲区，然后调用一个预期其有效的函数），则结果是不确定的。 有关详细信息，请参阅 [延迟缓冲区](../../../odbc/reference/develop-app/deferred-buffers.md)。  
  
 绑定将保持有效，直到它被新绑定替换、列未绑定或释放该语句。  
  
## <a name="unbinding-columns"></a>拆散列  
 若要取消对单个列的绑定，应用程序将调用 **SQLBindCol** ，并将 *ColumnNumber* 设置为该列的数目，并将 *TargetValuePtr* 设置为 null 指针。 如果 *ColumnNumber* 引用了未绑定列，则 **SQLBindCol** 仍返回 SQL_SUCCESS。  
  
 若要取消绑定所有列，应用程序将调用 **SQLFreeStmt** ，并将 *fOption* 设置为 SQL_UNBIND。 还可以通过将 ARD 的 SQL_DESC_COUNT 字段设置为零来实现此目的。  
  
## <a name="rebinding-columns"></a>重新绑定列  
 应用程序可以执行以下两个操作之一来更改绑定：  
  
-   调用 **SQLBindCol** 为已绑定的列指定新绑定。 驱动程序用新的绑定覆盖旧的绑定。  
  
-   指定要添加到由对 **SQLBindCol** 的绑定调用指定的缓冲区地址的偏移量。 有关详细信息，请参阅下一节 "绑定偏移量"。  
  
## <a name="binding-offsets"></a>绑定偏移量  
 绑定偏移量是在取消引用之前，添加到数据和长度/指示器缓冲区 (在 *TargetValuePtr* 和 *StrLen_or_IndPtr*) 参数中指定的值的值。 使用偏移量时，绑定是应用程序缓冲区布局方式的 "模板"，应用程序可以通过更改偏移量将此 "模板" 移到不同的内存区域。 由于每个绑定中的每个地址都添加了相同的偏移量，因此每个缓冲区集中的不同列的缓冲区间的相对偏移量必须相同。 当使用按行绑定时，这始终为 true;应用程序必须仔细地布局其缓冲区，以便在使用按列绑定时，此属性为 true。  
  
 使用绑定偏移量与通过调用 **SQLBindCol** 重新绑定列基本相同。 不同之处是，对 **SQLBindCol** 的新调用指定了数据缓冲区和长度/指示器缓冲区的新地址，而使用绑定偏移量不会更改地址，只是向其添加偏移量。 应用程序可以在需要时指定新的偏移量，并且始终会将此偏移量添加到最初绑定的地址。 尤其是，如果偏移量设置为0，或者如果语句特性设置为 null 指针，则驱动程序将使用最初绑定的地址。  
  
 为了指定绑定偏移量，应用程序将 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性设置为 SQLINTEGER 缓冲区的地址。 在应用程序调用使用绑定的函数之前，它会在此缓冲区中的偏移量（以字节为单位）。 为了确定要使用的缓冲区的地址，驱动程序将偏移量添加到绑定中的地址。 地址与偏移量之和必须是有效地址，但偏移量的添加地址不一定有效。 有关如何使用绑定偏移量的详细信息，请参阅本部分后面的 "缓冲区地址"。  
  
## <a name="binding-arrays"></a>绑定数组  
 如果行集大小 (SQL_ATTR_ROW_ARRAY_SIZE 语句特性的值) 大于1，则应用程序会绑定缓冲区的数组而不是单个缓冲区。 有关详细信息，请参阅 [块游标](../../../odbc/reference/develop-app/block-cursors.md)。  
  
 应用程序可以通过两种方式绑定数组：  
  
-   将数组绑定到每个列。 这称为按 *列绑定* ，因为每个数据结构 (数组) 包含单个列的数据。  
  
-   定义一个结构来保存整行的数据，并绑定这些结构的数组。 这称为按 *行绑定* ，因为每个数据结构包含单个行的数据。  
  
 每个缓冲区数组都必须具有至少与行集大小相同的元素。  
  
> [!NOTE]  
>  应用程序必须验证对齐是否有效。 有关对齐注意事项的详细信息，请参阅 [对齐](../../../odbc/reference/develop-app/alignment.md)。  
  
## <a name="column-wise-binding"></a>按列绑定  
 在按列绑定中，应用程序将单独的数据和长度/指示器数组绑定到每个列。  
  
 若要使用按列绑定，应用程序首先将 SQL_ATTR_ROW_BIND_TYPE 语句特性设置为 SQL_BIND_BY_COLUMN。  (这是默认值。 ) 对于每个要绑定的列，应用程序执行以下步骤：  
  
1.  分配数据缓冲区数组。  
  
2.  分配长度/指示器缓冲区的数组。  
  
    > [!NOTE]  
    >  当使用按列绑定时，如果应用程序直接写入描述符，则可以使用单独的数组作为长度和指示器数据。  
  
3.  调用具有以下参数的 **SQLBindCol** ：  
  
    -   *TargetType* 是数据缓冲区数组中单个元素的类型。  
  
    -   *TargetValuePtr* 是数据缓冲区数组的地址。  
  
    -   *BufferLength* 是数据缓冲区数组中单个元素的大小。 当数据为固定长度数据时，将忽略 *BufferLength* 参数。  
  
    -   *StrLen_or_IndPtr* 是长度/指示器数组的地址。  
  
 有关如何使用此信息的详细信息，请参阅本部分后面的 "缓冲区地址"。 有关按列绑定的详细信息，请参阅按 [列绑定](../../../odbc/reference/develop-app/column-wise-binding.md)。  
  
## <a name="row-wise-binding"></a>按行绑定  
 在按行绑定中，应用程序将定义一个结构，该结构包含每个要绑定的列的数据和长度/指示器缓冲区。  
  
 若要使用按行绑定，应用程序需要执行以下步骤：  
  
1.  定义用于保存单个数据行 (的结构，包括数据和长度/指示器缓冲区) 并分配这些结构的数组。  
  
    > [!NOTE]  
    >  当使用按行绑定时，如果应用程序直接写入描述符，则可以将单独的字段用于长度和指示器数据。  
  
2.  将 SQL_ATTR_ROW_BIND_TYPE 语句特性设置为包含单个数据行的结构的大小，或设置为结果列将绑定到的缓冲区的实例大小的。 长度必须包含所有绑定列的空间以及结构或缓冲区的任何填充，以确保当绑定列的地址递增指定的长度时，结果将指向下一行中同一列的开头。 在 ANSI C 中使用 *sizeof* 运算符时，此行为是保证的。  
  
3.  对于每个要绑定的列，调用 **SQLBindCol** ，并提供以下参数：  
  
    -   *TargetType* 是要绑定到列的数据缓冲区成员的类型。  
  
    -   *TargetValuePtr* 是第一个数组元素中的数据缓冲区成员的地址。  
  
    -   *BufferLength* 是数据缓冲区成员的大小。  
  
    -   *StrLen_or_IndPtr* 是要绑定的长度/指示器成员的地址。  
  
 有关如何使用此信息的详细信息，请参阅本部分后面的 "缓冲区地址"。 有关按列绑定的详细信息，请参阅按 [行绑定](../../../odbc/reference/develop-app/row-wise-binding.md)。  
  
## <a name="buffer-addresses"></a>缓冲区地址  
 *缓冲区地址* 是数据或长度/指示器缓冲区的实际地址。 驱动程序会在写入缓冲区之前计算缓冲区地址 (例如在提取期间) 。 它是通过下面的公式计算的，它使用在 *TargetValuePtr* 和 *StrLen_or_IndPtr* 参数中指定的地址、绑定偏移量和行号：  
  
 *绑定地址*  + *绑定偏移量*+ ( # B1 *行号*-1) x *元素大小*)   
  
 公式变量的定义位置如下表中所述。  
  
|变量|说明|  
|--------------|-----------------|  
|*绑定地址*|对于数据缓冲区，为 **SQLBindCol** 中的 *TargetValuePtr* 参数指定的地址。<br /><br /> 对于长度/指示器缓冲区，为 **SQLBindCol** 中的 *StrLen_or_IndPtr* 参数指定的地址。 有关详细信息，请参阅 "描述符和 SQLBindCol" 部分中的 "其他注释"。<br /><br /> 如果绑定地址是0，则不会返回任何数据值，即使之前公式计算的地址不为零。|  
|*绑定偏移量*|如果使用按行绑定，则使用 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性指定的地址上存储的值。<br /><br /> 如果使用按列绑定或 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性的值为 null 指针，则 *绑定偏移量* 为0。|  
|*行号*|行集中从1开始的行号。 对于单行读取（默认值），此值为1。|  
|*元素大小*|绑定数组中元素的大小。<br /><br /> 如果使用按列绑定，则为长度/指示器缓冲区的 **sizeof (SQLINTEGER)** 。 对于数据缓冲区，如果数据类型为可变长度，则为 *BufferLength* 参数的值 **; 如果数据** 类型为固定长度，则为数据类型的大小。<br /><br /> 如果使用按行绑定，则这是数据和长度/指示器缓冲区的 SQL_ATTR_ROW_BIND_TYPE 语句特性的值。|  
  
## <a name="descriptors-and-sqlbindcol"></a>描述符和 SQLBindCol  
 以下各节介绍了 **SQLBindCol** 如何与描述符交互。  
  
> [!CAUTION]  
>  对一个语句调用 **SQLBindCol** 可能会影响其他语句。 如果与该语句关联的 ARD 已显式分配并与其他语句相关联，则会发生这种情况。 由于 **SQLBindCol** 修改了描述符，因此修改应用于与此描述符关联的所有语句。 如果这不是所需的行为，应用程序应在调用 **SQLBindCol** 之前，将此描述符与其他语句取消关联。  
  
## <a name="argument-mappings"></a>参数映射  
 从概念上讲， **SQLBindCol** 按顺序执行以下步骤：  
  
1.  调用 **SQLGetStmtAttr** 以获取 ARD 句柄。  
  
2.  调用 **SQLGetDescField** 以获取此描述符的 SQL_DESC_COUNT 字段，如果 *ColumnNumber* 参数中的值超出 SQL_DESC_COUNT 的值，则调用 **SQLSetDescField** 以将 SQL_DESC_COUNT 的值增加到 *ColumnNumber*。  
  
3.  多次调用 **SQLSetDescField** ，将值分配给 ARD 的以下字段：  
  
    -   将 SQL_DESC_TYPE 和 SQL_DESC_CONCISE_TYPE 设置为 *TargetType* 的值，只不过当 *targettype* 是 datetime 或 interval 子类型的简洁标识符之一时，它会将 SQL_DESC_TYPE 分别设置为 SQL_DATETIME 或 SQL_INTERVAL;将 SQL_DESC_CONCISE_TYPE 设置为简洁标识符;并将 SQL_DESC_DATETIME_INTERVAL_CODE 设置为相应的 DATETIME 或 INTERVAL 子代码。  
  
    -   根据 *TargetType* 的需要，设置一个或多个 SQL_DESC_LENGTH、SQL_DESC_PRECISION、SQL_DESC_SCALE 和 SQL_DESC_DATETIME_INTERVAL_PRECISION。  
  
    -   将 SQL_DESC_OCTET_LENGTH 字段设置为 *BufferLength* 的值。  
  
    -   将 SQL_DESC_DATA_PTR 字段设置为 *TargetValuePtr* 的值。  
  
    -   将 SQL_DESC_INDICATOR_PTR 字段设置为 *StrLen_or_IndPtr* 的值。  (参见以下段落。 )   
  
    -   将 SQL_DESC_OCTET_LENGTH_PTR 字段设置为 *StrLen_or_IndPtr* 的值。  (参见以下段落。 )   
  
 *StrLen_or_IndPtr* 参数引用的变量用于指示器和长度信息。 如果提取遇到列的 null 值，则它将 SQL_NULL_DATA 存储在此变量中;否则，它会将数据长度存储在此变量中。 将 null 指针作为 *StrLen_or_IndPtr* 传递会使提取操作返回数据长度，但如果遇到 null 值并且无法返回 SQL_NULL_DATA，则会使提取失败。  
  
 如果对 **SQLBindCol** 的调用失败，则它将在 ARD 中设置的描述符字段的内容是未定义的，并且 ARD 的 SQL_DESC_COUNT 字段的值不变。  
  
## <a name="implicit-resetting-of-count-field"></a>计数字段的隐式重置  
 仅当这会增加 SQL_DESC_COUNT 的值时， **SQLBindCol** 才会将 SQL_DESC_COUNT 设置为 *ColumnNumber* 参数的值。 如果 *TargetValuePtr* 参数中的值为 null 指针，并且 *ColumnNumber* 参数中的值等于 SQL_DESC_COUNT (即，当取消绑定) 的最大绑定列时，SQL_DESC_COUNT 设置为剩余的最大绑定列的数目。  
  
## <a name="cautions-regarding-sql_default"></a>关于 SQL_DEFAULT 的注意事项  
 若要成功检索列数据，应用程序必须确定应用程序缓冲区中数据的长度和起点。 当应用程序指定显式 *TargetType* 时，可以轻松检测到应用程序误解。 但是，当应用程序指定 SQL_DEFAULT 的 *TargetType* 时， **SQLBindCol** 可应用于应用程序所需的不同数据类型的列，无论是对元数据的更改还是通过将代码应用于其他列。 在这种情况下，应用程序可能不会始终确定提取的列数据的开始或长度。 这可能会导致未报告的数据错误或内存冲突。  
  
## <a name="code-example"></a>代码示例  
 在下面的示例中，应用程序对 Customers 表执行 **SELECT** 语句以返回客户 id、名称和电话号码的结果集，按名称排序。 然后，它调用 **SQLBindCol** 将数据的列绑定到本地缓冲区。 最后，应用程序通过 **SQLFetch** 提取每个数据行，并打印每个客户的姓名、ID 和电话号码。  
  
 有关更多代码示例，请参阅 [SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)、 [SQLColumns 函数](../../../odbc/reference/syntax/sqlcolumns-function.md)、 [SQLFetchScroll 函数](../../../odbc/reference/syntax/sqlfetchscroll-function.md)和 [SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)。  
  
```cpp  
// SQLBindCol_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
  
#define UNICODE  
#include <sqlext.h>  
  
#define NAME_LEN 50  
#define PHONE_LEN 60
  
void show_error() {  
   printf("error\n");  
}  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
   SQLWCHAR szName[NAME_LEN], szPhone[PHONE_LEN], sCustID[NAME_LEN];  
   SQLLEN cbName = 0, cbCustID = 0, cbPhone = 0;  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLWCHAR*) L"NorthWind", SQL_NTS, (SQLWCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {   
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               retcode = SQLExecDirect(hstmt, (SQLWCHAR *) L"SELECT CustomerID, ContactName, Phone FROM CUSTOMERS ORDER BY 2, 1, 3", SQL_NTS);  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
                  // Bind columns 1, 2, and 3  
                  retcode = SQLBindCol(hstmt, 1, SQL_C_WCHAR, &sCustID, 100, &cbCustID);  
                  retcode = SQLBindCol(hstmt, 2, SQL_C_WCHAR, szName, NAME_LEN, &cbName);  
                  retcode = SQLBindCol(hstmt, 3, SQL_C_WCHAR, szPhone, PHONE_LEN, &cbPhone);   
  
                  // Fetch and print each row of data. On an error, display a message and exit.  
                  for (int i=0 ; ; i++) {  
                     retcode = SQLFetch(hstmt);  
                     if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
                        show_error();  
                     if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                     {
                        //replace wprintf with printf
                        //%S with %ls
                        //warning C4477: 'wprintf' : format string '%S' requires an argument of type 'char *'
                        //but variadic argument 2 has type 'SQLWCHAR *'
                        //wprintf(L"%d: %S %S %S\n", i + 1, sCustID, szName, szPhone);  
                        printf("%d: %ls %ls %ls\n", i + 1, sCustID, szName, szPhone);  
                    }    
                     else  
                        break;  
                  }  
               }  
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLCancel(hstmt);  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
 另请参阅 [示例 ODBC 程序](../../../odbc/reference/sample-odbc-program.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|返回有关结果集中的列的信息|[SQLDescribeCol 函数](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|提取多行数据|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|释放语句上的列缓冲区|[SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|提取部分或全部数据列|[SQLGetData 函数](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|返回结果集列的数目|[SQLNumResultCols 函数](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
