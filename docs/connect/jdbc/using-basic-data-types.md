---
title: 使用基本 JDBC 数据类型
description: Microsoft JDBC Driver for SQL Server 使用基本 JDBC 数据类型，将 SQL Server 数据类型转换为 Java 可以理解的格式。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: d7044936-5b8c-4def-858c-28a11ef70a97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1c6f764fffca3408380da9c96227eb9d7bb4457c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195265"
---
# <a name="using-basic-data-types"></a>使用基本数据类型

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 使用 JDBC 基本数据类型将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型转换为 Java 编程语言能够理解的格式，反之亦然。 JDBC 驱动程序支持 JDBC 4.0 API，其中包括 SQLXML  数据类型和区域 (Unicode) 数据类型，如 NCHAR  、NVARCHAR  、LONGNVARCHAR  和 NCLOB  。  
  
## <a name="data-type-mappings"></a>数据类型映射

下表列出了基本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、JDBC 和 Java 编程语言数据类型之间的默认映射：  
  
| SQL Server 类型   | JDBC 类型 (java.sql.Types)                        | Java 语言类型          |
| ------------------ | -------------------------------------------------- | ---------------------------- |
| bigint             | BIGINT                                             | long                         |
| binary             | BINARY                                             | byte[]                       |
| bit                | BIT                                                | boolean                      |
| char               | CHAR                                               | 字符串                       |
| date               | DATE                                               | java.sql.Date                |
| datetime<sup>3</sup>          | TIMESTAMP                               | java.sql.Timestamp           |
| datetime2          | TIMESTAMP                                          | java.sql.Timestamp           |
| datetimeoffset<sup>2</sup> | microsoft.sql.Types.DATETIMEOFFSET         | microsoft.sql.DateTimeOffset |
| Decimal            | DECIMAL                                            | java.math.BigDecimal         |
| FLOAT              | DOUBLE                                             | double                       |
| image              | LONGVARBINARY                                      | byte[]                       |
| int                | INTEGER                                            | int                          |
| money              | DECIMAL                                            | java.math.BigDecimal         |
| nchar              | CHAR<br /><br /> NCHAR (Java SE 6.0)               | 字符串                       |
| ntext              | LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0) | 字符串                       |
| numeric            | NUMERIC                                            | java.math.BigDecimal         |
| nvarchar           | VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)         | 字符串                       |
| nvarchar(max)      | VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)         | 字符串                       |
| real               | real                                               | FLOAT                        |
| smalldatetime      | TIMESTAMP                                          | java.sql.Timestamp           |
| smallint           | SMALLINT                                           | short                        |
| smallmoney         | DECIMAL                                            | java.math.BigDecimal         |
| text               | LONGVARCHAR                                        | 字符串                       |
| time               | TIME<sup>1</sup>                                   | java.sql.Time<sup>1</sup>            |
| timestamp          | BINARY                                             | byte[]                       |
| tinyint            | TINYINT                                            | short                        |
| udt                | VARBINARY                                          | byte[]                       |
| uniqueidentifier   | CHAR                                               | 字符串                       |
| varbinary          | VARBINARY                                          | byte[]                       |
| varbinary(max)     | VARBINARY                                          | byte[]                       |
| varchar            | VARCHAR                                            | 字符串                       |
| varchar(max)       | VARCHAR                                            | 字符串                       |
| xml                | LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0) | 字符串<br /><br /> SQLXML    |
| sqlvariant         | microsoft.sql.Types.SQL_VARIANT                    | 对象                       |
| geometry           | VARBINARY                                          | byte[]                       |
| geography          | VARBINARY                                          | byte[]                       |
  
<sup>1</sup> 若要将 java.sql.Time 与时间 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型一起使用，必须将 sendTimeAsDatetime 连接属性设置为 false。  
  
<sup>2</sup> 可以编程方式使用 [DateTimeOffset 类](reference/datetimeoffset-class.md)访问 datetimeoffset 的值。  
  
<sup>3</sup> 请注意，从 SQL Server 2016 开始，java.sql.Timestamp 值不能再用于比较 datetime 列中的值。 此限制是由于以不同方式将 datetime 转换为 datetime2 的服务器端更改，从而导致不可相等的值。 此问题的解决方法是将 datetime 列更改为 datetime2(3)，使用 String 而不是 java.sql.Timestamp，或将数据库兼容级别更改为 120 或更低。
  
以下几部分提供了如何使用 JDBC 驱动程序和基本数据类型的示例。 有关如何在 Java 应用程序中使用基本数据类型的更多详细示例，请参阅[基本数据类型示例](basic-data-types-sample.md)。  
  
## <a name="retrieving-data-as-a-string"></a>以字符串格式检索数据

如果必须从映射到任意 JDBC 基本数据类型的数据源检索数据，并以字符串的格式查看这些数据，或者如果不需要强类型的数据，则可以使用 [SQLServerResultSet](reference/sqlserverresultset-class.md) 类的 [getString](reference/getstring-method-sqlserverresultset.md) 方法，如下所示：  
  
[!code[JDBC#UsingBasicDataTypes1](codesnippet/Java/using-basic-data-types_1.java)]  
  
## <a name="retrieving-data-by-data-type"></a>按数据类型检索数据

如果必须从数据源检索数据，并且已知检索的数据类型，则应使用 SQLServerResultSet 类的任一 get\<Type> 方法（也称为 Getter 方法）。 通过 get\<Type> 方法，可以使用列名或列索引，如下所示：  
  
[!code[JDBC#UsingBasicDataTypes2](codesnippet/Java/using-basic-data-types_2.java)]  
  
> [!NOTE]  
> 使用确定位数的 getUnicodeStream 和 getBigDecimal 方法已遭弃用，不受 JDBC 驱动程序支持。

## <a name="updating-data-by-data-type"></a>按数据类型更新数据

如果必须更新数据源中字段的值，请使用 SQLServerResultSet 类的一种 update\<Type> 方法。 在下面的示例中，[updateInt](reference/updateint-method-sqlserverresultset.md) 方法与 [updateRow](reference/updaterow-method-sqlserverresultset.md) 方法结合使用，用于更新数据源中的数据：  
  
[!code[JDBC#UsingBasicDataTypes3](codesnippet/Java/using-basic-data-types_3.java)]  
  
> [!NOTE]  
> JDBC Driver 无法更新列名长度超过 127 个字符的 SQL Server 列。 如果尝试更新名称长度超过 127 个字符的列，将引发异常。  
  
## <a name="updating-data-by-parameterized-query"></a>通过参数化查询来更新数据

如果必须通过使用参数化查询来更新数据源中的数据，可以使用 [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md) 类的任一 set\<Type> 方法（也称为 setter 方法）来设置参数的数据类型。 在下面的示例中，[prepareStatement](reference/preparestatement-method-sqlserverconnection.md) 方法用于预编译参数化查询，然后在调用 [executeUpdate](reference/executeupdate-method.md) 方法前，使用 [setString](reference/setstring-method-sqlserverpreparedstatement.md) 方法来设置参数的字符串值。  
  
[!code[JDBC#UsingBasicDataTypes4](codesnippet/Java/using-basic-data-types_4.java)]  
  
若要详细了解参数化查询，请参阅[使用包含参数的 SQL 语句](using-an-sql-statement-with-parameters.md)。  

## <a name="passing-parameters-to-a-stored-procedure"></a>向存储过程传递参数

如果必须向存储过程传递类型参数，则可使用 [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) 类的任一 set\<Type> 方法通过索引或名称来设置此参数。 在下面的示例中，[prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) 方法用于设置对存储过程的调用，然后在调用 [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) 方法之前，使用 [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) 方法设置调用的参数。  
  
[!code[JDBC#UsingBasicDataTypes5](codesnippet/Java/using-basic-data-types_5.java)]  
  
> [!NOTE]  
> 在此实例中，将返回一个结果集，包含此存储过程的运行结果。

若要详细了解结合使用 JDBC 驱动程序与存储过程和输入参数，请参阅[使用包含输入参数的存储过程](using-a-stored-procedure-with-input-parameters.md)。  

## <a name="retrieving-parameters-from-a-stored-procedure"></a>从存储过程检索参数

如果必须从存储过程检索参数，则必须首先使用 SQLServerCallableStatement 类的 [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) 方法通过名称或索引注册一个输出参数，然后在调用存储过程后，将返回的输出参数分配给合适的变量。 在下面的示例中，使用 prepareCall 方法设置对存储过程的调用，使用 registerOutParameter 方法设置输出参数，然后在调用 executeQuery 方法前，使用 [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) 方法设置调用的参数。 使用 [getShort](../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md) 方法来检索存储过程的输出参数返回的值。  
  
[!code[JDBC#UsingBasicDataTypes6](../../connect/jdbc/codesnippet/Java/using-basic-data-types_6.java)]  
  
> [!NOTE]  
> 除返回的输出参数外，还可能返回一个结果集，包含此存储过程的运行结果。  
  
若要详细了解结合使用 JDBC 驱动程序与存储过程和输出参数，请参阅[使用包含输出参数的存储过程](../../connect/jdbc/using-a-stored-procedure-with-output-parameters.md)。  

## <a name="see-also"></a>另请参阅

[了解 JDBC 驱动程序数据类型](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
