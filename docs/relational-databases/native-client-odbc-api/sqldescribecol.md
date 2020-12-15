---
description: SQLDescribeCol
title: SQLDescribeCol |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeCol function
ms.assetid: ffbf34c6-8268-434f-829a-82009a6cda59
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f85320fff9c44a1904ea3500b32b0d9305237b99
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465321"
---
# <a name="sqldescribecol"></a>SQLDescribeCol
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对于已执行的语句， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序不需要查询服务器来描述结果集中的列。 在这种情况下， **SQLDescribeCol** 不会导致服务器往返。 与 [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md)和 [SQLNumResultCols](../../relational-databases/native-client-odbc-api/sqlnumresultcols.md)类似，在已准备但未执行的语句上调用 **SQLDescribeCol** 将生成服务器往返。  
  
 当某个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或语句批处理返回多个结果行集时，由序号引用的某一列可能来自单独的表或引用结果集中完全不同的列。 应对每个集都调用 **SQLDescribeCol** 。 如果结果集有变化，应用程序应该在提取行结果之前重新绑定数据值。 有关处理多个结果集返回的详细信息，请参阅 [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)。  
  
 当已准备的一批 SQL 语句生成多个结果集时，只为第一个结果集报告列属性。  
  
 对于大值数据类型，在 *DataTypePtr* 中返回的值为 SQL_VARCHAR、SQL_VARBINARY 或 SQL_NVARCHAR。 *ColumnSizePtr* 中的值 SQL_SS_LENGTH_UNLIMITED 指示大小为 "无限制"。  
  
 从 "允许 SQLDescribeCol" 开始，数据库引擎中的改进 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 可获取预期结果的更准确说明。 更准确的结果可能与早期版本的 SQLDescribeCol 返回的值不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
## <a name="sqldescribecol-support-for-enhanced-date-and-time-features"></a>SQLDescribeCol 对日期和时间增强功能的支持  
 日期/时间类型返回以下值：  
  
| Attribute | *DataTypePtr* | *ColumnSizePtr* | *DecimalDigitsPtr* |  
| --------- | ------------- |---------------- | ------------------ |  
|datetime|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|date|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19、21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26、28..34|0..7|  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqldescribecol-support-for-large-clr-udts"></a>SQLDescribeCol 对大型 CLR UDT 的支持  
 **SQLDescribeCol** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLDescribeCol 函数](../../odbc/reference/syntax/sqldescribecol-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
