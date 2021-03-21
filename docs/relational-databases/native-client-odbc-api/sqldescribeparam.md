---
description: SQLDescribeParam
title: SQLDescribeParam |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeParam function
ms.assetid: 396e74b1-5d08-46dc-b404-2ef2003e4689
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06a173317a6539f7419976b4916effaaf93fca2c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753377"
---
# <a name="sqldescribeparam"></a>SQLDescribeParam
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  若要描述任何 SQL 语句的参数， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序会在对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 准备好的 ODBC 语句句柄调用 SQLDescribeParam 时生成并执行 SELECT 语句。 结果集的元数据确定预定义语句中的参数的特征。 SQLDescribeParam 可以返回 SQLExecute 或 SQLExecDirect 可能返回的任何错误代码。  
  
 从 "允许 SQLDescribeParam" 开始，数据库引擎中的改进 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 可获取预期结果的更准确说明。 更准确的结果可能与早期版本的 SQLDescribeParam 返回的值不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
 此外 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ， *ParameterSizePtr* 现在还返回一个值，该值与 [ODBC 规范](../../odbc/reference/appendixes/column-size.md)中定义的相应参数标记的列或表达式的大小的定义一致。 在以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 中， *ParameterSizePtr* 可以是类型的 **SQL_DESC_OCTET_LENGTH** 的相应值，也可以是为某一类型提供给 SQLBindParameter 的不相关的列大小值，应忽略该值 (**SQL_INTEGER**，例如) 。  
  
 在以下情况下，驱动程序不支持调用 SQLDescribeParam：  
  
-   在 SQLExecDirect 为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 包含 from 子句的 UPDATE 或 DELETE 语句之后。  
  
-   对于 HAVING 子句中包含参数或与 SUM 函数的结果相比较的任何 ODBC 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
-   对于依赖于包含参数的子查询的任何 ODBC 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
-   对于在比较和类似表达式中包含参数标记或包含限定谓词的 ODBC SQL 语句。  
  
-   对于其参数之一为函数参数的任何查询。  
  
-   当命令中有 (/* \* /) 的注释时 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。  
  
 在处理一批 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时，驱动程序也不支持在批处理中第一个语句后的语句中的参数标记上调用 SQLDescribeParam。  
  
 当描述已准备的存储过程的参数时，SQLDescribeParam 将使用系统存储过程 [sp_sproc_columns](../../relational-databases/system-stored-procedures/sp-sproc-columns-transact-sql.md) 来检索参数特征。 sp_sproc_columns 可以报告当前用户数据库中的存储过程的数据。 准备完全限定的存储过程名称可使 SQLDescribeParam 跨数据库执行。 例如，可以在任何数据库中准备和执行系统存储过程 [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) ，如下所示：  
  
```  
SQLPrepare(hstmt, "{call sp_who(?)}", SQL_NTS);  
```  
  
 成功完成准备后执行 SQLDescribeParam 将返回连接到任何数据库但 **master** 后的空行集。 如下所示的相同调用会导致 SQLDescribeParam 成功，而不考虑当前用户数据库：  
  
```  
SQLPrepare(hstmt, "{call master..sp_who(?)}", SQL_NTS);  
```  
  
 对于大值数据类型，在 *DataTypePtr* 中返回的值为 SQL_VARCHAR、SQL_VARBINARY 或 SQL_NVARCHAR。 若要指示大值数据类型参数的大小为 "无限制"，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序将 *ParameterSizePtr* 设置为0。 为标准 **varchar** 参数返回实际大小值。  
  
> [!NOTE]  
>  对于 SQL_VARCHAR、SQL_VARBINARY 或 SQL_WVARCHAR 参数，如果参数绑定有最大大小，则返回该参数的绑定大小，而非“无限制”。  
  
 若要绑定大小“无限制”的输入参数，必须使用执行时数据。 不能绑定 "无限制" 大小的输出参数 (没有用于从 output 参数流式处理数据的方法，例如 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) 对结果集) 。  
  
 对于输出参数，必须绑定一个缓冲区，如果值过大，则填充此缓冲区，并返回 SQL_SUCCESS_WITH_INFO 消息和“字符串数据；右端被截断”的警告。 随后将放弃截断的数据。  
  
## <a name="sqldescribeparam-and-table-valued-parameters"></a>SQLDescribeParam 和表值函数  
 应用程序可以使用 SQLDescribeParam 检索预定义语句的表值参数信息。 有关详细信息，请参阅 [预定义语句的表值参数元数据](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md)。  
  
 有关通常情况下的表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqldescribeparam-support-for-enhanced-date-and-time-features"></a>SQLDescribeParam 对日期和时间增强功能的支持  
 日期/时间类型返回以下值：  
  
| 特性 | *DataTypePtr* | *ParameterSizePtr* | *DecimalDigitsPtr* |  
| --------- | ------------- | ------------------ | ------------------ |  
|datetime|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|date|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19、21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26、28..34|0..7|  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqldescribeparam-support-for-large-clr-udts"></a>SQLDescribeParam 对大型 CLR UDT 的支持  
 **SQLDescribeParam** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLDescribeParam 函数](../../odbc/reference/syntax/sqldescribeparam-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
