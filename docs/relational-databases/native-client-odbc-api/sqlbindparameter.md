---
description: SQLBindParameter
title: SQLBindParameter |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBindParameter function
ms.assetid: c302c87a-e7f4-4d2b-a0a7-de42210174ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a2b6beb7697957cebcffb1f58c36a2902872ac35
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751407"
---
# <a name="sqlbindparameter"></a>SQLBindParameter
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当用于为 Native Client ODBC 驱动程序提供数据时， **SQLBindParameter** 可以消除数据转换的负担 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，从而提高应用程序的客户端和服务器组件的性能。 其他好处包括在插入或更新近似数字数据类型时减少精度损失。  
  
> [!NOTE]  
>  如果将 **char** 和 **wchar** 类型数据插入到 image 列中，则使用传入的数据的大小，而不是转换为二进制格式后数据的大小。  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序在某一参数数组的单个数组元素上遇到错误，则该驱动程序将继续为其余数组元素执行语句。 如果该应用程序为语句绑定了某一由参数状态元素构成的数组，则可以从该数组确定生成错误的参数行。  
  
 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序，则在绑定输入参数时指定 SQL_PARAM_INPUT。 在绑定用 OUTPUT 关键字定义的存储过程参数时，只指定 SQL_PARAM_OUTPUT 或 SQL_PARAM_INPUT_OUTPUT。  
  
 [](../../relational-databases/native-client-odbc-api/sqlrowcount.md) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果绑定参数数组的数组元素导致语句执行出错，则 Native Client ODBC 驱动程序的 SQLRowCount 不可靠。 ODBC 语句属性 SQL_ATTR_PARAMS_PROCESSED_PTR 报告在发生错误前已处理的行数。 然后，如有必要，该应用程序将遍历其参数状态数组，以便发现成功执行的语句数目。  
  
## <a name="binding-parameters-for-sql-character-types"></a>SQL 字符类型的绑定参数  
 如果传入的 SQL 数据类型为字符类型，则 *ColumnSize* 是 (不是字节) 的字符大小。 如果数据字符串的长度大于8000，则应将 *ColumnSize* 设置为 **SQL_SS_LENGTH_UNLIMITED**，这表示 SQL 类型的大小没有限制。  
  
 例如，如果 SQL 数据类型为 **SQL_WVARCHAR**，则 *ColumnSize* 不应大于4000。 如果实际数据长度大于4000，则应将 *ColumnSize* 设置为 **SQL_SS_LENGTH_UNLIMITED** ，以便驱动程序将使用 **nvarchar (max)** 。  
  
## <a name="sqlbindparameter-and-table-valued-parameters"></a>SQLBindParameter 和表值参数  
 与其他参数类型一样，表值参数由 SQLBindParameter 绑定。  
  
 在已绑定某一表值参数后，也绑定其列。 若要绑定列，请调用 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) ，将 SQL_SOPT_SS_PARAM_FOCUS 设置为表值参数的序号。 然后，为表值参数中的每一列调用 SQLBindParameter。 若要返回到顶级参数绑定，请将 SQL_SOPT_SS_PARAM_FOCUS 设置为 0。  
  
 有关将参数映射到表值参数的描述符字段的信息，请参阅 [Table-Valued 参数和列值的绑定和数据传输](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md)。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlbindparameter-support-for-enhanced-date-and-time-features"></a>SQLBindParameter 对日期和时间增强功能的支持  
 日期/时间类型的参数值按 [从 C 转换到 SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md)中所述的方式进行转换。 请注意，如果使用类型为 " **time** **" 和 "** **datetimeoffset** " 的参数，SQL_SS_TIMESTAMPOFFSET_STRUCT **SQL_SS_TIME2_STRUCT** (则必须将 *ValueType* 指定为 **SQL_C_DEFAULT** 或 **SQL_C_BINARY** 。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlbindparameter-support-for-large-clr-udts"></a>SQLBindParameter 对大型 CLR UDT 的支持  
 **SQLBindParameter** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLBindParameter 函数](../../odbc/reference/syntax/sqlbindparameter-function.md)  
  
