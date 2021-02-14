---
description: 映射替换函数以实现应用程序后向兼容性
title: 映射替换函数以实现应用的兼容性-ODBC |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- mapping replacement functions [ODBC]
- upgrading applications [ODBC], mapping replacement functions
- compatibility [ODBC], mapping replacement functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], mapping replacement functions
- application upgrades [ODBC], mapping replacement functions
- backward compatibility [ODBC], mapping replacement functions
ms.assetid: f5e6d9da-76ef-42cb-b3f5-f640857df732
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4c31d523a9fb50dd45bf8ab56c551d3830b0861
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082158"
---
# <a name="mapping-replacement-functions-for-backward-compatibility-of-applications"></a>映射替换函数以实现应用程序后向兼容性
只要不使用新功能，*通过 odbc 2.X 驱动程序管理* 器处理的 odbc 3.x 应用程序就可以 *使用 odbc 2.x* *驱动程序。* 不过，复制的功能和行为更改都将影响 ODBC 1.x 应用程序 *在 odbc 2.x* *驱动程序上* 的工作方式。 当 *使用 ODBC 2.x* 驱动程序时，驱动程序管理器会将 *以下 odbc 1.x* 函数（已将一个或多个 odbc *2. x* 函数）映射到相应 *的 odbc 2.x 函数。*  
  
|ODBC *2.x* 函数|ODBC *2.x* 函数|  
|-------------------------|-------------------------|  
|**SQLAllocHandle**|**SQLAllocEnv**、 **SQLAllocConnect** 或 **SQLAllocStmt**|  
|**SQLBulkOperations**|**SQLSetPos**|  
|**SQLColAttribute**|**SQLColAttributes**|  
|**SQLEndTran**|**SQLTransact**|  
|**SQLFetch**|**SQLExtendedFetch**|  
|**SQLFetchScroll**|**SQLExtendedFetch**|  
|**SQLFreeHandle**|**SQLFreeEnv**、 **SQLFreeConnect** 或 **SQLFreeStmt**|  
|**SQLGetConnectAttr**|**SQLGetConnectOption**|  
|**SQLGetDiagRec**|**SQLError**|  
|**SQLGetStmtAttr**|**SQLGetStmtOption**[1]|  
|**SQLSetConnectAttr**|**SQLSetConnectOption**|  
|**SQLSetStmtAttr**|**SQLSetStmtOption**[1]|  
  
 [1] 还可以执行其他操作，具体取决于所请求的属性。  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
 根据需要，驱动程序管理器会将此映射到 **SQLAllocEnv**、 **SQLAllocConnect** 或 **SQLAllocStmt**。 对 **SQLAllocHandle** 的以下调用：  
  
```  
SQLAllocHandle(HandleType, InputHandle, OutputHandlePtr);  
```  
  
 将导致驱动程序管理器执行以下 (概念，不) 映射进行错误检查：  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLAllocEnv(OutputHandlePtr));  
   case SQL_HANDLE_DBC: return (SQLAllocConnect (InputHandle, OutputHandlePtr));  
   case SQL_HANDLE_STMT: return (SQLAllocStmt (InputHandle, OutputHandlePtr));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
 驱动程序管理器将此映射到 **SQLSetPos**。 对 **SQLBulkOperations** 的以下调用：  
  
```  
SQLBulkOperations(hstmt, Operation);  
```  
  
 将产生以下一系列步骤：  
  
1.  如果 SQL_ADD 操作参数，则驱动程序管理器将调用 **SQLSetPos** ，如下所示：  
  
    ```  
    SQLSetPos (hstmt, 0, SQL_ADD, SQL_LOCK_NO_CHANGE);  
    ```  
  
2.  如果未 SQL_ADD 操作参数，则驱动程序将返回 SQLSTATE HY092 (属性/选项标识符) 无效。  
  
3.  如果应用程序尝试更改对 **SQLFetch** 或 **SQLFetchScroll** 和 **SQLBulkOperations** 的调用之间的 SQL_ATTR_ROW_STATUS_PTR，驱动程序管理器将返回 SQLSTATE HY011 (属性 "现在无法设置) "。  
  
4.  如果 SQL_ADD 操作参数，则应用程序必须调用 **SQLBindCol** 来绑定要插入的数据。 它无法调用 **SQLSetDescField** 或 **SQLSetDescRec** 来绑定要插入的数据。  
  
5.  如果 SQL_ADD 操作参数并且要插入的行数与当前行集的大小不同，则必须调用 **SQLSetStmtAttr** ，将 SQL_ATTR_ROW_ARRAY_SIZE 语句特性设置为在调用 **SQLBulkOperations** 之前要插入的行数。 若要恢复到以前的行集大小，应用程序必须先设置 SQL_ATTR_ROW_ARRAY_SIZE 语句特性，然后才能调用 **SQLFetch**、 **SQLFetchScroll** 或 **SQLSetPos** 。  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
 驱动程序管理器将此映射到 **SQLColAttributes**。 对 **SQLColAttribute** 的以下调用：  
  
```  
SQLColAttribute(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
```  
  
 将产生以下一系列步骤：  
  
1.  如果 *FieldIdentifier* 是下列其中一项：  
  
     SQL_DESC_PRECISION、SQL_DESC_SCALE、SQL_DESC_LENGTH、SQL_DESC_OCTET_LENGTH、SQL_DESC_UNNAMED、SQL_DESC_BASE_COLUMN_NAME、SQL_DESC_LITERAL_PREFIX、SQL_DESC_LITERAL_SUFFIX 或 SQL_DESC_LOCAL_TYPE_NAME  
  
     驱动程序管理器返回 SQL_ERROR，其中 SQLSTATE HY091 (描述符字段标识符) 无效。 本部分没有进一步的规则适用。  
  
2.  驱动程序管理器将 SQL_COLUMN_COUNT、SQL_COLUMN_NAME 或 SQL_COLUMN_NULLABLE 分别映射到 SQL_DESC_COUNT、SQL_DESC_NAME 或 SQL_DESC_NULLABLE。  (*，ODBC 2.x* 驱动程序只需要支持 SQL_COLUMN_COUNT、SQL_COLUMN_NAME 和 SQL_COLUMN_NULLABLE，而不是 SQL_DESC_COUNT、SQL_DESC_NAME 和 SQL_DESC_NULLABLE。 ) 对 SQLColAttribute 的调用映射到：  
  
    ```  
    SQLColAttributes(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
    ```  
  
3.  所有其他 *FieldIdentifier* 值将传递给驱动程序， **SQLColAttribute** 映射到 **SQLColAttributes** ，如前文所述。  
  
4.  如果 *BufferLength* 小于0，则驱动程序管理器将返回 SQL_ERROR，其中 SQLSTATE HY090 (字符串或缓冲区长度) 无效。 本部分没有进一步的规则适用。  
  
5.  如果 SQL_DESC_CONCISE_TYPE *FieldIdentifier* ，并且返回的类型为简洁日期时间数据类型，则驱动程序管理器将映射日期、时间和时间戳代码的返回值。  
  
6.  驱动程序管理器执行必要的检查，以查看是否需要引发 SQLSTATE HY010 (函数序列) 错误。 如果是这样，驱动程序管理器将返回 SQL_ERROR 和 SQLSTATE HY010 (函数序列错误) 。 本部分没有进一步的规则适用。  
  
## <a name="sqlendtran"></a>SQLEndTran  
 驱动程序管理器将此映射到 **SQLTransact**。 对 **SQLEndTran** 的以下调用：  
  
```  
SQLEndTran(HandleType, Handle, CompletionType);  
```  
  
 将导致驱动程序管理器执行以下 (概念，不) 映射进行错误检查：  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV:return(SQLTransact(Handle, SQL_NULL_HDBC, CompletionType));  
   case SQL_HANDLE_DBC:return(SQLTransact(SQL_NULL_HENV, Handle, CompletionType);  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlfetch"></a>SQLFetch  
 驱动程序管理器将此映射到带有 SQL_FETCH_NEXT 的 *FetchOrientation* 参数的 **SQLExtendedFetch** 。 对 **SQLFetch** 的以下调用：  
  
```  
SQLFetch (StatementHandle);  
```  
  
 将导致驱动程序管理器调用 **SQLExtendedFetch**，如下所示：  
  
```  
rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, &RowCount, RowStatusArray);  
```  
  
 在此调用中， *pcRow* 参数设置为应用程序通过调用 **SQLSetStmtAttr** 将 SQL_ATTR_ROWS_FETCHED_PTR 语句特性设置为的值。  
  
> [!NOTE]  
>  当应用程序调用 **SQLSetStmtAttr** 将 SQL_ATTR_ROW_STATUS_PTR 设置为指向状态数组时，驱动程序管理器将缓存指针。 *RowStatusArray* 可以等于 null 指针。  
  
 如果该驱动程序不支持 **SQLExtendedFetch** ，并且已加载了游标库，则驱动程序管理器将使用游标库的 **SQLExtendedFetch** 将 **SQLFetch** 映射到 **SQLExtendedFetch**。 如果驱动程序不支持 **SQLExtendedFetch** ，并且未加载游标库，驱动程序管理器会将对 **SQLFetch** 的调用传递给驱动程序。 如果应用程序调用 **SQLSetStmtAttr** 来设置 SQL_ATTR_ROW_STATUS_PTR，则驱动程序管理器将确保填充数组。 如果应用程序调用 **SQLSetStmtAttr** SQL_ATTR_ROWS_FETCHED_PTR 设置，则驱动程序管理器会将此字段设置为1。  
  
## <a name="sqlfetchscroll"></a>SQLFetchScroll  
 驱动程序管理器将此映射到 **SQLExtendedFetch**。 对 **SQLFetchScroll** 的以下调用：  
  
```  
SQLFetchScroll(StatementHandle, FetchOrientation, FetchOffset);  
```  
  
 将产生以下一系列步骤：  
  
1.  当应用程序调用 **SQLSetStmtAttr** 来设置 SQL_ATTR_ROW_STATUS_PTR (这会将 IRD) 中的 SQL_DESC_ARRAY_STATUS_PTR 字段设置为指向状态数组，驱动程序管理器会缓存此指针。 让此指针 *RowStatusArray*;否则，让 *RowStatusArray* 等于 null 指针。 如果 *RowStatusArray* 参数设置为 null 指针，则驱动程序管理器将生成行状态数组。  
  
2.  如果 *FetchOrientation* 不是 SQL_FETCH_NEXT、SQL_FETCH_PRIOR、SQL_FETCH_ABSOLUTE、SQL_FETCH_RELATIVE、SQL_FETCH_FIRST、SQL_FETCH_LAST 或 SQL_FETCH_BOOKMARK 之一，驱动程序管理器将返回，其中 SQL_ERROR 和 SQLSTATE HY106 (提取类型超出范围) 。 本部分没有进一步的规则适用。  
  
3.  大小写：  
  
-   如果 *FetchOrientation* 等于 SQL_FETCH_BOOKMARK，则：  
  
    -   如果前面调用 **SQLSetStmtAttr** 来设置 SQL_ATTR_FETCH_BOOKMARK_PTR 的值，则让 *Bmk* 成为通过取消引用指针 SQL_DESC_FETCH_BOOKMARK_PTR 获取的值。  
  
    -   否则，返回带有 SQLSTATE HY111 的 SQL_ERROR () 的书签值无效。 本部分没有进一步的规则适用。  
  
     驱动程序管理器现在调用 **SQLExtendedFetch**，如下所示：  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, Bmk, pcRow, RowStatusArray);  
    ```  
  
-   否则，驱动程序管理器将调用 **SQLExtendedFetch**，如下所示：  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, pcRow, RowStatusArray);  
    ```  
  
     在这些调用中， *pcRow* 参数设置为应用程序通过调用 **SQLSetStmtAttr** 将 SQL_ATTR_ROWS_FETCHED_PTR 语句特性设置为的值。  
  
-   SQL_ATTR_ROW_ARRAY_SIZE 映射到 SQL_ROWSET_SIZE。  
  
-   如果 *rc* 等于 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，并且 *FetchOrientation* 等于 SQL_FETCH_BOOKMARK，并且 *FetchOffset* 不等于0，则驱动程序管理器将发出警告，SQLSTATE 01S10 (尝试提取书签偏移量、忽略偏移量) 并返回 SQL_SUCCESS_WITH_INFO。  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 根据需要，驱动程序管理器会将此映射到 **SQLFreeEnv**、 **SQLFreeConnect** 或 **SQLFreeStmt** 。 对 **SQLFreeHandle** 的以下调用：  
  
```  
SQLFreeHandle(HandleType, Handle);  
```  
  
 将导致驱动程序管理器执行以下 (概念，不) 映射进行错误检查：  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLFreeEnv(Handle));  
   case SQL_HANDLE_DBC: return (SQLFreeConnect(Handle));  
   case SQL_HANDLE_STMT: return (SQLFreeStmt(Handle, SQL_DROP));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
 驱动程序管理器将此映射到 **SQLGetConnectOption**。 对 **SQLGetConnectAttr** 的以下调用：  
  
```  
SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 将产生以下一系列步骤：  
  
1.  如果 *特性* 不是驱动程序定义的连接或语句特性，并且不是 *在 ODBC 2.x* 中定义的属性，则驱动程序管理器将返回 SQL_ERROR 与 SQLSTATE HY092 () 的属性/选项标识符无效。 本部分中没有进一步的规则适用。  
  
2.  如果 *Attribute* 等于 SQL_ATTR_AUTO_IPD 或 SQL_ATTR_METADATA_ID，则驱动程序管理器会返回 SQL_ERROR，SQLSTATE HY092 (无效的属性/选项标识符) 。  
  
3.  驱动程序管理器执行必要的检查，以查看 SQLSTATE 08003 (连接是否未打开) 或 SQLSTATE HY010 是否需要引发)  (函数序列错误。 如果是这样，驱动程序管理器将返回 SQL_ERROR 并发布相应的错误消息。 本部分没有进一步的规则适用。  
  
4.  驱动程序管理器调用 **SQLGetConnectOption** ，如下所示：  
  
    ```  
    SQLGetConnectOption (ConnectionHandle, Attribute, ValuePtr);  
    ```  
  
     请注意， *BufferLength* 和 *StringLengthPtr* 将被忽略。  
  
## <a name="sqlgetdata"></a>SQLGetData  
 当使用 ODBC *2.x 驱动程序的 odbc* 1.x 应用程序调用 **SQLGetData** 时，如果 *ColumnNumber* 参数等于0，*则 ODBC 1.x* *驱动程序管理* 器会将此映射到 *选项* 属性设置为 SQL_GET_BOOKMARK 的对 **SQLGetStmtOption** 的调用。  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
 驱动程序管理器将此映射到 **SQLGetStmtOption**。 对 **SQLGetStmtAttr** 的以下调用：  
  
```  
SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 将产生以下一系列步骤：  
  
1.  如果 *特性* 不是驱动程序定义的连接或语句特性，并且不是 *在 ODBC 2.x* 中定义的属性，则驱动程序管理器将返回 SQL_ERROR 与 SQLSTATE HY092 () 的属性/选项标识符无效。 本部分中没有进一步的规则适用。  
  
2.  如果 *特性* 是下列其中一项：  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     驱动程序管理器返回 SQL_ERROR 与 SQLSTATE HY092 (属性/选项标识符) 无效。 本部分没有进一步的规则适用。  
  
3.  驱动程序管理器执行必要的检查，以查看是否需要引发 SQLSTATE HY010 (函数序列) 错误。 如果是这样，驱动程序管理器将返回 SQL_ERROR 和 SQLSTATE HY010 (函数序列错误) 。 本部分没有进一步的规则适用。  
  
4.  如果 *Attribute* 等于 SQL_ATTR_ROWS_FETCHED_PTR，驱动程序管理器将返回一个指向内部驱动程序管理器变量 *鱼尾纹* 的指针，该指针在调用 **SQLExtendedFetch** 时使用或将使用它。 本部分没有进一步的规则适用。  
  
5.  如果 *Attribute* 等于 SQL_DESC_FETCH_BOOKMARK_PTR，则驱动程序管理器将返回在调用 **SQLSetStmtAttr** 期间缓存的适当的指针。  
  
6.  如果 *Attribute* 等于 SQL_ATTR_ROW_STATUS_PTR，则驱动程序管理器将返回在调用 **SQLSetStmtAttr** 期间缓存的适当的指针。  
  
7.  驱动程序管理器调用 **SQLGetStmtOption** ，如下所示：  
  
    ```  
    SQLGetStmtOption (hstmt, fOption, pvParam);  
    ```  
  
     其中， *hstmt*、 *fOption* 和 *pvParam* 分别设置为 *StatementHandle*、 *Attribute* 和 *将 valueptr* 的值。 *BufferLength* 和 *StringLengthPtr* 将被忽略。  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 驱动程序管理器将此映射到 **SQLSetConnectOption**。 对 **SQLSetConnectAttr** 的以下调用：  
  
```  
SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, StringLength);  
```  
  
 将产生以下一系列步骤：  
  
1.  如果 *特性* 不是驱动程序定义的连接或语句特性，并且不是 *在 ODBC 2.x* 中定义的属性，则驱动程序管理器将返回 SQL_ERROR 与 SQLSTATE HY092 () 的属性/选项标识符无效。 本部分中没有进一步的规则适用。  
  
2.  如果 *Attribute* 等于 SQL_ATTR_AUTO_IPD，驱动程序管理器将返回具有 SQLSTATE HY092 的 SQL_ERROR (属性/选项标识符) 无效。  
  
3.  驱动程序管理器执行必要的检查，以查看 SQLSTATE 08003 (连接是否未打开) 或 SQLSTATE HY010 是否需要引发)  (函数序列错误。 如果需要引发这些错误之一，驱动程序管理器将返回 SQL_ERROR 并发布相应的错误消息。 本部分没有进一步的规则适用。  
  
4.  驱动程序管理器调用 **SQLSetConnectOption** ，如下所示：  
  
    ```  
    SQLSetConnectOption (hdbc, fOption, vParam);  
    ```  
  
     其中， *hdbc*、 *fOption* 和 *vParam* 分别设置为 *ConnectionHandle*、 *Attribute* 和 *将 valueptr* 的值。 将忽略 *StringLengthPtr* 。  
  
> [!NOTE]  
>  已弃用在连接级别上设置语句属性的功能。 不应 *通过 ODBC 1.x* 应用程序在连接级别设置语句特性。  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 驱动程序管理器将此映射到 **SQLSetStmtOption**。 对 **SQLSetStmtAttr** 的以下调用：  
  
```  
SQLSetStmtAttr(StatementHandle, Attribute, ValuePtr, StringLength);  
```  
  
 将产生以下一系列步骤：  
  
1.  如果 *特性* 不是驱动程序定义的连接或语句特性，并且不是 *在 ODBC 2.x* 中定义的属性，则驱动程序管理器将返回 SQL_ERROR 与 SQLSTATE HY092 () 的属性/选项标识符无效。 本部分中没有进一步的规则适用。  
  
2.  如果 *特性* 是下列其中一项：  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     驱动程序管理器返回 SQL_ERROR 与 SQLSTATE HY092 (属性/选项标识符) 无效。 本部分没有进一步的规则适用。  
  
3.  驱动程序管理器执行必要的检查，以查看是否) 需要引发 SQLSTATE HY010 (函数序列错误。 如果是这样，驱动程序管理器将返回 SQL_ERROR 和 SQLSTATE HY010 (函数序列错误) 。 本部分没有进一步的规则适用。  
  
4.  如果 *Attribute* 等于 SQL_ATTR_PARAMSET_SIZE 或 SQL_ATTR_PARAMS_PROCESSED_PTR，请参阅本主题后面的 "处理参数数组的映射" 一节。 本部分没有进一步的规则适用。  
  
5.  如果 *Attribute* 等于 SQL_ATTR_ROWS_FETCHED_PTR，驱动程序管理器将缓存指针值以便以后用于 **SQLFetchScroll**。  
  
6.  如果 *Attribute* 等于 SQL_ATTR_ROW_STATUS_PTR，驱动程序管理器将缓存指针值以便以后用于 **SQLFetchScroll** 或 **SQLSetPos**。 本部分没有进一步的规则适用。  
  
7.  如果 *Attribute* 等于 SQL_ATTR_FETCH_BOOKMARK_PTR，驱动程序管理器将缓存 *将 valueptr* ，并在以后调用 **SQLFetchScroll** 时使用缓存的值。 本部分没有进一步的规则适用。  
  
8.  驱动程序管理器调用 **SQLSetStmtOption** ，如下所示：  
  
    ```  
    SQLSetStmtOption (hstmt, fOption, vParam);  
    ```  
  
     其中， *hstmt*、 *fOption* 和 *vParam* 分别设置为 *StatementHandle*、 *Attribute* 和 *将 valueptr* 的值。 *StringLength* 参数将被忽略。  
  
     如果 ODBC 2.x 驱动程序支持字符串、*驱动程序特定* 的语句选项，*则 odbc 2.x* 应用程序应调用 **SQLSetStmtOption** 来设置这些选项。  
  
## <a name="mappings-for-handling-parameter-arrays"></a>用于处理参数数组的映射  
 当应用程序调用时：  
  
```  
SQLSetStmtAttr (StatementHandle, SQL_ATTR_PARAMSET_SIZE, Size, StringLength);  
```  
  
 驱动程序管理器调用：  
  
```  
SQLParamOptions (StatementHandle, Size, &RowCount);  
```  
  
 当应用程序调用 **SQLGetStmtAttr** 检索 SQL_ATTR_PARAMS_PROCESSED_PTR 时，驱动程序管理器稍后将返回指向此变量的指针。 在将语句句柄返回到已准备或已分配状态之前，驱动程序管理器无法更改此内部变量。  
  
 *ODBC 3.x* 应用程序可以调用 **SQLGetStmtAttr** 来获取 SQL_ATTR_PARAMS_PROCESSED_PTR 的值，即使它未在 APD 中显式设置 SQL_DESC_ARRAY_SIZE 字段。 例如，如果应用程序具有在 **SQLExecute** 返回 SQL_NEED_DATA 时要处理的参数的当前 "行"，则可能会出现这种情况。 无论 SQL_DESC_ARRAY_SIZE 为1还是大于1，都将调用此例程。 为此，驱动程序管理器将需要定义此内部变量，无论应用程序是否已调用 **SQLSetStmtAttr** 来设置 APD 中的 SQL_DESC_ARRAY_SIZE 字段。 如果尚未设置 SQL_DESC_ARRAY_SIZE，驱动程序管理器必须确保此变量在从 **SQLExecDirect** 或 **SQLExecute** 返回之前包含值1。  
  
## <a name="error-handling"></a>错误处理  
 在 ODBC *3.x 中，* 调用 **SQLFetch** 或 **SQLFetchScroll** 将填充 IRD 中的 SQL_DESC_ARRAY_STATUS_PTR，给定诊断记录的 SQL_DIAG_ROW_NUMBER 字段包含此记录所属行集中的行号。 使用此，应用程序可以将错误消息与给定的行位置相关联。  
  
 *ODBC 2.x* 驱动程序将无法提供此功能。 但是，它将在行) 中提供 SQLSTATE 01S01 (错误的错误分界。 在 *针对 odbc 2.x* 驱动程序的情况下使用 **SQLFetch** 或 **SQLFetchScroll** 的 odbc *1.x 应用程序* 需要了解这一事实。 另请注意，此类应用程序将不能调用 **SQLGetDiagField** 来实际获取 SQL_DIAG_ROW_NUMBER 字段。 使用 *odbc 2.x* *驱动程序的 odbc 1.x 应用程序* 将只能使用 SQL_DIAG_MESSAGE_TEXT、SQL_DIAG_NATIVE、SQL_DIAG_RETURNCODE 或 SQL_DIAG_SQLSTATE 的 *DiagIdentifier* 参数调用 **SQLGetDiagField** 。 *Odbc 2.x 驱动程序* 管理器在使用 odbc 2.x 驱动程序时维护诊断数据结构，*但 odbc 2.x* *驱动程序仅* 返回这四个字段。  
  
 当 ODBC 2.x *应用程序* 正在 *使用 odbc 2.x* 驱动程序时，如果某个操作可能导致驱动程序管理器返回多个错误，则 Odbc 1.X 驱动程序管理器可能会返回不同 *于 odbc 2.x* *驱动程序管理* 器的错误。  
  
## <a name="mappings-for-bookmark-operations"></a>书签操作的映射  
 当使用 ODBC 2.x 驱动程序 *的 odbc* *1.x 应用程序* 执行书签操作时，Odbc *2.x 驱动程序* 管理器将执行以下映射。  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 当 *使用 odbc 2.x* *驱动程序的 odbc* 1.X 应用程序调用 **SQLBindCol** 来绑定到 *fCType* 等于 SQL_C_VARBOOKMARK 的列0时，ODBC 3.x 驱动程序管理器将检查 *BUFFERLENGTH* 参数是否小于4，如果是，则返回 SQLSTATE HY090 (字符串或缓冲区长度) 无效。 如果 *BufferLength* 参数等于4，则在用 SQL_C_BOOKMARK 替换 *FCType* 后，驱动程序管理器将在驱动程序中调用 **SQLBindCol** 。  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 当 *使用 odbc 2.x* *驱动程序的 odbc* 1.X 应用程序调用 **SQLColAttribute** ，并将 *ColumnNumber* 参数设置为0时，驱动程序管理器将返回下表中列出的 *FieldIdentifier* 值。  
  
|*FieldIdentifier*|值|  
|-----------------------|-----------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|  
|SQL_DESC_CATALOG_NAME|""（空字符串）|  
|SQL_DESC_CONCISE_TYPE|SQL_BINARY|  
|SQL_DESC_COUNT|**SQLNumResultCols** 返回的值相同|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|  
|SQL_DESC_DISPLAY_SIZE|8|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|  
|SQL_DESC_LABEL|""（空字符串）|  
|SQL_DESC_LENGTH|0|  
|SQL_DESC_LITERAL_PREFIX|""（空字符串）|  
|SQL_DESC_LITERAL_SUFFIX|""（空字符串）|  
|SQL_DESC_LOCAL_TYPE_NAME|""（空字符串）|  
|SQL_DESC_NAME|""（空字符串）|  
|SQL_DESC_NULLABLE|SQL_NO_NULLS|  
|SQL_DESC_OCTET_LENGTH|4|  
|SQL_DESC_PRECISION|4|  
|SQL_DESC_SCALE|0|  
|SQL_DESC_SCHEMA_NAME|""（空字符串）|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|  
|SQL_DESC_TABLE_NAME|""（空字符串）|  
|SQL_DESC_TYPE|SQL_BINARY|  
|SQL_DESC_TYPE_NAME|""（空字符串）|  
|SQL_DESC_UNNAMED|SQL_UNNAMED|  
|SQL_DESC_UNSIGNED|SQL_FALSE|  
|SQL_DESC_UPDATEABLE|SQL_ATTR_READ_ONLY|  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 当使用 ODBC 2.x *驱动程序的 odbc* *1.x 应用程序* 调用 **SQLDescribeCol** ，并将 *ColumnNumber* 参数设置为0时，驱动程序管理器将返回下表中列出的值。  
  
|Buffer|Value|  
|------------|-----------|  
|ColumnName|""（空字符串）|  
|*NameLengthPtr|0|  
|*DataTypePtr|SQL_BINARY|  
|*ColumnSizePtr|4|  
|*DecimalDigitsPtr|0|  
|*NullablePtr|SQL_NO_NULLS|  
  
### <a name="sqlgetdata"></a>SQLGetData  
 当使用 ODBC 2.x *驱动程序的 odbc* *1.x 应用程序* 对 **SQLGetData** 进行以下调用以检索书签：  
  
```  
SQLGetData(StatementHandle, 0, SQL_C_VARBOOKMARK, TargetValuePtr, BufferLength, StrLen_or_IndPtr)  
```  
  
 调用映射到带有 SQL_GET_BOOKMARK 的 *fOption* 的 **SQLGetStmtOption** ，如下所示：  
  
```  
SQLGetStmtOption(hstmt, SQL_GET_BOOKMARK, TargetValuePtr)  
```  
  
 其中 *hstmt* 和 *pvParam* 分别设置为 *StatementHandle* 和 *TargetValuePtr* 中的值。 书签由 *pvParam* (*TargetValuePtr*) 参数所指向的缓冲区返回。 **SQLGetData** 调用中的由 *StrLen_or_IndPtr* 参数指向的缓冲区中的值设置为4。  
  
 若要在调用 **SQLGetData** 之前调用 **SQLFETCH** ，*但 ODBC 2.X* 驱动程序不支持 **SQLExtendedFetch**，则需要此映射。 在这种情况下， **SQLFetch** 将传递 *到 ODBC 2.x* 驱动程序，在这种情况下不支持书签检索。  
  
 不能 *在 ODBC 2.x* 驱动程序中调用 **SQLGetData** 来检索部分中的书签，因此，在将 *BufferLength* 参数设置为小于4且 *ColumnNumber* 参数设置为0的情况下调用 **SQLGetData** 时，将返回 SQLSTATE HY090 (无效的字符串或缓冲区长度) 。 不过，可以多次调用 **SQLGetData** 来检索同一书签。  
  
### <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 当使用 ODBC 2.x *驱动程序的 odbc* *1.x 应用程序* 调用 **SQLSetStmtAttr** 将 SQL_ATTR_USE_BOOKMARKS 属性设置为 SQL_UB_VARIABLE 时，驱动程序管理器会将该属性设置为 *基础 ODBC 2.x* 驱动程序中的 SQL_UB_ON。
