---
description: 数据类型转换（数据库引擎）
title: 数据类型转换（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- converting data types [SQL Server]
- CONVERT function
- data types [SQL Server], converting
- implicit data type conversions
- explicit data type conversions [SQL Server]
- converting data types [SQL Server], about converting data types
ms.assetid: ffacf45e-a488-48d0-9bb0-dcc7fd365299
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f879b6e749f3797a51970708be59b3b68920cb26
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474238"
---
# <a name="data-type-conversion-database-engine"></a>数据类型转换（数据库引擎）
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在以下情况下，可以转换数据类型：
-   当一个对象的数据移到另一个对象，或两个对象之间的数据进行比较或组合时，数据可能必须从一个对象的数据类型转换为另一个对象的数据类型。  
-   将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 结果列、返回代码或输出参数中的数据移到某个程序变量中时，必须将这些数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型转换成该变量的数据类型。  
  
在应用程序变量与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 结果集列、返回代码、参数或参数标记之间进行转换时，支持的数据类型转换由数据库 API 定义。
  
## <a name="implicit-and-explicit-conversion"></a>隐式和显式转换
可以隐式或显式转换数据类型。
  
隐式转换对用户不可见。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会自动将数据从一种数据类型转换为另一种数据类型。 例如，将 smallint 与 int 进行比较时，在比较之前，smallint 会被隐式转换为 int。
  
GETDATE() 隐式转换为日期样式 0。 SYSDATETIME() 隐式转换为日期样式 21。
  
显式转换使用 CAST 或 CONVERT 函数。
  
[CAST 和 CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) 函数可将值（局部变量、列或其他表达式）从一种数据类型转换为另一种数据类型。 例如，以下 `CAST` 函数可将数值 `$157.27` 转换为字符串 `'157.27'`：
  
```sql
CAST ( $157.27 AS VARCHAR(10) )  
```  
  
如果希望 [!INCLUDE[tsql](../../includes/tsql-md.md)] 程序代码符合 ISO 标准，请使用 CAST 而不要使用 CONVERT。 如果要利用 CONVERT 中的样式功能，请使用 CONVERT 而不要使用 CAST。
  
以下图例显示了可对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统提供的数据类型执行的所有显式和隐式数据类型转换。 这些包括 xml、bigint 和sql_variant。 不存在对 sql_variant 数据类型的赋值进行的隐式转换，但是存在转换为 sql_variant 的隐式转换 。
  
![数据类型转换表](../../t-sql/data-types/media/lrdatahd.png "数据类型转换表")

虽然上面的图表说明了 SQL Server 中允许的所有显式和隐式转换，但并未指出转换的结果数据类型。 当 SQL Server 执行显式转换时，语句本身会确定结果数据类型。 对于隐式转换，赋值语句（例如设置变量的值或在列中插入值）将产生由变量声明或列定义所定义的数据类型。 对于比较运算符或其他表达式，结果数据类型取决于数据类型优先级的规则。

例如，以下脚本定义一个类型为 `varchar` 的变量，将 `int` 类型值赋给该变量，然后选择该变量与字符串的串联。

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.'
```

`1` 的 `int` 值会转换为 `varchar`，因此 `SELECT` 语句返回值 `1 is a string.`。

下面的示例演示改为使用 `int` 变量的类似脚本：

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.'
```

在此例中，`SELECT` 语句会引发以下错误：

`Msg 245, Level 16, State 1, Line 3`
`Conversion failed when converting the varchar value ' is not a string.' to data type int.`

为了计算表达式 `@notastring + ' is not a string.'`，SQL Server 先遵循数据类型优先级的规则来完成隐式转换，然后才能计算表达式的结果。 由于 `int` 的优先级高于 `varchar`，因此 SQL Server 会尝试将字符串转换为整数，但是会失败，因为此字符串无法转换为整数。 如果表达式提供可以转换的字符串，则该语句会成功，如以下示例所示：

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

在此例中，字符串 `1` 可以转换为整数值 `1`，因而此 `SELECT` 语句会返回值 `2`。 请注意，当提供的数据类型为整数时，`+` 运算符会成为加法而不是串联。

## <a name="data-type-conversion-behaviors"></a>数据类型转换行为

将一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象的数据类型转换为另一种数据类型时，不支持某些隐式和显式数据类型转换。 例如，nchar 值无法被转换为 image 值。 nchar 只能显式转换为 binary，而不支持隐式转换为 binary。 但是，nchar 既可以显式也可以隐式转换为 nvarchar。
  
以下各主题说明各对应数据类型展示的转换行为：
  
 - [binary 和 varbinary (Transact-SQL)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)  
 - [datetime2 (Transact-SQL)](../../t-sql/data-types/datetime2-transact-sql.md)  
 - [money 和 smallmoney (Transact-SQL)](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
 - [bit (Transact-SQL)](../../t-sql/data-types/bit-transact-sql.md)  
 - [datetimeoffset (Transact-SQL)](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
 - [smalldatetime (Transact-SQL)](../../t-sql/data-types/smalldatetime-transact-sql.md)  
 - [char 和 varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
 - [decimal 和 numeric (Transact-SQL)](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)  
 - [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md)  
 - [date (Transact-SQL)](../../t-sql/data-types/date-transact-sql.md)  
 - [float 和 real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md)  
 - [time (Transact-SQL)](../../t-sql/data-types/time-transact-sql.md)  
 - [datetime (Transact-SQL)](../../t-sql/data-types/datetime-transact-sql.md)  
 - [int、bigint、smallint 和 tinyint (Transact-SQL)](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
 - [uniqueidentifier (Transact-SQL)](../../t-sql/data-types/uniqueidentifier-transact-sql.md)  
  
###  <a name="converting-data-types-by-using-ole-automation-stored-procedures"></a>使用 OLE 自动化存储过程转换数据类型  
由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 数据类型，而 OLE 自动化使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 数据类型，因此 OLE 自动化存储过程必须转换在两者之间传递的数据。
  
下表说明了从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 的数据类型转换。
  
|SQL Server 数据类型|Visual Basic 数据类型|  
|--------------------------|----------------------------|  
|**char**、**varchar**、**text**、**nvarchar**、**ntext**|**字符串**|  
|**decimal**、**numeric**|**字符串**|  
|**bit**|**布尔值**|  
|**binary**、**varbinary**、**image**|一维 Byte() 数组|  
|**int**|**Long**|  
|**smallint**|**整数**|  
|**tinyint**|**Byte**|  
|**float**|**双精度**|  
|**real**|**单精度**|  
|**money**、 **smallmoney**|**货币**|  
|**datetime**、**smalldatetime**|**日期**|  
|设置为 NULL 的任何类型|Variant 设置为 Null|  
  
除了 binary、varbinary 和 image 值以外，所有单个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 值都被转换为单个 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 值。 这些值将被转换为 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 中的一维 Byte() 数组。 此数组的范围为 Byte( 0 to _length_ 1)，其中 length 是  binary、varbinary 或 image 值中的字节数[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
  
以下是从 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 数据类型到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型的转换。
  
|Visual Basic 数据类型|SQL Server 数据类型|  
|----------------------------|--------------------------|  
|**Long**、**Integer**、**Byte**、**Boolean**、**Object**|**int**|  
|**Double**、**Single**|**float**|  
|**货币**|**money**|  
|**日期**|**datetime**|  
|小于或等于 4000 个字符的 String|**varchar**/**nvarchar**|  
|大于 4000 个字符的 String|**text**/**ntext**|  
|小于或等于 8000 字节的一维 Byte() 数组|**varbinary**|  
|大于 8000 字节的一维 Byte() 数组|**图像**|  
  
## <a name="see-also"></a>另请参阅
[OLE 自动存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
[COLLATE (Transact-SQL)](../statements/collations.md)
  
