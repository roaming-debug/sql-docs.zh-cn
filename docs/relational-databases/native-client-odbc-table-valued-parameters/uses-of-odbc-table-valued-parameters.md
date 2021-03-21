---
description: ODBC 表值参数的用法
title: ODBC Table-Valued 参数的使用 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), scenarios
- ODBC, table-valued parameters
ms.assetid: f1b73932-4570-4a8a-baa0-0f229d9c32ee
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0694bb36991fd0d9b8d7c9b652f6d7eaa93998a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755017"
---
# <a name="uses-of-odbc-table-valued-parameters"></a>ODBC 表值参数的用法
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题介绍将表值参数用于 ODBC 时的主要用户情况：  
  
-   完全绑定多行缓冲区情况下的表值参数（在所有值都位于内存中时将数据作为 TVP 发送）  
  
-   行流式处理情况下的表值参数（使用执行时数据将数据作为 TVP 发送）  
  
-   从系统目录中检索表值参数元数据  
  
-   检索准备的语句的表值参数元数据  
  
## <a name="table-valued-parameter-with-fully-bound-multirow-buffers-send-data-as-a-tvp-with-all-values-in-memory"></a>完全绑定多行缓冲区情况下的表值参数（在所有值都位于内存中时将数据作为 TVP 发送）  
 用于完全绑定多行缓冲区时，所有参数值都位于内存中。 这是 OLTP 事务（举例来说）的典型情况，在这种情况下，可以将表值参数封装到单个存储过程中。 如果不使用表值参数，这通常需要动态生成复杂的多语句批处理，或者执行多个针对服务器的调用。  
  
 表值参数本身通过使用 [SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md) 以及其他参数进行绑定。 绑定所有参数后，应用程序将在每个表值参数上设置参数焦点属性 SQL_SOPT_SS_PARAM_FOCUS，并对表值参数的列调用 SQLBindParameter。  
  
 表值参数的服务器类型是特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新增类型 SQL_SS_TABLE。 SQL_SS_TABLE 的绑定 C 类型必须始终为 SQL_C_DEFAULT。 不为表值参数绑定参数传输任何数据；该参数用于传递表元数据，并且控制如何传递表值参数各构成列中的数据。  
  
 表值参数的长度设置为要发送到服务器的行数。 表值参数的 SQLBindParameter 的 *ColumnSize* 参数指定可以发送的最大行数;这是列缓冲区的数组大小。 *ParameterValuePtr* 是 SQLBindParameter 中表值参数的参数缓冲区。 *ParameterValuePtr* 及其关联的 *BufferLength* 用于在需要时传递表值参数的类型名称。 类型名称不是存储过程调用必需的，但却是 SQL 语句所必需的。  
  
 如果在对 SQLBindParameter 的调用上指定了表值参数类型名称，则它必须始终指定为 Unicode 值，即使是在作为 ANSI 应用程序生成的应用程序中。 使用 SQLSetDescField 指定表值参数类型名称时，可以使用符合生成应用程序的方式的文本。 ODBC 驱动程序管理器将执行任何所需的 Unicode 转换。  
  
 可以通过使用 SQLGetDescRec、SQLSetDescRec、SQLGetDescField 和 SQLSetDescField，单独并显式操作表值参数和表值参数列的元数据。 但在大多数情况下，重载 SQLBindParameter 通常更方便，无需显式说明符访问。 此方法与其他数据类型的 SQLBindParameter 的定义一致，不同之处在于对于表值参数，受影响的描述符字段略有不同。  
  
 有时，应用程序将表值参数用于动态 SQL，此时必须提供该表值参数的类型名称。 如果是这种情况，并且未在连接的当前默认架构中定义表值参数，则必须使用 SQLSetDescField 设置 SQL_CA_SS_TYPE_CATALOG_NAME 和 SQL_CA_SS_TYPE_SCHEMA_NAME。 由于表类型定义和表值参数必须位于同一数据库中，在应用程序使用表值参数时不能设置 SQL_CA_SS_TYPE_CATALOG_NAME。 否则，SQLSetDescField 将报告错误。  
  
 此方案的示例代码位于将 `demo_fixed_TVP_binding` [Table-Valued 参数 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md)的过程中。  
  
## <a name="table-valued-parameter-with-row-streaming-send-data-as-a-tvp-using-data-at-execution"></a>行流式处理情况下的表值参数（使用执行时数据将数据作为 TVP 发送）  
 在此情况下，应用程序在驱动程序请求行时向其提供行，并且这些行流向服务器。 这样就无需将所有行缓存在内存中。 这是大容量插入/更新时的典型情况。 表值参数提供了介于参数数组和大容量复制之间的某个性能点。 也就是说：表值参数可以像参数数组那样易于编程，但它们在服务器端提供更高的灵活性。  
  
 表值参数及其列按上一节“完全绑定多行缓冲区情况下的表值参数”所述进行绑定，但是表值参数本身的长度指示器设置为 SQL_DATA_AT_EXEC。 驱动程序以通常的方式对 SQLExecute 或 SQLExecuteDirect 进行响应，这是执行时数据参数的方式，即通过返回 SQL_NEED_DATA。 当驱动程序准备好接收表值参数的数据时，SQLParamData 将返回 SQLBindParameter 中 *ParameterValuePtr* 的值。  
  
 应用程序将 SQLPutData 用于表值参数，以指示表值参数构成列的数据可用性。 当为表值参数调用 SQLPutData 时， *DataPtr* 必须始终为 null，并且 *StrLen_or_Ind* 必须为0或小于或等于 (*ColumnSize* 参数为 SQLBindParameter) 指定的数组值的数组大小。 0 表示表值参数没有更多的行，驱动程序将继续处理下一个实际过程参数。 当 *StrLen_or_Ind* 不为0时，驱动程序将以与非表值参数绑定参数相同的方式处理表值参数构成列：每个表值参数列可以指定其实际数据长度（SQL_NULL_DATA），也可以在执行时通过其长度/指示器缓冲区指定数据。 如果要将字符或二进制值传递到块中，可以通过重复调用 SQLPutData 来传递表值参数列值。  
  
 处理完所有表值参数列后，驱动程序将返回到表值参数以处理更多的表值参数数据行。 因此，对于执行时数据表值参数，驱动程序不遵循通常的顺序扫描绑定参数的方式。 在 *StrLen_Or_IndPtr* 等于0的情况下调用 SQLPutData 之前，将轮询绑定表值参数，此时驱动程序将跳过表值参数列并移至下一个实际的存储过程参数。  当 SQLPutData 传递的指示器值大于或等于1时，驱动程序将按顺序处理表值参数列和行，直到它具有所有绑定行和列的值。 然后驱动程序返回到表值参数。 在从 SQLParamData 接收表值参数的标记和对表值参数调用 SQLPutData (hstmt，NULL，n) 时，应用程序必须为要传递给服务器的下一行或多行设置表值参数构成列数据和指示器缓冲区内容。  
  
 此方案的示例代码在 `demo_variable_TVP_binding` [使用 Table-Valued 参数 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md)中。  
  
## <a name="retrieving-table-valued-parameter-metadata-from-the-system-catalog"></a>从系统目录中检索表值参数元数据  
 当应用程序为具有表值参数参数的过程调用 SQLProcedureColumns 时，DATA_TYPE 作为 SQL_SS_TABLE 返回，TYPE_NAME 是表值参数的表类型的名称。 将两个附加列添加到 SQLProcedureColumns 返回的结果集： SS_TYPE_CATALOG_NAME 返回定义表值参数的表类型的目录的名称，SS_TYPE_SCHEMA_NAME 返回架构的名称，其中定义了表值参数的表类型。 与 ODBC 规范一致，SS_TYPE_CATALOG_NAME 和 SS_TYPE_SCHEMA_NAME 出现在以前版本的中添加的所有驱动程序特定列之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以及 odbc 本身所强制的所有列之后。  
  
 不仅将为表值参数填充这些新列，还将为 CLR 用户定义类型参数填充这些新列。 仍将填充 UDT 参数的现有架构和目录列，但是为数据类型提供所需的常见架构和目录列将简化未来的应用程序开发。 （请注意，XML 架构集合稍有不同，未包括在此更改中。）  
  
 应用程序使用 SQLTables 来确定表类型的名称，其方式与持久表、系统表和视图的名称相同。 引入了一个新的表类型 TABLE TYPE，该类型支持应用程序标识与表值参数关联的表类型。 表类型和常规表使用不同的命名空间。 这意味着可以对表类型和实际表使用相同的名称。 为处理同名情况，引入了一个新的语句属性 SQL_SOPT_SS_NAME_SCOPE。 此属性指定采用表名作为参数的 SQLTables 和其他目录函数是否应将表名解释为实际表的名称或表类型的名称。  
  
 应用程序使用 SQLColumns 来确定表类型的列，其方式与持久表的列相同，但必须首先将 SQL_SOPT_SS_NAME_SCOPE 设置为指示它使用的是表类型而不是实际表。 SQLPrimaryKeys 也可与表类型一起使用，SQL_SOPT_SS_NAME_SCOPE。  
  
 此方案的示例代码在 `demo_metadata_from_catalog_APIs` [使用 Table-Valued 参数 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md)中。  
  
## <a name="retrieving-table-valued-parameter-metadata-for-a-prepared-statement"></a>检索准备的语句的表值参数元数据  
 在此方案中，应用程序使用 SQLNumParameters 和 SQLDescribeParam 检索表值参数的元数据。  
  
 IPD 字段 SQL_CA_SS_TYPE_NAME 用于检索表值参数的类型名称。 IPD 字段 SQL_CA_SS_TYPE_SCHEMA_NAME 和 SQL_CA_SS_TYPE_CATALOG_NAME 分别用于检索表值参数的目录和架构。  
  
 表类型定义和表值参数必须位于同一数据库中。 如果应用程序在使用表值参数时设置 SQL_CA_SS_TYPE_CATALOG_NAME，SQLSetDescField 将报告错误。  
  
 SQL_CA_SS_TYPE_CATALOG_NAME 和 SQL_CA_SS_TYPE_SCHEMA_NAME 还可以用于检索与 CLR 用户定义类型参数关联的目录和架构。 SQL_CA_SS_TYPE_CATALOG_NAME 和 SQL_CA_SS_TYPE_SCHEMA_NAME 是 CLR UDT 类型的现有类型特定目录架构属性的替代属性。  
  
 在此方案中，应用程序使用 SQLColumns 检索表值参数的列元数据，因为 SQLDescribeParam 不返回表值参数列的元数据。  
  
 此用例的示例代码位于将 `demo_metadata_from_prepared_statement` [Table-Valued 参数 &#40;ODBC&#41;使用 ](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md)的例程中。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
