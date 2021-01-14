---
description: SQLGetStmtAttr 函数
title: SQLGetStmtAttr 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetStmtAttr
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC]
ms.assetid: e321d460-e997-4527-aee6-207cf5a498e9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6af1813068b51780c9181f3e3e98559769495fe
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172369"
---
# <a name="sqlgetstmtattr-function"></a>SQLGetStmtAttr 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetStmtAttr** 返回语句特性的当前设置。  
  
> [!NOTE]  
>  有关在 ODBC 3 时驱动程序管理器将此函数映射到的内容的详细信息。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetStmtAttr(  
     SQLHSTMT        StatementHandle,  
     SQLINTEGER      Attribute,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *Attribute*  
 送要检索的属性。  
  
 *ValuePtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 *特性* 中指定的特性的值。  
  
 如果 *将 valueptr* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的总字节数) 可在 *将 valueptr* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送如果 *attribute* 是一个 ODBC 定义的属性，并且 *将 valueptr* 指向某个字符串或二进制缓冲区，则此参数应为 \* *将 valueptr* 的长度。 如果 *attribute* 是一个 ODBC 定义的属性，并且 \* *将 valueptr* 是一个整数，则将忽略 *BufferLength* 。 如果在调用 **SQLGetStmtAttrW**) 时， *\* 将 valueptr* 中返回的值为 Unicode 字符串 (，则 *BufferLength* 参数必须是偶数。  
  
 如果 *attribute* 是驱动程序定义的属性，则应用程序通过设置 *BufferLength* 参数来指示属性对驱动程序管理器的性质。 *BufferLength* 可具有以下值：  
  
-   如果 *\* 将 valueptr* 是指向字符串的指针，则 *BufferLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *\* 将 valueptr* 是一个指向二进制缓冲区的指针，则该应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *BufferLength* 中。 这会在 *BufferLength* 中置入负值。  
  
-   如果 *\* 将 valueptr* 是一个指向字符串或二进制字符串以外的值的指针，则 *BufferLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *\* 将 valueptr* 包含固定长度的数据类型，则 *BufferLength* 根据需要 SQL_IS_INTEGER 或 SQL_IS_UINTEGER。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (排除 null 终止字符) 可在 *\* 将 valueptr* 中返回的总字节数。 如果特性值为字符串，并且可返回的字节数大于或等于 *BufferLength*，则 *\* 将 valueptr* 中的数据将被截断为 *BufferLength* 减去 null 终止字符的长度，并以 null 值终止于驱动程序。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetStmtAttr** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetStmtAttr** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|*\* 将 valueptr* 中返回的数据被截断为 *BufferLength* 减去 null 终止字符的长度。 在 **StringLengthPtr* 中返回未截断字符串值的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|24000|无效的游标状态|自变量 *特性* 是 SQL_ATTR_ROW_NUMBER 的，游标未打开，或者游标位于结果集的开头之前或结果集结束之后。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 参数 *MessageText* 中 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLGetStmtAttr** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效|*(DM) \*将 valueptr* 是一个字符串，BufferLength 小于零，但不等于 SQL_NTS。|  
|HY092|无效的属性/选项标识符|为该驱动程序支持的 ODBC 版本指定 *的参数值无效。*|  
|HY109|游标位置无效|*特性* 参数已 SQL_ATTR_ROW_NUMBER，并且该行已被删除或无法提取。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|为 argument *特性* 指定的值为驱动程序所支持的 odbc 版本的有效 odbc 语句属性，但该驱动程序不支持该属性。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 相对应的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 有关语句属性的常规信息，请参阅 [语句特性](../../../odbc/reference/develop-app/statement-attributes.md)。  
  
 对 **SQLGetStmtAttr** 的调用在 *\* 将 valueptr* 中返回 *特性* 中指定的语句特性的值。 该值可以是 SQLULEN 生成值，也可以是以 null 值结束的字符串。 如果值是 SQLULEN 生成值，则某些驱动程序只能写入缓冲区中的较低32位或16位，并使高阶位保持不变。 因此，在调用此函数之前，应用程序应使用 SQLULEN 生成的缓冲区并将值初始化为0。 此外，不使用 *BufferLength* 和 *StringLengthPtr* 参数。 如果值为以 null 结尾的字符串，则应用程序将在 *BufferLength* 参数中指定该字符串的最大长度，驱动程序将返回 *\* StringLengthPtr* 缓冲区中该字符串的长度。  
  
 允许应用程序调用 **SQLGetStmtAttr** 与 ODBC 2 一起使用。*x* 驱动程序时，在驱动程序管理器中将对 **SQLGetStmtAttr** 的调用映射到 **SQLGetStmtOption**。  
  
 以下语句属性是只读的，因此可以通过 **SQLGetStmtAttr** 进行检索，但 **SQLSetStmtAttr** 不能对其进行设置：  
  
-   SQL_ATTR_IMP_PARAM_DESC  
  
-   SQL_ATTR_IMP_ROW_DESC  
  
-   SQL_ATTR_ROW_NUMBER  
  
 有关可以设置和检索的特性的列表，请参阅 [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|返回连接属性的设置|[SQLGetConnectAttr 函数](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|设置连接属性|[SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
