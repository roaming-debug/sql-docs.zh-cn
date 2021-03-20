---
description: 准备和执行语句 (ODBC)
title: " (ODBC) 准备和执行语句 |Microsoft Docs"
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
- statement preparation
ms.assetid: 0adecc63-4da5-486c-bc48-09a004a2fae6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 94270caf9aae58a48ec9905062b7f8f317cdfeb3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753717"
---
# <a name="prepare-and-execute-a-statement-odbc"></a>准备和执行语句 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-prepare-a-statement-once-and-then-execute-it-multiple-times"></a>准备一次语句，然后多次执行它  
  
1.  调用 [SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md) 可准备语句。  
  
2.  也可以调用 [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) 来确定预定义语句中的参数数量。  
  
3.  （可选）对于预定义语句中的每个参数：  
  
    -   调用 [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) 以获取参数信息。  
  
    -   使用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md)将每个参数绑定到程序变量。 设置任何执行时数据参数。  
  
4.  对于每次执行预定义语句：  
  
    -   如果语句有参数标记，请将数据值放到绑定参数缓冲区中。  
  
    -   调用 [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) 以执行预定义语句。  
  
    -   如果使用执行时数据输入参数，则 [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) 将返回 SQL_NEED_DATA。 使用 [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) 和 [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)以区块形式发送数据。  
  
### <a name="to-prepare-a-statement-with-column-wise-parameter-binding"></a>用按列参数绑定预定义语句  
  
1.  调用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 设置以下属性：  
  
    -   将 SQL_ATTR_PARAMSET_SIZE 设置为参数集 (S) 的数目。  
  
    -   将 SQL_ATTR_PARAM_BIND_TYPE 设置为 SQL_PARAMETER_BIND_BY_COLUMN。  
  
    -   将 SQL_ATTR_PARAMS_PROCESSED_PTR 属性设置为指向 SQLUINTEGER 变量，以包含所处理的参数个数。  
  
    -   将 SQL_ATTR_PARAMS_STATUS_PTR 设置为指向 SQLUSSMALLINT 变量的数组 array[S]，以包含参数状态指示器。  
  
2.  调用 SQLPrepare 以准备语句。  
  
3.  也可以调用 [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) 来确定预定义语句中的参数数量。  
  
4.  （可选）对于预定义语句中的每个参数，调用 SQLDescribeParam 以获取参数信息。  
  
5.  对于每个参数标记：  
  
    -   分配 S 参数缓冲区的数组以存储数据值。  
  
    -   分配 S 参数缓冲区的数组以存储数据长度。  
  
    -   调用 SQLBindParameter，将参数数据值和数据长度数组绑定到语句参数。  
  
    -   如果参数是执行时数据文本或映像参数，则设置它。  
  
    -   如果使用任何执行时数据参数，则设置它们。  
  
6.  对于每次执行预定义语句：  
  
    -   将 S 个数据值和 S 个数据长度放到绑定参数数组中。  
  
    -   调用 SQLExecute 以执行预定义语句。  
  
    -   如果使用执行时数据输入参数，则 SQLExecute 返回 SQL_NEED_DATA。 使用 SQLParamData 和 SQLPutData 以区块形式发送数据。  
  
### <a name="to-prepare-a-statement-with-row-wise-bound-parameters"></a>用按行绑定参数预定义语句  
  
1.  分配结构数组 [S]，其中，S 是参数的集合数。 该结构对于每个参数有一个元素，并且每个元素有两部分：  
  
    -   第一部分是合适的数据类型的变量，以包含参数数据。  
  
    -   第二部分是 SQLINTEGER 变量，以包含状态指示器。  
  
2.  调用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 设置以下属性：  
  
    -   将 SQL_ATTR_PARAMSET_SIZE 设置为参数集 (S) 的数目。  
  
    -   将 SQL_ATTR_PARAM_BIND_TYPE 设置为在步骤 1 中分配的结构的大小。  
  
    -   将 SQL_ATTR_PARAMS_PROCESSED_PTR 属性设置为指向 SQLUINTEGER 变量，以包含所处理的参数个数。  
  
    -   将 SQL_ATTR_PARAMS_STATUS_PTR 设置为指向 SQLUSSMALLINT 变量的数组 array[S]，以包含参数状态指示器。  
  
3.  调用 SQLPrepare 以准备语句。  
  
4.  对于每个参数标记，调用 SQLBindParameter，将参数数据值和数据长度指针指向其在步骤1中分配的结构数组的第一个元素中的变量。 如果参数是执行时数据参数，则设置它。  
  
5.  对于每次执行预定义语句：  
  
    -   用数据值填充绑定参数缓冲区数组。  
  
    -   调用 SQLExecute 以执行预定义语句。 驱动程序将有效地执行 SQL 语句 S 次，每组参数一次。  
  
    -   如果使用执行时数据输入参数，则 SQLExecute 返回 SQL_NEED_DATA。 使用 SQLParamData 和 SQLPutData 以区块形式发送数据。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行查询操作指南主题 ](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
