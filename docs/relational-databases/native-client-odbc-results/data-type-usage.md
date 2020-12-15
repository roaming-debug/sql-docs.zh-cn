---
description: 数据类型用法
title: 数据类型用法 |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC data types
- ODBC data types, about ODBC data types
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC]
- SQL Server Native Client ODBC driver, data types
- data types [ODBC], about data types
ms.assetid: 4f19b0d6-94ac-4a98-a121-57d38787864c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ea1f3c6d629fa8b0a36f4b568afaf59bf73ada44
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473508"
---
# <a name="data-type-usage"></a>数据类型用法
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序并 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 强制使用以下数据类型。  
  
|数据类型|限制|  
|---------------|----------------|  
|日期文字|日期文字在 SQL_TYPE_TIMESTAMP 列中存储 ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** 或 **smalldatetime**) 数据类型时，时间值为12：00： 00.000 am。|  
|**money** 和 **smallmoney**|只有 **money** 和 **smallmoney** 数据类型的整数部分才有意义。 如果 SQL **money** 数据的小数部分在数据类型转换期间被截断，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序将返回一个警告，而不是错误。|  
|SQL_BINARY（可以为 Null）|连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 版和更早版本的实例时，如果 SQL_BINARY 列可以为 Null，则在数据源中存储的数据不使用零进行填充。 检索此类列中的数据时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序会在右侧使用零填充该驱动程序。 但是，在由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行的操作（比如串联）中所创建的数据则没有这样的填充。<br /><br /> 而且，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 或更早版本的实例中将数据放在这样的列中时，如果数据太长而无法放入列中，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将截断右侧数据。<br /><br /> 注意： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序支持连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 及更早版本。|  
|SQL_CHAR（截断）|在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 和更早版本的实例并将数据放入 SQL_CHAR 列中时，如果数据太长而无法放入该列中，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在没有警告的情况下从右侧截断它。<br /><br /> 注意： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序支持连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 及更早版本。|  
|SQL_CHAR（可以为 Null）|连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 和更早版本的实例时，如果 SQL_CHAR 列可以为 Null，则不以空白填充数据源中存储的数据。 检索此类列中的数据时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序会在右侧使用空白来填充它。 但是，在由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行的操作（比如串联）中所创建的数据则没有这样的填充。<br /><br /> 注意： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序支持连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 及更早版本。|  
|SQL_LONGVARBINARY、SQL_LONGVARCHAR、SQL_WLONGVARCHAR|当连接到实例6时，完全支持对 SQL_LONGVARBINARY、SQL_LONGVARCHAR 或 SQL_WLONGVARCHAR 数据类型 (使用 WHERE 子句) 的列的更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。*x* 及更高版本。 当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4.2 *x* 的实例时，S1000 错误 "部分插入/更新"。 文本或图像列的插入/更新失败”。<br /><br /> 注意： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序支持连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 及更早版本。|  
|字符串函数参数|字符串函数 *string_exp* 参数的数据类型必须为 SQL_CHAR 或 SQL_VARCHAR。 在字符串函数中不支持 SQL_LONG_VARCHAR 数据类型。 *Count* 参数必须小于或等于8000，因为 SQL_CHAR 和 SQL_VARCHAR 数据类型的最大长度限制为8000个字符。|  
|时间文字|当时间文本存储在 SQL_TIMESTAMP 列中时 ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型为 **datetime** 或 **smalldatetime**) ，其日期值为1900年1月1日。|  
|**timestamp**|只有空值才能手动插入到 **timestamp** 列。 但是，因为 **timestamp** 列由自动更新，所以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 覆盖 NULL 值。|  
|**tinyint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Tinyint** 数据类型无符号。 默认情况下， **tinyint** 列绑定到数据类型为的变量 SQL_C_UTINYINT。|  
|别名数据类型|当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4.2 *x* 的实例时，ODBC 驱动程序将 NULL 添加到未显式声明列的为 null 性的列定义。 因此，将忽略在别名数据类型的定义中存储的为 Null 性。<br /><br /> 当连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4.2 *x* 的实例时，如果别名数据类型的列的基本数据类型为 **char** 或 **binary** ，并且没有声明为 null 性的列，则将创建为数据类型 **varchar** 或 **varbinary**。 [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md)、 [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)和 [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) 返回 SQL_VARCHAR 或 SQL_VARBINARY 作为这些列的数据类型。 不对从这些列检索的数据进行填充。<br /><br /> 注意： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序支持连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 及更早版本。|  
|LONG 数据类型|*执行时数据* 参数会限制 SQL_LONGVARBINARY 和 SQL_LONGVARCHAR 数据类型。|  
|大值类型|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序将在接受或返回 ODBC SQL 数据类型的 api 中分别 **(** 最大 (类型) 、 **varbinary (max)** 和 **nvarchar) 最大 SQL_VARCHAR** 类型 SQL_VARBINARY SQL_WVARCHAR () 类型。|  
| (UDT 的用户定义类型) |UDT 列被映射为 SQL_SS_UDT。 如果使用 UDT 的 ToString() 或 ToXMLString() 方法或者通过 CAST/CONVERT 函数，在 SQL 语句中将 UDT 列显式映射到另一种类型，则在结果集中该列的类型将反映该列被转换成的实际类型。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序只能作为二进制文件绑定到 UDT 列。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仅支持在 SQL_SS_UDT 和 SQL_C_BINARY 数据类型之间的转换。|  
|XML|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会自动将 XML 转换为 Unicode 文本。 XML 类型将映射为 SQL_SS_XML。|  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;处理结果 ](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
