---
description: SQLSetDescField 函数
title: SQLSetDescField 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescField
helpviewer_keywords:
- SQLSetDescField function [ODBC]
ms.assetid: 8c544388-fe9d-4f94-a0ac-fa0b9c9c88a5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bd48da2caec1acba5cfe775a2baa1efaaf886aab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200964"
---
# <a name="sqlsetdescfield-function"></a>SQLSetDescField 函数

**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLSetDescField** 设置描述符记录的单个字段的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
SQLRETURN SQLSetDescField(  
     SQLHDESC      DescriptorHandle,  
     SQLSMALLINT   RecNumber,  
     SQLSMALLINT   FieldIdentifier,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    BufferLength);  
```  
  
## <a name="arguments"></a>参数  
 *DescriptorHandle*  
 送描述符句柄。  
  
 *RecNumber*  
 送指示描述符记录，其中包含应用程序要设置的字段。 描述符记录从0开始编号，记录号0为书签记录。 对于标头字段，将忽略 *RecNumber* 参数。  
  
 *FieldIdentifier*  
 送指示要设置其值的描述符字段。 有关详细信息，请参阅 "注释" 部分中的 "*FieldIdentifier* 参数"。  
  
 *ValuePtr*  
 送指向包含说明符信息的缓冲区的指针或一个整数值。 数据类型取决于 *FieldIdentifier* 的值。 如果 *将 valueptr* 是一个整数值，则可能会将其视为8字节 (SQLLEN) 、4个字节 (SQLINTEGER) 或2个字节 (SQLSMALLINT) ，具体取决于 *FieldIdentifier* 参数的值。  
  
 *BufferLength*  
 送如果 *FieldIdentifier* 是一个 ODBC 定义的字段，而 *将 valueptr* 指向某个字符串或二进制缓冲区，则此参数的长度应为 **将 valueptr*。 对于字符串数据，此参数应包含字符串中的字节数。  
  
 如果 *FieldIdentifier* 是一个 ODBC 定义的字段，而 *将 valueptr* 是一个整数，则将忽略 *BufferLength* 。  
  
 如果 *FieldIdentifier* 是驱动程序定义的字段，应用程序会通过设置 *BufferLength* 参数来指示该字段的特性。 *BufferLength* 可具有以下值：  
  
-   如果 *将 valueptr* 是指向字符串的指针，则 *BufferLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *将 valueptr* 是一个指向二进制缓冲区的指针，则该应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *BufferLength* 中。 这会在 *BufferLength* 中置入负值。  
  
-   如果 *将 valueptr* 是一个指向字符串或二进制字符串以外的值的指针，则 *BufferLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *将 valueptr* 包含固定长度的值，则 *BufferLength* 为 SQL_IS_INTEGER、SQL_IS_UINTEGER、SQL_IS_SMALLINT 或 SQL_IS_USMALLINT （视情况而定）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetDescField** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DESC 和 *DescriptorHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLSetDescField** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|该驱动程序不支持在 *\* 将 valueptr* (中指定的值，如果 *将 valueptr* 是指针) 或 *将 valueptr* (中的值。如果 *将 valueptr* 是一个整数值) ，或 *\* 将 valueptr* 由于实现工作条件而无效，则驱动程序将替换相似的值。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07009|描述符索引无效|*FieldIdentifier* 参数是一个记录字段， *RecNumber* 参数为0， *DescriptorHandle* 参数称为 IPD 句柄。<br /><br /> *RecNumber* 参数小于0， *DESCRIPTORHANDLE* 参数引用 ARD 或 APD。<br /><br /> *RecNumber* 参数大于数据源可以支持的列或参数的最大数目，以及引用 APD 或 ARD 的 *DescriptorHandle* 参数。<br /><br />  (DM) *FieldIdentifier* 参数 SQL_DESC_COUNT，并且 *\* 将 valueptr* 参数小于0。<br /><br /> *RecNumber* 参数等于0， *DescriptorHandle* 参数称为隐式分配的 APD。  (显式分配的应用程序描述符不会出现此错误，因为在执行时间之前显式分配的应用程序描述符是 APD 还是 ARD。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22001|字符串数据，右截断|*FieldIdentifier* 参数 SQL_DESC_NAME， *BufferLength* 参数的值大于 SQL_MAX_IDENTIFIER_LEN。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) *DescriptorHandle* 已与一个 StatementHandle 相关联，该的异步执行函数 (未调用) 此函数，并且在调用此函数时仍在执行。<br /><br /> 为 *StatementHandle* 关联并返回 SQL_NEED_DATA (DM) **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 为与 *DescriptorHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLSetDescField** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为与 *DescriptorHandle* 关联的其中一个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY016|无法修改实现行描述符|*DescriptorHandle* 参数与 IRD 相关联，但 *FieldIdentifier* 参数不是 SQL_DESC_ARRAY_STATUS_PTR 或 SQL_DESC_ROWS_PROCESSED_PTR。|  
|HY021|描述符信息不一致|SQL_DESC_TYPE 和 SQL_DESC_DATETIME_INTERVAL_CODE 字段不构成有效的 ODBC SQL 类型或特定于驱动程序的有效 SQL 类型 (用于 Ipd) 或 Apd 或 ARDs (的有效 ODBC C 类型) 。<br /><br /> 一致性检查过程中检查的描述符信息不一致。  (参阅 **SQLSetDescRec** 中的 "一致性检查" ) |  
|HY090|字符串或缓冲区长度无效| (DM) *\* 将 valueptr* 是一个字符串，而 *BufferLength* 小于零但不等于 SQL_NTS。<br /><br />  (DM) 驱动程序是一个 ODBC 2.x *驱动程序* ，描述符为 ARD， *ColumnNumber* 参数设置为0，并且为参数 *BufferLength* 指定的值不等于4。|  
|HY091|描述符字段标识符无效|为 *FieldIdentifier* 参数指定的值不是 ODBC 定义的字段，并且不是实现定义的值。<br /><br /> *FieldIdentifier* 参数对于 *DescriptorHandle* 参数无效。<br /><br /> *FieldIdentifier* 参数是一个只读的、ODBC 定义的字段。|  
|HY092|无效的属性/选项标识符|*\* 将 valueptr* 中的值对于 *FieldIdentifier* 参数无效。<br /><br /> *FieldIdentifier* 参数 SQL_DESC_UNNAMED，*将 valueptr* 已 SQL_NAMED。|  
|HY105|无效的参数类型| (DM) 为 SQL_DESC_PARAMETER_TYPE 字段指定的值无效。  (有关详细信息，请参阅 **SQLBindParameter** 中的 "*InputOutputType* 参数" 一节。 ) |  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [ODBC 3.8 中的新增功能](../../../odbc/reference/what-s-new-in-odbc-3-8.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *DescriptorHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 应用程序可以调用 **SQLSetDescField** 来一次设置一个描述符字段。 一次调用 **SQLSetDescField** 时，会在单个描述符中设置单个字段。 如果可以设置该字段，则可以调用此函数来设置任何描述符类型中的任何字段。  (参见本节后面的表。 )   
  
> [!NOTE]  
>  如果对 **SQLSetDescField** 的调用失败，则由 *RecNumber* 参数标识的说明符记录的内容是不确定的。  
  
 可以调用其他函数来设置多个描述符字段，只调用一次函数。 **SQLSetDescRec** 函数将设置多个字段，这些字段会影响 (SQL_DESC_TYPE、SQL_DESC_DATETIME_INTERVAL_CODE、SQL_DESC_OCTET_LENGTH、SQL_DESC_PRECISION、SQL_DESC_SCALE、SQL_DESC_DATA_PTR、SQL_DESC_OCTET_LENGTH_PTR 和 SQL_DESC_INDICATOR_PTR 字段) 的数据类型和绑定到列或参数的缓冲区。 可以使用 **SQLBindCol** 或 **SQLBindParameter** 对列或参数的绑定进行完整规范。 这些函数使用一个函数调用设置一组特定的描述符字段。  
  
 通过将偏移量添加到绑定指针 (SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 或 SQL_DESC_OCTET_LENGTH_PTR) ，可以调用 **SQLSetDescField** 来更改绑定缓冲区。 这会在不调用 **SQLBindCol** 或 **SQLBindParameter** 的情况下更改绑定缓冲区，这允许应用程序更改 SQL_DESC_DATA_PTR，而不会更改其他字段，如 SQL_DESC_DATA_TYPE。  
  
 如果应用程序调用 **SQLSetDescField** 来设置 SQL_DESC_COUNT 或延迟字段 SQL_DESC_DATA_PTR、SQL_DESC_OCTET_LENGTH_PTR 或 SQL_DESC_INDICATOR_PTR 中的任何字段，则该记录将变为未绑定。  
  
 通过使用适当的 *FieldIdentifier* 调用 **SQLSetDescField** 来设置描述符标头字段。 许多标头字段也是语句特性，因此也可以通过调用 **SQLSetStmtAttr** 来设置它们。 这样，应用程序便可以设置描述符字段，而无需首先获取描述符句柄。 当调用 **SQLSetDescField** 来设置标头字段时，将忽略 *RecNumber* 参数。  
  
 *RecNumber* 为0，用于设置书签字段。  
  
> [!NOTE]  
>  应始终在调用 **SQLSetDescField** 以设置书签字段之前设置语句特性 SQL_ATTR_USE_BOOKMARKS。 虽然这不是必需的，但强烈建议这样做。  
  
## <a name="sequence-of-setting-descriptor-fields"></a>设置描述符字段的顺序  
 当通过调用 **SQLSetDescField** 设置描述符字段时，应用程序必须遵循特定的顺序：  
  
1.  应用程序必须先设置 SQL_DESC_TYPE、SQL_DESC_CONCISE_TYPE 或 SQL_DESC_DATETIME_INTERVAL_CODE 字段。  
  
2.  设置这些字段之一后，应用程序可以设置数据类型的属性，驱动程序将数据类型属性字段设置为数据类型的相应默认值。 自动默认类型属性字段可确保在应用程序指定了数据类型后，说明符始终可以使用。 如果应用程序显式设置数据类型属性，则它将覆盖默认属性。  
  
3.  在步骤1中列出的某个字段已设置并且已设置数据类型属性后，应用程序可以设置 SQL_DESC_DATA_PTR。 这会提示对描述符字段进行一致性检查。 如果在设置 SQL_DESC_DATA_PTR 字段之后，应用程序更改数据类型或属性，则驱动程序将 SQL_DESC_DATA_PTR 设置为 null 指针，解除绑定记录。 这会强制应用程序按顺序完成正确步骤，然后才能使用描述符记录。  
  
## <a name="initialization-of-descriptor-fields"></a>描述符字段的初始化  
 分配描述符后，可以将描述符中的字段初始化为默认值，不使用默认值进行初始化，也可以为描述符类型定义。 下表说明了每种类型的描述符的每个字段的初始化，其中 "D" 指示该字段使用默认值进行初始化，"ND" 指示该字段在未使用默认值的情况下进行了初始化。 如果显示数字，则字段的默认值为该数字。 这些表还指明了字段是读/写 (R/W) 还是只读 (R) 。  
  
 只有在准备或执行了语句并填充了 IRD 后，IRD 的字段才具有默认值，而不是在分配语句句柄或描述符后。 在填充 IRD 之前，任何获取 IRD 字段访问权限的尝试都将返回错误。  
  
 某些描述符字段是为一个或多个（但不是全部）描述符类型（ (ARDs 和 IRDs）和 Apd 和 Ipd) 定义的。 如果对某一类型的描述符未定义某个字段，则任何使用该说明符的函数都不需要该字段。  
  
 **SQLGetDescField** 可以访问的字段不一定由 **SQLSetDescField** 设置。 下表列出了可以由 **SQLSetDescField** 设置的字段。  
  
 标头字段的初始化在下表中进行了介绍。  
  
|标头字段名称|类型|R/W|默认|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_ALLOC_TYPE|SQLSMALLINT|ARD： R APD： R IRD： R IPD： R|ARD：显式的隐式或 SQL_DESC_ALLOC_USER SQL_DESC_ALLOC_AUTO<br /><br /> APD：显式的隐式或 SQL_DESC_ALLOC_USER SQL_DESC_ALLOC_AUTO<br /><br /> IRD： SQL_DESC_ALLOC_AUTO<br /><br /> IPD： SQL_DESC_ALLOC_AUTO|  
|SQL_DESC_ARRAY_SIZE|SQLULEN 生成|ARD： R/W APD： R/W IRD：未使用的 IPD：未使用|ARD： [1] APD： [1] IRD：未使用的 IPD：未使用|  
|SQL_DESC_ARRAY_STATUS_PTR|SQLUSMALLINT*|ARD： R/W APD： R/W IRD： R/W IPD： R/W|ARD： Null ptr APD： Null ptr IRD： Null ptr IPD： Null ptr|  
|SQL_DESC_BIND_OFFSET_PTR|SQLLEN|ARD： R/W APD： R/W IRD：未使用的 IPD：未使用|ARD： Null ptr APD： Null ptr IRD：未使用的 IPD：未使用|  
|SQL_DESC_BIND_TYPE|SQLINTEGER|ARD： R/W APD： R/W IRD：未使用的 IPD：未使用|ARD： SQL_BIND_BY_COLUMN<br /><br /> APD： SQL_BIND_BY_COLUMN<br /><br /> IRD：未使用<br /><br /> IPD：未使用|  
|SQL_DESC_COUNT|SQLSMALLINT|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： 0 APD： 0 IRD： D IPD：0|  
|SQL_DESC_ROWS_PROCESSED_PTR|SQLULEN 生成|ARD：未使用的 APD：未使用的 IRD： R/W IPD： R/W|ARD：未使用的 APD：未使用的 IRD： Null ptr IPD： Null ptr|  
  
 [1] 仅当驱动程序自动填充 IPD 时才定义这些字段。 如果不是，则未定义。 如果应用程序尝试设置这些字段，SQLSTATE HY091 (将返回无效的描述符字段标识符) 。  
  
 记录字段的初始化如下表中所示。  
  
|记录字段名称|类型|R/W|默认|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQLINTEGER|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_BASE_COLUMN_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_BASE_TABLE_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_CASE_SENSITIVE|SQLINTEGER|ARD：未使用的 APD：未使用的 IRD： R IPD： R|ARD：未使用的 APD：未使用的 IRD： D IPD： D [1]|  
|SQL_DESC_CATALOG_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_CONCISE_TYPE|SQLSMALLINT|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： SQL_C_ 默认 APD： SQL_C_ 默认 IRD： D IPD： ND|  
|SQL_DESC_DATA_PTR|SQLPOINTER|ARD： R/W APD： R/W IRD：未使用的 IPD：未使用|ARD： Null ptr APD： Null ptr IRD：未使用的 IPD：未使用 [2]|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQLSMALLINT|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|SQLINTEGER|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_DISPLAY_SIZE|SQLLEN|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_FIXED_PREC_SCALE|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD： R IPD： R|ARD：未使用的 APD：未使用的 IRD： D IPD： D [1]|  
|SQL_DESC_INDICATOR_PTR|SQLLEN|ARD： R/W APD： R/W IRD：未使用的 IPD：未使用|ARD： Null ptr APD： Null ptr IRD：未使用的 IPD：未使用|  
|SQL_DESC_LABEL|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_LENGTH|SQLULEN 生成|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_LITERAL_PREFIX|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_LITERAL_SUFFIX|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_LOCAL_TYPE_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD： R|ARD：未使用的 APD：未使用的 IRD： D IPD： D [1]|  
|SQL_DESC_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_NULLABLE|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD： R IPD： R|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_NUM_PREC_RADIX|SQLINTEGER|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_OCTET_LENGTH|SQLLEN|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_OCTET_LENGTH_PTR|SQLLEN|ARD： R/W APD： R/W IRD：未使用的 IPD：未使用|ARD： Null ptr APD： Null ptr IRD：未使用的 IPD：未使用|  
|SQL_DESC_PARAMETER_TYPE|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD：未使用的 IPD： R/W|ARD：未使用的 APD：未使用的 IRD：未使用的 IPD： D = SQL_PARAM_INPUT|  
|SQL_DESC_PRECISION|SQLSMALLINT|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_ROWVER|SQLSMALLINT|ARD：未使用<br /><br /> APD：未使用<br /><br /> IRD： R<br /><br /> IPD： R|ARD：未使用<br /><br /> APD：未使用<br /><br /> IRD： ND<br /><br /> IPD： ND|  
|SQL_DESC_SCALE|SQLSMALLINT|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_SCHEMA_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_SEARCHABLE|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_TABLE_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
|SQL_DESC_TYPE|SQLSMALLINT|ARD： R/W APD： R/W IRD： R IPD： R/W|ARD： SQL_C_DEFAULT APD： SQL_C_DEFAULT IRD： D IPD： ND|  
|SQL_DESC_TYPE_NAME|SQLCHAR|ARD：未使用的 APD：未使用的 IRD： R IPD： R|ARD：未使用的 APD：未使用的 IRD： D IPD： D [1]|  
|SQL_DESC_UNNAMED|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD： R IPD： R/W|ARD： ND APD： ND IRD： D IPD： ND|  
|SQL_DESC_UNSIGNED|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD： R IPD： R|ARD：未使用的 APD：未使用的 IRD： D IPD： D [1]|  
|SQL_DESC_UPDATABLE|SQLSMALLINT|ARD：未使用的 APD：未使用的 IRD： R IPD：未使用|ARD：未使用的 APD：未使用的 IRD： D IPD：未使用|  
  
 [1] 仅当驱动程序自动填充 IPD 时才定义这些字段。 如果不是，则未定义。 如果应用程序尝试设置这些字段，SQLSTATE HY091 (将返回无效的描述符字段标识符) 。  
  
 [2] 可以设置 IPD 中的 SQL_DESC_DATA_PTR 字段来强制执行一致性检查。 在对 **SQLGetDescField** 或 **SQLGetDescRec** 的后续调用中，驱动程序无需返回 SQL_DESC_DATA_PTR 设置为的值。  
  
## <a name="fieldidentifier-argument"></a>FieldIdentifier 参数  
 *FieldIdentifier* 参数指示要设置的描述符字段。 描述符包含 *描述符标头，* 其中包括下一节 "标头字段" 中所述的标头字段，以及零个或多个 *说明符记录，* 由 "标头字段" 一节中所述的记录字段组成。  
  
## <a name="header-fields"></a>标头字段  
 每个描述符都有一个标头，其中包含以下字段：  
  
 **SQL_DESC_ALLOC_TYPE [All]**  
 此只读 SQLSMALLINT 标头字段指定是由驱动程序自动分配描述符，还是由应用程序显式分配。 应用程序可以获取此字段，但不能修改它。 如果驱动程序自动分配了描述符，该字段将设置为驱动程序 SQL_DESC_ALLOC_AUTO。 如果该描述符由应用程序显式分配，则将其设置为由驱动程序 SQL_DESC_ALLOC_USER。  
  
 **SQL_DESC_ARRAY_SIZE [应用程序描述符]**  
 在 ARDs 中，此 SQLULEN 生成标头字段指定行集中的行数。 这是通过调用 **SQLFetch** 或 **SQLFetchScroll** 或通过调用 **SQLBulkOperations** 或 **SQLSetPos** 来运行的行数。  
  
 在 Apd 中，此 SQLULEN 生成标头字段指定每个参数的值的数目。  
  
 此字段的默认值为1。 如果 SQL_DESC_ARRAY_SIZE 大于1，则 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 的 APD 或 ARD 点到数组。 每个数组的基数等于此字段的值。  
  
 还可以通过使用 SQL_ATTR_ROW_ARRAY_SIZE 特性调用 **SQLSetStmtAttr** 来设置 ARD 中的此字段。 还可以通过使用 SQL_ATTR_PARAMSET_SIZE 特性调用 **SQLSetStmtAttr** 来设置 APD 中的此字段。  
  
 **SQL_DESC_ARRAY_STATUS_PTR [All]**  
 对于每个描述符类型，此 SQLUSMALLINT * 标头字段指向 SQLUSMALLINT 值的数组。 这些数组的命名方式如下：行状态数组 (IRD) ，参数状态数组 (IPD) ，行操作数组 (ARD) ，以及参数操作数组 (APD) 。  
  
 在 IRD 中，此标头字段指向在调用 **SQLBulkOperations**、 **SQLFetch**、 **SQLFetchScroll** 或 **SQLSetPos** 后包含状态值的行状态数组。 数组中的元素数目与行集中的行数相同。 应用程序必须分配一个 SQLUSMALLINTs 数组，并将此字段设置为指向该数组。 默认情况下，该字段设置为 null 指针。 该驱动程序将填充 array-除非 SQL_DESC_ARRAY_STATUS_PTR 字段设置为 null 指针，在这种情况下，将不会生成状态值，并且不会填充数组。  
  
> [!CAUTION]  
>  如果应用程序通过 IRD 的 SQL_DESC_ARRAY_STATUS_PTR 字段设置的行状态数组的元素，则不确定驱动程序的行为。  
  
 首先，通过调用 **SQLBulkOperations**、 **SQLFetch**、 **SQLFetchScroll** 或 **SQLSetPos** 来填充数组。 如果调用未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则此字段指向的数组的内容是不确定的。 数组中的元素可以包含以下值：  
  
-   SQL_ROW_SUCCESS：行已成功提取，自从上次提取后未发生更改。  
  
-   SQL_ROW_SUCCESS_WITH_INFO：行已成功提取，自从上次提取后未发生更改。 但对于该行，返回了警告。  
  
-   SQL_ROW_ERROR：提取行时出错。  
  
-   SQL_ROW_UPDATED：行已成功提取，自从上次提取后已被更新。 如果再次提取该行，则其状态将 SQL_ROW_SUCCESS。  
  
-   SQL_ROW_DELETED：自从上次提取行后已将其删除。  
  
-   SQL_ROW_ADDED：行是由 **SQLBulkOperations** 插入的。 如果再次提取该行，则其状态将 SQL_ROW_SUCCESS。  
  
-   SQL_ROW_NOROW：行集与结果集的末尾重叠，并且未返回对应到此行状态数组的此元素的行。  
  
 还可以通过使用 SQL_ATTR_ROW_STATUS_PTR 特性调用 **SQLSetStmtAttr** 来设置 IRD 中的此字段。  
  
 仅在返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO 之后，IRD 的 "SQL_DESC_ARRAY_STATUS_PTR" 字段才有效。 如果返回代码不是其中的一个，则 SQL_DESC_ROWS_PROCESSED_PTR 指向的位置是不确定的。  
  
 在 IPD 中，此标头字段指向参数状态数组，其中包含对 **SQLExecute** 或 **SQLExecDirect** 的调用后每组参数值的状态信息。 如果对 **SQLExecute** 或 **SQLExecDirect** 的调用未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则此字段指向的数组的内容是不确定的。 应用程序必须分配一个 SQLUSMALLINTs 数组，并将此字段设置为指向该数组。 该驱动程序将填充 array-除非 SQL_DESC_ARRAY_STATUS_PTR 字段设置为 null 指针，在这种情况下，将不会生成状态值，并且不会填充数组。 数组中的元素可以包含以下值：  
  
-   SQL_PARAM_SUCCESS：已成功为此参数集执行 SQL 语句。  
  
-   SQL_PARAM_SUCCESS_WITH_INFO：已成功为此参数集执行 SQL 语句;但是，诊断数据结构中提供了警告信息。  
  
-   SQL_PARAM_ERROR：处理此参数集时出错。 诊断数据结构中提供了其他错误信息。  
  
-   SQL_PARAM_UNUSED：未使用此参数集，这可能是由于某些先前的参数集导致了中止进一步处理的错误，或者因为在 APD 的 SQL_DESC_ARRAY_STATUS_PTR 字段指定的数组中为该参数集设置了 SQL_PARAM_IGNORE。  
  
-   SQL_PARAM_DIAG_UNAVAILABLE：未提供诊断信息。 例如，驱动程序将参数数组视为单一单元，因此不会生成此级别的错误信息。  
  
 还可以通过使用 SQL_ATTR_PARAM_STATUS_PTR 特性调用 **SQLSetStmtAttr** 来设置 IPD 中的该字段。  
  
 在 ARD 中，此标头字段指向值的行操作数组，应用程序可以设置这些值，以指示对于 **SQLSetPos** 操作是否忽略此行。 数组中的元素可以包含以下值：  
  
-   SQL_ROW_PROCEED：使用 **SQLSetPos** 在大容量操作中包含行。  (此设置不能保证对行进行操作。 如果行在 IRD 行状态数组中具有状态 SQL_ROW_ERROR，驱动程序可能无法在行中执行该操作。 )   
  
-   SQL_ROW_IGNORE：使用 **SQLSetPos** 从大容量操作中排除该行。  
  
 如果未设置数组的元素，则在大容量操作中包括所有行。 如果 ARD 的 "SQL_DESC_ARRAY_STATUS_PTR" 字段中的值为 null 指针，则大容量操作中包含所有行;解释与指向有效数组的指针和 SQL_ROW_PROCEED 数组的所有元素都是相同的。 如果数组中的元素设置为 SQL_ROW_IGNORE，则不会更改被忽略行的行状态数组中的值。  
  
 还可以通过使用 SQL_ATTR_ROW_OPERATION_PTR 特性调用 **SQLSetStmtAttr** 来设置 ARD 中的此字段。  
  
 在 APD 中，此标头字段指向值的参数操作数组，应用程序可以设置这些值，以指示当调用 **SQLExecute** 或 **SQLExecDirect** 时，是否忽略此参数集。 数组中的元素可以包含以下值：  
  
-   SQL_PARAM_PROCEED： **SQLExecute** 或 **SQLExecDirect** 调用中包含参数集。  
  
-   SQL_PARAM_IGNORE：从 **SQLExecute** 或 **SQLExecDirect** 调用中排除了参数集。  
  
 如果未设置数组的元素，则将在 **SQLExecute** 或 **SQLExecDirect** 调用中使用数组中的所有参数集。 如果 APD 的 "SQL_DESC_ARRAY_STATUS_PTR" 字段中的值为 null 指针，则将使用所有参数集;解释与指向有效数组的指针和 SQL_PARAM_PROCEED 数组的所有元素都是相同的。  
  
 还可以通过使用 SQL_ATTR_PARAM_OPERATION_PTR 特性调用 **SQLSetStmtAttr** 来设置 APD 中的此字段。  
  
 **SQL_DESC_BIND_OFFSET_PTR [应用程序描述符]**  
 此 SQLLEN * 标头字段指向绑定偏移量。 默认情况下，它设置为 null 指针。 如果此字段不是 null 指针，驱动程序将取消引用指针，并将取消引用的值添加到描述符记录中具有非 null 值的每个延迟字段 (在提取时 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR) ，并在绑定时使用新的指针值。  
  
 绑定偏移量始终直接添加到 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 字段的值。 如果偏移量更改为不同的值，则新值仍会直接添加到每个描述符字段中的值。 新偏移量不会添加到字段值和任何以前的偏移量。  
  
 此字段是 *延迟的字段*：设置该字段时，它不会使用，但在以后需要确定数据缓冲区的地址时，驱动程序将使用该字段。  
  
 还可以通过使用 SQL_ATTR_ROW_BIND_OFFSET_PTR 特性调用 **SQLSetStmtAttr** 来设置 ARD 中的此字段。 还可以通过使用 SQL_ATTR_PARAM_BIND_OFFSET_PTR 特性调用 **SQLSetStmtAttr** 来设置 ARD 中的此字段。  
  
 有关详细信息，请参阅 [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md) 和 [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)中的按行绑定的描述。  
  
 **SQL_DESC_BIND_TYPE [应用程序描述符]**  
 此 SQLUINTEGER 标头字段设置要用于绑定列或参数的绑定方向。  
  
 在 ARDs 中，此字段指定在关联语句句柄上调用 **SQLFetchScroll** 或 **SQLFetch** 时的绑定方向。  
  
 若要为列选择按列绑定，此字段设置为默认)  (SQL_BIND_BY_COLUMN。  
  
 还可以通过使用 SQL_ATTR_ROW_BIND_TYPE *特性* 调用 **SQLSETSTMTATTR** 来设置 ARD 中的此字段。  
  
 在 Apd 中，此字段指定要用于动态参数的绑定方向。  
  
 若要为参数选择按列绑定，此字段设置为 SQL_BIND_BY_COLUMN (默认) 。  
  
 还可以通过使用 SQL_ATTR_PARAM_BIND_TYPE *特性* 调用 **SQLSETSTMTATTR** 来设置 APD 中的此字段。  
  
 **SQL_DESC_COUNT [All]**  
 此 SQLSMALLINT 标头字段指定包含数据的编号最高的记录的从1开始的索引。 当驱动程序设置描述符的数据结构时，还必须设置 "SQL_DESC_COUNT" 字段，以显示有多少记录是有意义的。 当应用程序分配此数据结构的实例时，无需指定为保留空间的记录数。 当应用程序指定记录的内容时，驱动程序将执行任何所需的操作，以确保描述符句柄引用适当大小的数据结构。  
  
 SQL_DESC_COUNT 不是绑定 (的所有数据列的计数。如果该字段位于 ARD) ，则为所有绑定的参数; 如果该字段在 APD) 中，则不是所有 (绑定的参数，而是编号最大的记录数。 如果编号最大的列或参数是未绑定的，则 SQL_DESC_COUNT 更改为下一编号最大的列或参数的编号。 如果通过调用 **SQLBindCol** 并将 *TargetValuePtr* 参数设置为 null 指针，或将 ParameterValuePtr 参数设置为 Null 指针) 的 **SQLBindParameter** （ 参数设置为 null 指针，则不会更改包含小于最大编号列数的列或参数 SQL_DESC_COUNT） (。 如果附加的列或参数的数字大于包含数据的最高编号记录的数字，则驱动程序将自动增加 "SQL_DESC_COUNT" 字段中的值。 如果通过使用 SQL_UNBIND 选项调用 **SQLFreeStmt** 来解除所有列的绑定，则 ARD 和 IRD 中的 SQL_DESC_COUNT 字段将设置为0。 如果用 SQL_RESET_PARAMS 选项调用 **SQLFreeStmt** ，则 APD 和 IPD 中的 SQL_DESC_COUNT 字段将设置为0。  
  
 SQL_DESC_COUNT 中的值可以通过调用 **SQLSetDescField** 由应用程序显式设置。 如果 SQL_DESC_COUNT 中的值显式减少，则会有效地删除所有数字大于 SQL_DESC_COUNT 中的新值的记录。 如果 SQL_DESC_COUNT 中的值显式设置为0，并且字段在 ARD 中，则将释放除绑定书签列之外的所有数据缓冲区。  
  
 ARD 的此字段中的记录计数不包括绑定书签列。 取消绑定书签列的唯一方法是将 SQL_DESC_DATA_PTR 字段设置为 null 指针。  
  
 **SQL_DESC_ROWS_PROCESSED_PTR [实现描述符]**  
 在 IRD 中，此 SQLULEN 生成 \* 标头字段指向一个缓冲区，该缓冲区包含调用 **SQLFetch** 或 **SQLFetchScroll** 后提取的行数，或者通过调用 **SQLBulkOperations** 或 **SQLSetPos**（包括错误行）所执行的大容量操作所影响的行数。  
  
 在 IPD，此 SQLUINTEGER * 标头字段指向一个缓冲区，该缓冲区包含已处理的参数集的数目，包括错误集。 如果为 null 指针，则不返回任何数字。  
  
 仅在对 IRD 字段调用 **SQLFetch** 或 **SQLFetchScroll** (后返回了 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO 后，SQL_DESC_ROWS_PROCESSED_PTR 才有效，) 或 IPD 字段 (的 **SQLExecute**、 **SQLExecDirect** 或 **SQLParamData**) 返回了该字段。 如果在此字段指向的缓冲区中填充的调用未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则除非返回 SQL_NO_DATA，否则，将缓冲区中的值设置为0。  
  
 还可以通过使用 SQL_ATTR_ROWS_FETCHED_PTR 特性调用 **SQLSetStmtAttr** 来设置 ARD 中的此字段。 还可以通过使用 SQL_ATTR_PARAMS_PROCESSED_PTR 特性调用 **SQLSetStmtAttr** 来设置 APD 中的此字段。  
  
 此字段指向的缓冲区由应用程序分配。 它是由驱动程序设置的延迟输出缓冲区。 默认情况下，它设置为 null 指针。  
  
## <a name="record-fields"></a>记录字段  
 每个描述符都包含一个或多个记录，这些记录包含定义列数据或动态参数的字段，具体取决于描述符的类型。 每个记录都是单个列或参数的完整定义。  
  
 **SQL_DESC_AUTO_UNIQUE_VALUE [IRDs]**  
 此只读 SQLINTEGER 记录字段包含 SQL_TRUE 如果该列是自动递增列，则为; 如果该列不是自动递增列，则为 SQL_FALSE。 此字段是只读的，但基础自动递增列不一定是只读的。  
  
 **SQL_DESC_BASE_COLUMN_NAME [IRDs]**  
 此只读 SQLCHAR * 记录字段包含结果集列的基本列名称。 如果基列名称不存在 (与) 表达式的列的情况相同），则此变量包含空字符串。  
  
 **SQL_DESC_BASE_TABLE_NAME [IRDs]**  
 此只读 SQLCHAR * 记录字段包含结果集列的基表名称。 如果无法定义或不适用基表名称，则此变量包含空字符串。  
  
 **SQL_DESC_CASE_SENSITIVE [实现描述符]**  
 如果列或参数被视为区分大小写和比较区分大小写，SQL_FALSE 则此只读 SQLINTEGER 记录字段包含 SQL_TRUE; 如果列不被视为区分大小写，则为非字符列。  
  
 **SQL_DESC_CATALOG_NAME [IRDs]**  
 此只读 SQLCHAR * 记录字段包含包含该列的基表的目录。 如果列是表达式，则返回值与驱动程序相关; 如果该列是视图的一部分，则返回值。 如果数据源不支持目录或无法确定目录，则此变量包含空字符串。  
  
 **SQL_DESC_CONCISE_TYPE [All]**  
 此 SQLSMALLINT 标头字段指定所有数据类型（包括 datetime 和 interval 数据类型）的简洁数据类型。  
  
 "SQL_DESC_CONCISE_TYPE"、"SQL_DESC_TYPE" 和 "SQL_DESC_DATETIME_INTERVAL_CODE" 字段中的值是相互依赖的。 每次设置其中一个字段时，还必须设置另一个字段。 可以通过调用 **SQLBindCol** 或 **SQLBindParameter** 或 **SQLSetDescField** 设置 SQL_DESC_CONCISE_TYPE。 可以通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 来设置 SQL_DESC_TYPE。  
  
 如果将 SQL_DESC_CONCISE_TYPE 设置为一个非间隔或日期时间数据类型的简洁数据类型，则 SQL_DESC_TYPE 字段将设置为相同的值，SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为0。  
  
 如果将 SQL_DESC_CONCISE_TYPE 设置为简洁日期时间或间隔数据类型，则 SQL_DESC_TYPE 字段将设置为相应的详细类型 (SQL_DATETIME 或 SQL_INTERVAL) 并将 SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为相应的子代码。  
  
 **SQL_DESC_DATA_PTR [应用程序描述符和 Ipd]**  
 此 SQLPOINTER 记录字段指向一个变量，该变量将包含 Apd) 的参数值 (或 ARDs) 的列值 (。 此字段是 *延迟字段*。 它在设置时不会使用，但会在以后由驱动程序用来检索数据。  
  
 如果对 **SQLBindCol** 的调用中的 *TargetValuePtr* 参数为 null 指针，或者如果 ARD 中的 SQL_DESC_DATA_PTR 字段通过调用 **SQLSetDescField** 或 **SQLSETDESCREC** 设置为 null 指针，则 ARD 的 "SQL_DESC_DATA_PTR" 字段指定的列未绑定。 如果 SQL_DESC_DATA_PTR 字段设置为 null 指针，则不会影响其他字段。  
  
 如果对 **SQLFetch** 或 **SQLFetchScroll** 的调用填充此字段指向的缓冲区未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则缓冲区的内容未定义。  
  
 只要设置了 APD、ARD 或 IPD 的 SQL_DESC_DATA_PTR 字段，驱动程序就会检查 SQL_DESC_TYPE 字段中的值是否包含有效的 ODBC C 数据类型或驱动程序特定的数据类型之一，以及影响数据类型的所有其他字段是否一致。 提示一致性检查是唯一使用 IPD 的 SQL_DESC_DATA_PTR 字段。 具体来说，如果应用程序设置了 IPD 和更高版本的 SQL_DESC_DATA_PTR 字段，则不一定会返回该字段已设置的值。 有关详细信息，请参阅 [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)中的 "一致性检查"。  
  
 **SQL_DESC_DATETIME_INTERVAL_CODE [All]**  
 当 SQL_DATETIME 或 SQL_INTERVAL SQL_DESC_TYPE 字段时，此 SQLSMALLINT 记录字段包含特定 datetime 或 interval 数据类型的子代码。 这对于 SQL 和 C 数据类型都是如此。 此代码由数据类型名称（其 "TYPE" 或 "C_TYPE"）替换为日期时间类型) 的 "TYPE" 或 "" (，或将 "CODE" 替换为间隔类型 (的 "INTERVAL" 或 "C_INTERVAL") 。  
  
 如果应用程序描述符中的 SQL_DESC_TYPE 和 SQL_DESC_CONCISE_TYPE 设置为 SQL_C_DEFAULT，并且描述符不与语句句柄相关联，则 SQL_DESC_DATETIME_INTERVAL_CODE 的内容不确定。  
  
 对于下表中列出的日期时间数据类型，可以设置此字段。  
  
|日期/时间类型|DATETIME_INTERVAL_CODE|  
|--------------------|------------------------------|  
|SQL_TYPE_DATE/SQL_C_TYPE_DATE|SQL_CODE_DATE|  
|SQL_TYPE_TIME/SQL_C_TYPE_TIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP/SQL_C_TYPE_TIMESTAMP|SQL_CODE_TIMESTAMP|  
  
 对于下表中列出的间隔数据类型，可以设置此字段。  
  
|间隔类型|DATETIME_INTERVAL_CODE|  
|-------------------|------------------------------|  
|SQL_INTERVAL_DAY/SQL_C_INTERVAL_DAY|SQL_CODE_DAY|  
|SQL_INTERVAL_DAY_TO_HOUR/SQL_C_INTERVAL_DAY_TO_HOUR|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE/SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_CODE_DAY_TO_MINUTE|  
|SQL_INTERVAL_DAY_TO_SECOND/SQL_C_INTERVAL_DAY_TO_SECOND|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR/SQL_C_INTERVAL_HOUR|SQL_CODE_HOUR|  
|SQL_INTERVAL_HOUR_TO_MINUTE/SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND/SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE/SQL_C_INTERVAL_MINUTE|SQL_CODE_MINUTE|  
|SQL_INTERVAL_MINUTE_TO_SECOND/SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_CODE_MINUTE_TO_SECOND|  
|SQL_INTERVAL_MONTH/SQL_C_INTERVAL_MONTH|SQL_CODE_MONTH|  
|SQL_INTERVAL_SECOND/SQL_C_INTERVAL_SECOND|SQL_CODE_SECOND|  
|SQL_INTERVAL_YEAR/SQL_C_INTERVAL_YEAR|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH/SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_CODE_YEAR_TO_MONTH|  
  
 有关数据间隔和此字段的详细信息，请参阅 [数据类型标识符和描述符](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)。  
  
 **SQL_DESC_DATETIME_INTERVAL_PRECISION [All]**  
 如果 SQL_INTERVAL SQL_DESC_TYPE 字段，则此 SQLINTEGER 记录字段包含间隔前导精度。 当 SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为 INTERVAL 数据类型时，此字段将设置为默认间隔的默认值。  
  
 **SQL_DESC_DISPLAY_SIZE [IRDs]**  
 此只读 SQLINTEGER 记录字段包含显示列中的数据所需的最大字符数。  
  
 **SQL_DESC_FIXED_PREC_SCALE [实现描述符]**  
 如果该列是精确数值列并且具有固定精度和非零小数位数，则此只读 SQLSMALLINT 记录字段设置为 SQL_TRUE; 如果该列不是具有固定精度和小数位数的精确数值列，则为 SQL_FALSE。  
  
 **SQL_DESC_INDICATOR_PTR [应用程序描述符]**  
 在 ARDs 中，此 SQLLEN * 记录字段指向指示器变量。 如果列值为 NULL，则此变量包含 SQL_NULL_DATA。 对于 Apd，指示器变量设置为 SQL_NULL_DATA 以指定 NULL 动态参数。 否则，除非 SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR 中的值与) 相同的指针，否则变量为零 (。  
  
 如果 ARD 中的 SQL_DESC_INDICATOR_PTR 字段为空指针，则将阻止该驱动程序返回有关该列是否为 NULL 的信息。 如果该列为 NULL 并且 SQL_DESC_INDICATOR_PTR 为 null 指针，则在调用 **SQLFetch** 或 **SQLFetchScroll** 后，当驱动程序尝试填充缓冲区时，将返回所需的 SQLSTATE 22002 (指示器变量，但未提供) 。 如果对 **SQLFetch** 或 **SQLFetchScroll** 的调用未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则缓冲区的内容不确定。  
  
 "SQL_DESC_INDICATOR_PTR" 字段确定是否设置了 SQL_DESC_OCTET_LENGTH_PTR 指向的字段。 如果列的数据值为 NULL，则驱动程序将指示器变量设置为 SQL_NULL_DATA。 然后，将不设置由 SQL_DESC_OCTET_LENGTH_PTR 指向的字段。 如果在提取过程中没有遇到 NULL 值，则 SQL_DESC_INDICATOR_PTR 所指向的缓冲区设置为零，由 SQL_DESC_OCTET_LENGTH_PTR 指向的缓冲区设置为数据的长度。  
  
 如果 APD 中的 SQL_DESC_INDICATOR_PTR 字段为 null 指针，则应用程序不能使用此描述符记录来指定 NULL 参数。  
  
 此字段是 *延迟字段*：设置该字段时，它不会使用它，而是由驱动程序稍后用来指示 ARDs) 的为空性 (或确定 apd) 的为空性 (。  
  
 **SQL_DESC_LABEL [IRDs]**  
 此只读 SQLCHAR * 记录字段包含列标签或标题。 如果该列没有标签，则此变量包含列名称。 如果该列未命名且未标记，则此变量包含空字符串。  
  
 **SQL_DESC_LENGTH [All]**  
 此 SQLULEN 生成记录字段是字符的最大长度或实际长度，或以字节为单位的二进制数据类型。 它是固定长度的数据类型的最大长度，或可变长度数据类型的实际长度。 它的值始终排除以字符串结尾的 null 终止字符。 对于类型为 SQL_TYPE_DATE 的值、SQL_TYPE_TIME、SQL_TYPE_TIMESTAMP 或 SQL 时间间隔数据类型之一，此字段具有日期时间或间隔值的字符串表示形式的长度（字符）。  
  
 此字段中的值可能与 ODBC 2.x 中定义的 "length" 值 *不同。* 有关详细信息，请参阅 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。  
  
 **SQL_DESC_LITERAL_PREFIX [IRDs]**  
 此只读 SQLCHAR * 记录字段包含驱动程序将其识别为此数据类型的文本前缀的一个或多个字符。 对于不适用文本前缀的数据类型，此变量包含空字符串。  
  
 **SQL_DESC_LITERAL_SUFFIX [IRDs]**  
 此只读 SQLCHAR * 记录字段包含驱动程序可识别为此数据类型的文本后缀的一个或多个字符。 对于不适用文本后缀的数据类型，此变量包含空字符串。  
  
 **SQL_DESC_LOCAL_TYPE_NAME [实现描述符]**  
 此只读 SQLCHAR * 记录字段包含可能与数据类型的常规名称不同的数据类型的任何本地化 (本机语言) 名称。 如果没有本地化名称，则返回空字符串。 此字段仅用于显示目的。  
  
 **SQL_DESC_NAME [实现描述符]**  
 如果应用了列，则行描述符中的此 SQLCHAR * 记录字段将包含列别名。 如果列别名不适用，则返回列名称。 在任一情况下，驱动程序都会将 SQL_DESC_UNNAMED 字段设置为在设置 SQL_DESC_NAME 字段时 SQL_NAMED。 如果没有列名称或列别名，驱动程序将在 "SQL_DESC_NAME" 字段中返回空字符串，并将 SQL_DESC_UNNAMED 字段设置为 "SQL_UNNAMED"。  
  
 应用程序可将 IPD 的 "SQL_DESC_NAME" 字段设置为参数名称或别名，以按名称指定存储过程参数。  (有关详细信息，请参阅 [按名称绑定参数) 的命名参数 (](../../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md)。 ) IRD 的 SQL_DESC_NAME 字段为只读字段;SQLSTATE HY091 (无效的描述符字段标识符，如果应用程序尝试对其进行设置，将返回) 。  
  
 在 Ipd 中，如果驱动程序不支持命名参数，则此字段未定义。 如果驱动程序支持命名参数并且能够描述参数，则在此字段中返回参数名称。  
  
 **SQL_DESC_NULLABLE [实现描述符]**  
 在 IRDs 中，如果列可以具有 NULL 值，则此只读 SQLSMALLINT 记录字段 SQL_NULLABLE SQL_NO_NULLS，如果列没有 NULL 值，则为; 如果该列不知道列是否接受 NULL 值，则为 SQL_NULLABLE_UNKNOWN。 此字段与结果集列有关，而不是基列。  
  
 在 Ipd 中，此字段始终设置为 SQL_NULLABLE 因为动态参数始终可以为 null，并且不能由应用程序设置。  
  
 **SQL_DESC_NUM_PREC_RADIX [All]**  
 如果 SQL_DESC_TYPE 字段中的数据类型为近似数值数据类型，则此 SQLINTEGER 字段包含值2，因为 SQL_DESC_PRECISION 字段包含位数。 如果 SQL_DESC_TYPE 字段中的数据类型为精确数值数据类型，则此字段包含值10，因为 SQL_DESC_PRECISION 字段包含十进制数字的位数。 对于所有非数值数据类型，此字段都设置为0。  
  
 **SQL_DESC_OCTET_LENGTH [All]**  
 此 SQLLEN 记录字段包含字符串或二进制数据类型的长度（以字节为单位）。 对于固定长度的字符或二进制类型，这是实际的长度（以字节为单位）。 对于长度可变的字符或二进制类型，这是最大长度（以字节为单位）。 此值始终为实现描述符的 null 终止字符排除空间，并始终包含用于应用程序描述符的 null 终止字符的空间。 对于应用程序数据，此字段包含缓冲区的大小。 对于 Apd，此字段仅为输出参数或输入/输出参数定义。  
  
 **SQL_DESC_OCTET_LENGTH_PTR [应用程序描述符]**  
 此 SQLLEN * 记录字段指向一个变量，该变量将包含参数描述符的动态参数 (的总长度（以字节为单位）) 或 (行说明符) 的绑定列值。  
  
 对于 APD，对于除字符串和二进制字符之外的所有参数，此值将被忽略;如果此字段指向 SQL_NTS，则动态参数必须以 null 值终止。 若要指示绑定参数将是执行时数据参数，应用程序会将 APD 的适当记录中的此字段设置为一个变量，该变量在执行时将包含 SQL_LEN_DATA_AT_EXEC 宏的值 SQL_DATA_AT_EXEC 或结果。 如果有多个这样的字段，则可以将 SQL_DESC_DATA_PTR 设置为唯一标识参数的值，以帮助应用程序确定请求的参数。  
  
 如果 ARD 的 OCTET_LENGTH_PTR 字段为 null 指针，则驱动程序将不返回列的长度信息。 如果 APD 的 "SQL_DESC_OCTET_LENGTH_PTR" 字段为 null 指针，则驱动程序会假定字符字符串和二进制值以 null 结尾。  (二进制值不应以 null 结尾，但应为其指定长度以避免截断。 )   
  
 如果对 **SQLFetch** 或 **SQLFetchScroll** 的调用填充此字段指向的缓冲区未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则缓冲区的内容未定义。 此字段是 *延迟字段*。 它在设置时不会使用，但会在以后由驱动程序用来确定或指示数据的八进制长度。  
  
 **SQL_DESC_PARAMETER_TYPE [Ipd]**  
 此 SQLSMALLINT 记录字段设置为输入参数 SQL_PARAM_INPUT、输入/输出参数 SQL_PARAM_INPUT_OUTPUT、输出参数的 SQL_PARAM_OUTPUT、输入/输出流式处理参数 SQL_PARAM_INPUT_OUTPUT_STREAM 或输出流式处理参数 SQL_PARAM_OUTPUT_STREAM。 默认情况下，它设置为 SQL_PARAM_INPUT。  
  
 对于 IPD，如果驱动程序未自动填充 IPD，该字段将设置为 SQL_PARAM_INPUT 默认情况下， (SQL_ATTR_ENABLE_AUTO_IPD 语句特性 SQL_FALSE 为) 。 应用程序应在非输入参数参数的 IPD 中设置此字段。  
  
 **SQL_DESC_PRECISION [All]**  
 此 SQLSMALLINT 记录字段包含精确数值类型的位数、尾数中的位数 (二进制精度) 对于近似数值类型，或 SQL_TYPE_TIME、SQL_TYPE_TIMESTAMP 或 SQL_INTERVAL_SECOND 数据类型的秒小数部分中的数字个数。 对于所有其他数据类型，此字段未定义。  
  
 此字段中的值可能与 ODBC 2.x 中定义的 "precision" 值 *不同。* 有关详细信息，请参阅 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。  
  
 **SQL_DESC_ROWVER [实现描述符]**  
 此 SQLSMALLINTrecord 字段指示在更新行时 DBMS 是否自动修改列 (例如 SQL Server) 中类型为 "timestamp" 的列。 如果该列是行版本控制列 SQL_FALSE，则将此 "记录" 字段的值设置为 SQL_TRUE; 否则设置为。 此列属性类似于使用 SQL_ROWVER 的 IdentifierType 调用 **SQLSpecialColumns** ，以确定是否自动更新列。  
  
 **SQL_DESC_SCALE [All]**  
 此 SQLSMALLINT 记录字段包含 decimal 和 numeric 数据类型的已定义小数位数。 对于所有其他数据类型，该字段未定义。  
  
 此字段中的值可能与 ODBC 2.x 中定义的 "scale" 的值 *不同。* 有关详细信息，请参阅 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。  
  
 **SQL_DESC_SCHEMA_NAME [IRDs]**  
 此只读 SQLCHAR * 记录字段包含包含该列的基表的架构名称。 如果列是表达式，则返回值与驱动程序相关; 如果该列是视图的一部分，则返回值。 如果数据源不支持架构，或者无法确定架构名称，则此变量包含空字符串。  
  
 **SQL_DESC_SEARCHABLE [IRDs]**  
 此只读 SQLSMALLINT 记录字段设置为以下值之一：  
  
-   如果列不能用于 **WHERE** 子句，则 SQL_PRED_NONE。  (这与 ODBC 2.x 中的 SQL_UNSEARCHABLE 值相同 )   
  
-   如果列可以在 **WHERE** 子句中使用，则 SQL_PRED_CHAR （仅适用 **于 LIKE** 谓词）。  (这与 ODBC 2.x 中的 SQL_LIKE_ONLY 值相同 )   
  
-   如果列可用于包含所有比较运算符（**如** 除外）的 **WHERE** 子句中，则 SQL_PRED_BASIC。  (这与 ODBC 2.x 中的 SQL_EXCEPT_LIKE 值相同 )   
  
-   如果可以在 **WHERE** 子句中将列用于任何比较运算符，则 SQL_PRED_SEARCHABLE。  
  
 **SQL_DESC_TABLE_NAME [IRDs]**  
 此只读 SQLCHAR * 记录字段包含包含此列的基表的名称。 如果列是表达式，则返回值与驱动程序相关; 如果该列是视图的一部分，则返回值。  
  
 **SQL_DESC_TYPE [All]**  
 此 SQLSMALLINT 记录字段为除 datetime 和 interval 数据类型之外的所有数据类型指定简洁的 SQL 或 C 数据类型。 对于 datetime 和 interval 数据类型，此字段指定详细数据类型，该数据类型为 SQL_DATETIME 或 SQL_INTERVAL。  
  
 只要此字段包含 SQL_DATETIME 或 SQL_INTERVAL，SQL_DESC_DATETIME_INTERVAL_CODE 字段必须包含简洁类型的相应子代码。 对于日期时间数据类型，SQL_DESC_TYPE 包含 SQL_DATETIME，SQL_DESC_DATETIME_INTERVAL_CODE 字段包含特定 datetime 数据类型的子代码。 对于间隔数据类型，SQL_DESC_TYPE 包含 SQL_INTERVAL 并且 SQL_DESC_DATETIME_INTERVAL_CODE 字段包含特定 interval 数据类型的子代码。  
  
 "SQL_DESC_TYPE" 和 "SQL_DESC_CONCISE_TYPE" 字段中的值是相互依赖的。 每次设置其中一个字段时，还必须设置另一个字段。 可以通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 来设置 SQL_DESC_TYPE。 可以通过调用 **SQLBindCol** 或 **SQLBindParameter** 或 **SQLSetDescField** 设置 SQL_DESC_CONCISE_TYPE。  
  
 如果将 SQL_DESC_TYPE 设置为一个非间隔或日期时间数据类型的简洁数据类型，则 SQL_DESC_CONCISE_TYPE 字段将设置为相同的值，SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为0。  
  
 如果 SQL_DESC_TYPE 设置为详细日期时间或间隔数据类型 (SQL_DATETIME 或 SQL_INTERVAL) 并且 SQL_DESC_DATETIME_INTERVAL_CODE 字段设置为相应的子代码，则 SQL_DESC_CONCISE 类型字段将设置为相应的简明类型。 如果尝试将 SQL_DESC_TYPE 设置为简明日期时间或间隔类型之一，则将返回 SQLSTATE HY021 (不一致的描述符信息) 。  
  
 当通过调用 **SQLBindCol**、 **SQLBindParameter** 或 **SQLSetDescField** 设置 SQL_DESC_TYPE 字段时，以下字段将设置为以下默认值，如下表所示。 同一记录的其余字段的值是不确定的。  
  
|SQL_DESC_TYPE 的值|隐式设置的其他字段|  
|------------------------------|---------------------------------|  
|SQL_CHAR、SQL_VARCHAR、SQL_C_CHAR SQL_C_VARCHAR|SQL_DESC_LENGTH 设置为1。 SQL_DESC_PRECISION 设置为0。|  
|SQL_DATETIME|如果 SQL_DESC_DATETIME_INTERVAL_CODE 设置为 SQL_CODE_DATE 或 SQL_CODE_TIME，则 SQL_DESC_PRECISION 设置为0。 如果设置为 "SQL_DESC_TIMESTAMP"，SQL_DESC_PRECISION 设置为6。|  
|SQL_DECIMAL、SQL_NUMERIC SQL_C_NUMERIC|SQL_DESC_SCALE 设置为0。 SQL_DESC_PRECISION 设置为各自数据类型的实现定义的精度。<br /><br /> 有关如何手动绑定 SQL_C_NUMERIC 值的信息，请参阅 [SQL To C：数值](../../../odbc/reference/appendixes/sql-to-c-numeric.md) 。|  
|SQL_FLOAT，SQL_C_FLOAT|SQL_DESC_PRECISION 设置为 SQL_FLOAT 的实现定义的默认精度。|  
|SQL_INTERVAL|将 SQL_DESC_DATETIME_INTERVAL_CODE 设置为 INTERVAL 数据类型时，SQL_DESC_DATETIME_INTERVAL_PRECISION 设置为 2 (默认间隔前导精度) 。 当间隔具有秒部分时，SQL_DESC_PRECISION 设置为 6 (默认间隔秒精度) 。|  
  
 当应用程序调用 **SQLSetDescField** 来设置说明符的字段而不是调用 **SQLSetDescRec** 时，应用程序必须首先声明数据类型。 如果是这样，则会隐式设置上表中指出的其他字段。 如果隐式设置的任何值都是不可接受的，则应用程序可以调用 **SQLSetDescField** 或 **SQLSetDescRec** 来显式设置不可接受的值。  
  
 **SQL_DESC_TYPE_NAME [实现描述符]**  
 此只读 SQLCHAR * 记录字段包含与数据源相关的类型名称 (例如，"CHAR"、"VARCHAR" 等) 。 如果数据类型名称未知，则此变量包含空字符串。  
  
 **SQL_DESC_UNNAMED [实现描述符]**  
 行描述符中的此 SQLSMALLINT 记录字段由驱动程序设置为在设置 SQL_DESC_NAME 字段时 SQL_NAMED 或 SQL_UNNAMED。 如果 SQL_DESC_NAME 字段包含列别名或列别名不适用，则驱动程序将 SQL_DESC_UNNAMED 字段设置为 "SQL_NAMED"。 如果应用程序将 IPD 的 "SQL_DESC_NAME" 字段设置为参数名称或别名，则驱动程序将 IPD 的 SQL_DESC_UNNAMED 字段设置为 "SQL_NAMED"。 如果没有列名称或列别名，则驱动程序将 SQL_DESC_UNNAMED 字段设置为 SQL_UNNAMED。  
  
 应用程序可将 IPD 的 SQL_DESC_UNNAMED 字段设置为 SQL_UNNAMED。 如果应用程序尝试将 IPD 的 SQL_DESC_UNNAMED 字段设置为 SQL_NAMED，则驱动程序将返回 SQLSTATE HY091 (无效的描述符字段标识符) 。 IRD 的 SQL_DESC_UNNAMED 字段是只读的;SQLSTATE HY091 (无效的描述符字段标识符，如果应用程序尝试对其进行设置，将返回) 。  
  
 **SQL_DESC_UNSIGNED [实现描述符]**  
 如果列类型为无符号或非数字，则将此只读的 "SQLSMALLINT 记录" 字段设置为 SQL_TRUE; 如果列类型已签名，则设置为 SQL_FALSE。  
  
 **SQL_DESC_UPDATABLE [IRDs]**  
 此只读 SQLSMALLINT 记录字段设置为以下值之一：  
  
-   如果结果集列是只读的，则 SQL_ATTR_READ_ONLY。  
  
-   如果结果集列是可读写的，则 SQL_ATTR_WRITE。  
  
-   如果不知道结果集列是否可更新，则 SQL_ATTR_READWRITE_UNKNOWN。  
  
 SQL_DESC_UPDATABLE 描述结果集中的列的更新，而不是基表中的列。 此结果集列所基于的基表中的列的可更新性可能不同于此字段中的值。 列是否可更新可以基于数据类型、用户权限和结果集本身的定义。 如果某列是否可更新，则应返回 SQL_ATTR_READWRITE_UNKNOWN。  
  
## <a name="consistency-checks"></a>一致性检查  
 每当应用程序传入 ARD、APD 或 IPD 的 SQL_DESC_DATA_PTR 字段值时，驱动程序就会自动执行一致性检查。 如果任何字段与其他字段不一致，则 **SQLSetDescField** 将返回 SQLSTATE HY021 (不一致的描述符信息) 。 有关详细信息，请参阅 [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)中的 "一致性检查"。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|绑定列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|绑定参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|获取描述符字段|[SQLGetDescField 函数](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|获取多个描述符字段|[SQLGetDescRec 函数](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|设置多个描述符字段|[SQLSetDescRec 函数](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)
