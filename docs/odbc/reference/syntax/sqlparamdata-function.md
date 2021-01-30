---
description: SQLParamData 函数
title: SQLParamData 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLParamData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLParamData
helpviewer_keywords:
- SQLParamData function [ODBC]
ms.assetid: 68fe010d-9539-4e5b-a260-c8d32423b1db
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e050102612c7e5d370e730909522fa23e801a8f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195844"
---
# <a name="sqlparamdata-function"></a>SQLParamData 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLParamData** 与 **SQLPutData** 一起用于在语句执行时提供参数数据，使用 **SQLGetData** 检索流出的输出参数数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLParamData(  
     SQLHSTMT       StatementHandle,  
     SQLPOINTER *   ValuePtrPtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *ValuePtrPtr*  
 输出指向某个缓冲区的指针，该缓冲区用于返回在 **SQLBindParameter** (中为参数数据) 指定的 *ParameterValuePtr* 缓冲区的地址 SQL_DESC_DATA_PTR，或者在 **SQLBindCol** (中为列数据) 指定的 *TargetValuePtr* 缓冲区的地址。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NEED_DATA、SQL_NO_DATA、SQL_STILL_EXECUTING、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_PARAM_DATA_AVAILABLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLParamData** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLParamData** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|绑定参数的 **SQLBindParameter** 中的 *ValueType* 参数标识的数据值无法转换为 **SQLBindParameter** 中 *ParameterType* 参数所标识的数据类型。<br /><br /> 为绑定为 SQL_PARAM_INPUT_OUTPUT 或 SQL_PARAM_OUTPUT 的参数返回的数据值无法转换为 **SQLBindParameter** 中 *ValueType* 参数标识的数据类型。<br /><br />  (如果一个或多个行的数据值无法转换，但已成功返回一个或多个行，则此函数将返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22026|字符串数据，长度不匹配|**SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型为 "Y"，为长参数发送了较少的数据 (数据类型为 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或长数据源特定的数据类型) 与 **SQLBindParameter** 中的 *StrLen_or_IndPtr* 参数一起指定。<br /><br /> **SQLGetInfo** 中的 SQL_NEED_LONG_DATA_LEN 信息类型为 "Y"，为长列发送了较少的数据 (数据类型为 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或长数据源特定的数据类型) ，该数据类型与使用 **SQLBulkOperations** 添加或更新的数据行中的列相对应，或使用 **SQLSetPos** 进行更新。|  
|40001|序列化失败|由于另一个事务发生资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ;然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 之前的函数调用不是对在其中 SQL_NEED_DATA 了返回代码的 **SQLExecDirect**、 **SQLExecute**、 **SQLBulkOperations** 或 **SQLSetPos** 的调用，或者以前的函数调用是对 **SQLPutData** 的调用。<br /><br /> 以前的函数调用是对 **SQLParamData** 的调用。<br /><br />  (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLParamData** 函数时，此异步函数仍在执行。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br /> 为 *StatementHandle* 调用了 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用 **SQLCancel** 。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 相对应的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
 如果在 SQL 语句中发送参数的数据时调用 **SQLParamData** ，则它可以返回可由调用的函数返回的任何 SQLSTATE，以 (**SQLExecute** 或 **SQLExecDirect**) 执行该语句。 如果在发送使用 **SQLBulkOperations** 更新或添加的列的数据时调用此方法，或者使用 **SQLSetPos** 进行更新，则它可以返回 **SQLBulkOperations** 或 **SQLSetPos** 返回的任何 SQLSTATE。  
  
## <a name="comments"></a>注释  
 可以调用 **SQLParamData** 来提供两个用途的数据执行数据：要在调用 **SQLExecute** 或 **SQLExecDirect** 时使用的参数数据，或在通过调用 **SQLBulkOperations** 或通过调用 **SQLSetPos** 更新或添加行时使用的列数据。 执行时， **SQLParamData** 会向应用程序返回驱动程序所需的数据的指示器。  
  
 当应用程序调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** 时，驱动程序将返回 SQL_NEED_DATA 如果它需要执行时数据数据。 然后，应用程序调用 **SQLParamData** 来确定要发送的数据。 如果驱动程序需要参数数据，则驱动程序将在 *\* ValuePtrPtr* 输出缓冲区中返回应用程序放入行集缓冲区中的值。 应用程序可以使用此值来确定驱动程序正在请求的参数数据。 如果驱动程序需要列数据，则驱动程序会在 *\* ValuePtrPtr* 缓冲区中返回列最初绑定到的地址，如下所示：  
  
 *绑定地址*  + *绑定偏移量*+ ( # B1 *行号*-1) x *元素大小*)   
  
 其中的变量定义如下表所示。  
  
|变量|说明|  
|--------------|-----------------|  
|*绑定地址*|通过 **SQLBindCol** 中的 *TargetValuePtr* 参数指定的地址。|  
|*绑定偏移量*|存储在用 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性指定的地址的值。|  
|*行号*|行集中从1开始的行号。 对于单行读取（默认值），此值为1。|  
|*元素大小*|数据和长度/指示器缓冲区的 SQL_ATTR_ROW_BIND_TYPE 语句特性的值。|  
  
 它还返回 SQL_NEED_DATA，它是应用程序的指示符，它应调用 **SQLPutData** 来发送数据。  
  
 应用程序会根据需要多次调用 **SQLPutData** 以发送列或参数的执行时数据数据。 为列或参数发送所有数据后，应用程序将再次调用 **SQLParamData** 。 如果 **SQLParamData** 再次返回 SQL_NEED_DATA，则必须为另一个参数或列发送数据。 因此，应用程序再次调用 **SQLPutData**。 如果为所有参数或列发送了所有执行时数据数据，则 **SQLParamData** 将返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，未定义 *\* ValuePtrPtr* 中的值，并且可以执行 SQL 语句，或者可以处理 **SQLBulkOperations** 或 **SQLSetPos** 调用。  
  
 如果 **SQLParamData** 提供的搜索更新或 delete 语句的参数数据不会影响数据源中的任何行，则对 **SQLParamData** 的调用将返回 SQL_NO_DATA。  
  
 若要详细了解如何在语句执行时传递执行时数据参数数据，请参阅 [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) 中的 "传递参数值" 和 [发送长数据](../../../odbc/reference/develop-app/sending-long-data.md)。 有关如何更新或添加执行时数据列数据的详细信息，请参阅[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)中的 "使用书签执行批量更新" 一[节中的](../../../odbc/reference/syntax/sqlsetpos-function.md)"使用 SQLSetPos"，以及[长数据、SQLSetPos 和 SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md)。  
  
 可以调用 **SQLParamData** 来检索流式处理的输出参数。 当 **SQLMoreResults**、 **SQLExecute**、 **SQLGetData** 或 **SQLExecDirect** 返回 SQL_PARAM_DATA_AVAILABLE 时，请调用 **SQLParamData** 以确定哪个参数具有可用值。 有关 SQL_PARAM_DATA_AVAILABLE 和流式输出参数的详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
## <a name="code-example"></a>代码示例  
 请参阅 [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|在语句中返回有关参数的信息|[SQLDescribeParam 函数](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|在执行时发送参数数据|[SQLPutData 函数](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
