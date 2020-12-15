---
description: 直接执行语句 (ODBC)
title: 直接 (ODBC) 执行语句 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
ms.assetid: b690f9de-66e1-4ee5-ab6a-121346fb5f85
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96acd03e9caed3660801c3097ab1133dc47d075e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467728"
---
# <a name="execute-a-statement-directly-odbc"></a>直接执行语句 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-execute-a-statement-directly-and-one-time-only"></a>直接执行语句并且只执行一次  
  
1.  如果语句具有参数标记，请使用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md) 将每个参数绑定到程序变量。 使用数据值填充程序变量，然后设置任何执行时数据参数。  
  
2.  调用 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md) 以执行该语句。  
  
3.  如果使用执行时数据输入参数，则 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md) 将返回 SQL_NEED_DATA。 使用 [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) 和 [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)以区块形式发送数据。  

### <a name="to-execute-a-statement-multiple-times-by-using-column-wise-parameter-binding"></a>通过使用按列参数绑定多次执行语句  
  
1.  调用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 设置以下属性：  
  
     将 SQL_ATTR_PARAMSET_SIZE 设置为参数集 (S) 的数目。  
  
     将 SQL_ATTR_PARAM_BIND_TYPE 设置为 SQL_PARAMETER_BIND_BY_COLUMN。  
  
     将 SQL_ATTR_PARAMS_PROCESSED_PTR 属性设置为指向 SQLUINTEGER 变量，以包含所处理的参数个数。  
  
     将 SQL_ATTR_PARAMS_STATUS_PTR 设置为指向 SQLUSSMALLINT 变量的数组 [S]，以包含参数状态指示器。  
  
2.  对于每个参数标记：  
  
     分配 S 参数缓冲区的数组以存储数据值。  
  
     分配 S 参数缓冲区的数组以存储数据长度。  
  
     调用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md) ，将参数数据值和数据长度数组绑定到语句参数。  
  
     设置任意执行时数据 text 或 image 参数。  
  
     将 S 数据值和 S 数据长度放到绑定参数数组中。  
  
3.  调用 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md) 以执行该语句。 驱动程序将有效地执行该语句 S 次，每组参数一次。  
  
4.  如果使用执行时数据输入参数，则 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md) 将返回 SQL_NEED_DATA。 使用 [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) 和 [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)以区块形式发送数据。  
  
### <a name="to-execute-a-statement-multiple-times-by-using-row-wise-parameter-binding"></a>通过使用按行参数绑定多次执行语句  
  
1.  分配结构数组 [S]，其中，S 是参数的集合数。 该结构对于每个参数有一个元素，并且每个元素有两部分：  
  
     第一部分是合适的数据类型的变量，以包含参数数据。  
  
     第二部分是 SQLINTEGER 变量，以包含状态指示器。  
  
2.  调用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 设置以下属性：  
  
     将 SQL_ATTR_PARAMSET_SIZE 设置为参数集 (S) 的数目。  
  
     将 SQL_ATTR_PARAM_BIND_TYPE 设置为在步骤 1 中分配的结构的大小。  
  
     将 SQL_ATTR_PARAMS_PROCESSED_PTR 属性设置为指向 SQLUINTEGER 变量，以包含所处理的参数个数。  
  
     将 SQL_ATTR_PARAMS_STATUS_PTR 设置为指向 SQLUSSMALLINT 变量的数组 [S]，以包含参数状态指示器。  
  
3.  对于每个参数标记，调用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md) ，将参数的数据值和数据长度指针指向其在步骤1中分配的结构数组的第一个元素中的变量。 如果参数是执行时数据参数，则设置它。  
  
4.  用数据值填充绑定参数缓冲区数组。  
  
5.  调用 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md) 以执行该语句。 驱动程序将有效地执行该语句 S 次，每组参数一次。  
  
6.  如果使用执行时数据输入参数，则 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md) 将返回 SQL_NEED_DATA。 使用 [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) 和 [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)以区块形式发送数据。  
  
 **注意** 与 [SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md) 和 [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) 相比，与 [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)一起使用时，对列和按行绑定更常见。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行查询操作指南主题 ](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
