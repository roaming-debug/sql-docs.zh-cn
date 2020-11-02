---
title: CAST 和 CONVERT (Transact-SQL) | Microsoft Docs
description: CAST 和 CONVERT Transact-SQL 函数的参考。 这些函数将表达式由一种数据类型转换为另一种数据类型。
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CAST_TSQL
- CONVERT_TSQL
- CAST
- CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- automatic data type conversion
- varbinary data type
- CONVERT function
- data types [SQL Server], converting
- large value data types
- implicit data type conversions
- image data type, converting
- explicit data type conversions [SQL Server]
- binary [SQL Server], converting
- text data type, converting
- date and time [SQL Server], cast and convert
- truncating conversions
- converting data types [SQL Server], conversion functions
- time zones [SQL Server]
- roundtrip conversions
ms.assetid: a87d0850-c670-4720-9ad5-6f5a22343ea8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1472a8d7117fb45fc7f9d4adb04f690ed9662ea2
ms.sourcegitcommit: ead0b8c334d487a07e41256ce5d6acafa2d23c9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92412674"
---
# <a name="cast-and-convert-transact-sql"></a>CAST 和 CONVERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

这些函数将表达式由一种数据类型转换为另一种数据类型。  

## <a name="syntax"></a>语法  
  
```sqlsyntax 
-- CAST Syntax:  
CAST ( expression AS data_type [ ( length ) ] )  
  
-- CONVERT Syntax:  
CONVERT ( data_type [ ( length ) ] , expression [ , style ] )  
```  

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*expression*  
任何有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。
  
data_type  
目标数据类型。 这包括 xml、bigint 和sql_variant  。 不能使用别名数据类型。
  
*length*  
指定目标数据类型长度的可选整数，适用于允许用户指定长度的数据类型。 默认值为 30。
  
style  
指定 CONVERT 函数将如何转换 expression 的整数表达式。 对于 NULL 的样式值，则返回 NULL。 data_type 确定范围。 
  
## <a name="return-types"></a>返回类型
返回转换为 data_type 的 expression 。
  
## <a name="date-and-time-styles"></a>日期和时间样式  
对于日期或时间数据类型的 expression，style 可以具有下表所示的某个值 。 其他值作为 0 进行处理。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，在从日期和时间类型转换为 datetimeoffset 时支持的唯一样式是 0 或 1。 所有其他转换样式均返回错误 9809。
  
> [!NOTE]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用科威特算法来支持阿拉伯样式的日期格式。
  
|不带世纪数位 (yy) (<sup>1</sup>)|带世纪数位 (yyyy)|Standard|输入/输出 (<sup>3</sup>)|  
|---|---|--|---|
|-|0 或 100 (<sup>1,</sup><sup>2</sup>) |datetime 和 smalldatetime 的默认值|mon dd yyyy hh:miAM（或 PM）|  
|**1**|**101**|美国|  1 = mm/dd/yy<br /> 101 = mm/dd/yyyy|  
|**2**|**102**|ANSI|  2 = yy.mm.dd<br /> 102 = yyyy.mm.dd|  
|**3**|**103**|英国/法国|  3 = dd/mm/yy<br /> 103 = dd/mm/yyyy|  
|**4**|**104**|德语|  4 = dd.mm.yy<br /> 104 = dd.mm.yyyy|  
|**5**|**105**|意大利语|  5 = dd-mm-yy<br /> 105 = dd-mm-yyyy|  
|**6**|**106** <sup>(1)</sup>|-|  6 = dd mon yy<br /> 106 = dd mon yyyy|  
|**7**|**107** <sup>(1)</sup>|-|  7 = Mon dd, yy<br /> 107 = Mon dd, yyyy|  
|8 或 24 |**108**|-|hh:mi:ss|  
|-|9 或 109 (<sup>1,</sup><sup>2</sup>) |默认格式 + 毫秒|mon dd yyyy hh:mi:ss:mmmAM（或 PM）|  
|**10**|**110**|USA| 10 = mm-dd-yy<br /> 110 = mm-dd-yyyy|  
|**11**|**111**|日本| 11 = yy/mm/dd<br /> 111 = yyyy/mm/dd|  
|**12**|**112**|ISO| 12 = yymmdd<br /> 112 = yyyymmdd|  
|-|13 或 113 (<sup>1,</sup><sup>2</sup>) |欧洲默认格式 + 毫秒|dd mon yyyy hh:mi:ss:mmm（24 小时制）|  
|**14**|**114**|-|hh:mi:ss:mmm（24 小时制）|  
|-|20 或 120 (<sup>2</sup>) |ODBC 规范|yyyy-mm-dd hh:mi:ss（24 小时制）|  
|-|21、25 或 121   (<sup>2</sup>)|time、date、datetime2 和 datetimeoffset 的 ODBC 规范（带毫秒）默认值|yyyy-mm-dd hh:mi:ss.mmm（24 小时制）|  
|22|-|美国| mm/dd/yy hh:mi:ss AM（或 PM）|
|-|23|ISO8601|yyyy-mm-dd|
|-|**126** ( <sup>4</sup>)|ISO8601|yyyy-mm-ddThh:mi:ss.mmm（无空格）<br /><br /> **注意：** 毫秒 (mmm) 值为 0 时，不会显示毫秒小数部分的值。 例如，值“2012-11-07T18:26:20.000”显示为“2012-11-07T18:26:20”。| 
|-|**127** ( <sup>6, 7</sup>)|带时区 Z 的 ISO8601。|yyyy-MM-ddThh:mm:ss.fffZ（不带空格）<br /><br /> **注意：** 毫秒 (mmm) 值为 0 时，不会显示毫秒小数值。 例如，值“2012-11-07T18:26:20.000”显示为“2012-11-07T18:26:20”。|  
|-|**130** ( <sup>1,</sup><sup>2</sup>)|回历 (<sup>5</sup>)|dd mon yyyy hh:mi:ss:mmmAM<br /><br /> 在此样式中，mon 表示完整月份名称的多标记回历 unicode 表示形式。 该值在 SSMS 的默认 US 安装中不会正确呈现。|  
|-|**131** ( <sup>2</sup>)|回历 (<sup>5</sup>)|dd/mm/yyyy hh:mi:ss:mmmAM|  
  
<sup>1</sup> 这些样式值返回不确定的结果。 包括所有 (yy)（不带世纪数位）样式和一部分 (yyyy)（带世纪数位）样式。
  
<sup>2</sup> 默认值（0 或 100、9 或 109、13 或 113、20 或 120，23，以及 21、25 或 121）始终返回世纪位数 (yyyy)            。

<sup>3</sup> 转换为 datetime 时输入；转换为字符数据时输出。

<sup>4</sup> 为用于 XML 而设计。 对于从 datetime 或 smalldatetime 到字符数据的转换，请参阅上一个表，查看输出格式 。

<sup>5</sup> 回历是有多种变体的日历系统。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用科威特算法。

> [!IMPORTANT]
>  默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 基于截止年份 2049 年来解释两位数的年份。 这意味着 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将两位数年份 49 解释为 2049 年，将两位数年份 50 解释为 1950 年。 许多客户端应用程序（包括基于自动化对象的应用程序）都使用截止年份 2030 年。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供两位数年份截止配置选项来更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所使用的截止年份。 这允许对日期进行一致处理。 建议您指定四位数年份。

<sup>6</sup> 仅在从字符数据强制转换到 datetime 或 smalldatetime 时提供支持 。 仅表示日期或时间成分的字符数据强制转换为 datetime 或 smalldatetime 数据类型时，未指定的时间成分设置为 00:00:00.000，未指定的日期成分设置为 1900-01-01 。
  
<sup>7</sup> 使用可选的时区指示符 Z，可更容易地将具有时区信息的 XML datetime 值映射到没有时区的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] datetime 值  。 Z 指示时区 UTC-0。 \+ 或 - 方向的 HH:MM 偏移量则指示其他时区。 例如：`2006-12-12T23:45:12-08:00`。
  
将 smalldatetime 转换为字符数据时，包含秒或毫秒的样式将在这些位置上显示零。 从 datetime 或 smalldatetime 值转换时，可以使用合适的 char 或 varchar 数据类型长度截断不需要的日期部分   。
  
使用包含时间的样式将字符数据转换为 datetimeoffset 时，将在结果末尾追加时区偏移量。
  
## <a name="float-and-real-styles"></a>float 和 real 样式
对于 float 或 real 表达式，style 可能具有下表显示的值之一  。 其他值作为 0 进行处理。
  
|值|输出|  
|---|---|
|**0** （默认值）|最多包含 6 位。 根据需要使用科学记数法。|  
|**1**|始终为 8 位值。 始终使用科学记数法。|  
|**2**|始终为 16 位值。 始终使用科学记数法。|  
|**3**|始终为 17 位值。 用于无损转换。 使用此样式，可以保证每个不重复的 float 或 real 值转换为不重复的字符串。<br /><br /> **适用于：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|  
|**126, 128, 129**|为了保持向后兼容而包括在内，而以后的版本可能不推荐这些值。|  
  
## <a name="money-and-smallmoney-styles"></a>money 和 smallmoney 样式
对于 money 和 smallmoney 表达式，style 可能具有下表显示的值之一  。 其他值作为 0 进行处理。
  
|值|输出|  
|---|---|
|**0** （默认值）|小数点左侧每三位数字之间不以逗号分隔，小数点右侧取两位数<br /><br />示例：4235.98。|  
|**1**|小数点左侧每三位数字之间以逗号分隔，小数点右侧取两位数<br /><br />示例：3,510.92。|  
|**2**|小数点左侧每三位数字之间不以逗号分隔，小数点右侧取四位数<br /><br />示例：4235.9819。|  
|**126**|转换为 char(n) 或 varchar(n) 时，等同于样式 2|  
  
## <a name="xml-styles"></a>xml 样式
对于 xml 表达式，style 可能具有下表显示的值之一 。 其他值作为 0 进行处理。
  
|值|输出|  
|---|---|
|**0** （默认值）|使用默认的分析行为，即放弃无用的空格，且不允许使用内部 DTD 子集。<br /><br />**注意：** 转换为 xml 数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的无用空格处理方式不同于 XML 1.0。 有关详细信息，请参阅[创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)。|  
|**1**|保留无用空格。 此样式设置将默认 xml:space 处理设置为匹配 xml:space="preserve" 的行为 。|  
|**2**|启用有限的内部 DTD 子集处理。<br /><br /> 如果启用，则服务器可使用内部 DTD 子集提供的以下信息来执行非验证分析操作。<br /><br />   - 应用属性的默认值<br />   - 解析并展开内部实体引用<br />   - 检查 DTD 内容模型来确定语法的正确性<br /><br /> 分析器忽略外部 DTD 子集。 此外，它不会评估 XML 声明来查看 standalone 属性具有 yes 值还是 no 值  。 相反，它将 XML 实例解析为独立文档。|  
|**3**|保留无用空格，并启用有限的内部 DTD 子集处理。|  
  
## <a name="binary-styles"></a>二进制样式
对于 binary(n)、char(n)、varbinary(n) 或 varchar(n) 表达式，style 可能具有下表显示的值之一    。 表中没有列出样式值将返回错误。
  
|值|输出|  
|---|---|
|**0** （默认值）|将 ASCII 字符转换为二进制字节，或者将二进制字节转换为 ASCII 字符。 每个字符或字节按照 1:1 进行转换。<br /><br /> 对于二进制 data_type，则会在结果左侧添加字符 0x。|  
|**1** , **2**|对于二进制 data_type，则表达式必须为字符表达式。 expression 必须具有偶数数量的十六进制数字（0、1、2、3、4、5、6、7、8、9、A、B、C、D、E、F、a、b、c、d、e、f）。 如果将 style 设置为 1，则表达式必须将 0x 作为前两个字符。 如果表达式中包含的字符数为奇数或者包含任何无效的字符，则会引发错误。<br /><br /> 如果转换后的表达式长度大于 data_type 长度，则会在右侧截断结果。<br /><br /> 如果固定长度 data_types 大于转换后的结果，则在结果右侧添加零。<br /><br /> 字符类型的 data_type 要求二进制表达式。 每个二进制字符均转换为两个十六进制字符。 如果转换后的表达式长度大于 data_type 长度，则会在右侧将其截断。<br /><br /> 对于固定大小的字符类型 data_type ，并且转换后的结果长度小于其 data_type 长度，则会在转换后的表达式右侧添加空格，以使十六进制数字的个数保持为偶数 。<br /><br /> 对于 style 2，将在转换后的结果左侧添加字符 0x。|  
  
## <a name="implicit-conversions"></a>隐式转换
隐式转换不需要规范 CAST 函数或 CONVERT 函数。 显示转换需要规范 CAST 函数或 CONVERT 函数。 以下图例显示了可对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统提供的数据类型执行的所有显式和隐式数据类型转换。 这些包括 bigint、sql_variant 和 xml  。 不存在对 sql_variant 数据类型的赋值进行的隐式转换，但是存在转换为 sql_variant 的隐式转换 。
  
> [!TIP]  
> 可从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=35834)将此图表下载为 PNG 文件。  
  
![数据类型转换表](../../t-sql/data-types/media/lrdatahd.png "数据类型转换表")
  
虽然上面的图表说明了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中允许的所有显式和隐式转换，但转换生成的数据类型取决于执行的操作：

-  对于显式转换，语句本身会确定生成的数据类型。    
-  对于隐式转换，赋值语句（例如设置变量的值或在列中插入值）将生成由变量声明或列定义所定义的数据类型。    
-  对于比较运算符或其他表达式，生成的数据类型取决于[数据类型优先级](../../t-sql/data-types/data-type-precedence-transact-sql.md)的规则。

> [!TIP]
> 有关[转换中数据类型优先级的影响](#precedence-example)的实际示例可在本部分后面看到。

在 datetimeoffset 与字符类型 char、nchar、nvarchar 和 varchar 之间转换时，转换后的时区偏移量部分的 HH 和 MM 都应始终为两个数字    。 例如 -08:00。
  
> [!NOTE]   
> 因为 Unicode 数据始终使用偶数个字节，所以在 binary 或 varbinary 与支持 Unicode 的数据类型之间进行转换时要特别小心 。 例如，以下转换不返回十六进制值 41。 而是返回十六进制值 4100：`SELECT CAST(CAST(0x41 AS nvarchar) AS varbinary)`。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。 
  
## <a name="large-value-data-types"></a>大值数据类型
大值数据类型具有与小值数据类型相同的隐式和显式转换行为 - 特别是 nvarchar、varbinary 和 varchar 数据类型  。 但是，请考虑以下原则：
-   从 image 转换到 varbinary(max) 以及从 varbinary(max) 转换到 image 属于隐式转换操作，同样的还有 text 与 varchar(max) 之间的转换和 ntext 与 nvarchar(max) 之间的转换     。  
-   从大值数据类型（如 varchar(max)）到小值数据类型（如 varchar）的转换是隐式转换，但如果大值的大小超过小值数据类型的指定长度，则产生截断 。  
-   从 nvarchar、varbinary 或 varchar 到其相应的大值数据类型的转换都是隐式转换  。  
-   从 sql_variant 数据类型到大值数据类型的转换是显式转换。  
-   大值数据类型不能转换为 sql_variant 数据类型。  
  
有关从 xml 数据类型进行转换的详细信息，请参阅[创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)。
  
## <a name="xml-data-type"></a>XML 数据类型
将 xml 数据类型显式或隐式转换为字符串或二进制数据类型时，xml 数据类型的内容将根据一组确定的规则进行序列化 。 有关这些规则的信息，请参阅[定义 XML 数据的序列化](../../relational-databases/xml/define-the-serialization-of-xml-data.md)。 有关从其他数据类型转换到 xml 数据类型对的信息，请参阅[创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)。
  
## <a name="text-and-image-data-types"></a>text 和 image 数据类型
text 和 image 数据类型不支持自动进行数据类型转换 。 可以显式将 text 数据转换为字符数据，将 image 数据转换为 binary 或 varbinary，但是最大长度为 8000 字节   。 如果试图进行不正确的转换（如将包含字母的字符表达式转换为 int），则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回错误消息。
  
## <a name="output-collation"></a>输出排序规则  
如果 CAST 或 CONVERT 输出字符串，并且接收字符串输入，则输出将与输入具有相同的排序规则和排序规则标签。 如果输入不是字符串，则输出采用数据库的默认排序规则以及强制默认的排序规则标签。 有关详细信息，请参阅[排序规则优先顺序 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)。
  
若要为输出分配不同的排序规则，请将 COLLATE 子句应用于 CAST 或 CONVERT 函数的结果表达式。 例如：
  
`SELECT CAST('abc' AS varchar(5)) COLLATE French_CS_AS`
  
## <a name="truncating-and-rounding-results"></a>截断结果和舍入结果
将字符或二进制表达式（binary、char、nchar、nvarchar、varbinary 或 varchar）转换为不同数据类型的表达式时，转换操作可能会截断输出数据，仅显示部分输出数据，或返回错误     。 如果结果太短而无法显示，则会发生这种情况。 会截断到 binary、char、nchar、nvarchar、varbinary 或 varchar 的转换，除了下表中显示的转换     。
  
|被转换的数据类型|转换为的数据类型|结果|  
|---|---|---|
|int、smallint 或 tinyint  |**char**|*|  
||**varchar**|*|  
||**nchar**|E|  
||**nvarchar**|E|  
|money、smallmoney、numeric、decimal、float 或 real     |**char**|E|  
||**varchar**|E|  
||**nchar**|E|  
||**nvarchar**|E|  
  
\* = 结果长度太短而无法显示<br /><br />E = 因为结果长度太短无法显示而返回错误。
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仅保证往返转换（也就是从原始数据类型进行转换后又返回原始数据类型的转换）在各版本间产生相同值。 以下示例显示的即是这样的往返转换：
  
```sql
DECLARE @myval DECIMAL (5, 2);  
SET @myval = 193.57;  
SELECT CAST(CAST(@myval AS VARBINARY(20)) AS DECIMAL(10,5));  
-- Or, using CONVERT  
SELECT CONVERT(DECIMAL(10,5), CONVERT(VARBINARY(20), @myval));  
```  
  
> [!WARNING]  
> 不要构造 binary 值然后将其转换为数值数据类型类别的一种数据类型。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不能保证 decimal 或 numeric 数据类型到 binary 的转换结果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的各个版本中都相同  。  
  
以下示例显示了由于太小而无法显示的结果表达式。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, SUBSTRING(p.Title, 1, 25) AS Title,
    CAST(e.SickLeaveHours AS char(1)) AS [Sick Leave]  
FROM HumanResources.Employee e JOIN Person.Person p 
    ON e.BusinessEntityID = p.BusinessEntityID  
WHERE NOT e.BusinessEntityID > 5;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```   
FirstName   LastName      Title   Sick Leave
---------   ------------- ------- --------`
Ken         Sanchez       NULL   *
Terri       Duffy         NULL   *
Roberto     Tamburello    NULL   *
Rob         Walters       NULL   *
Gail        Erickson      Ms.    *

(5 row(s) affected)  
```
  
转换小数位数不同的数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有时会返回截断后的结果值，有时会返回舍入值。 此表显示了此行为。
  
|源|目标|行为|  
|---|---|---|
|**numeric**|**numeric**|Round|  
|**numeric**|**int**|Truncate|  
|**numeric**|**money**|Round|  
|**money**|**int**|Round|  
|**money**|**numeric**|Round|  
|**float**|**int**|Truncate|  
|**float**|**numeric**|Round<br /><br /> 如果将使用科学记数法的 float 值转换为 decimal 或 numeric 时，转换会限制为只有 17 位精度的值  。 精度高于 17 的任何值都将舍入为零。|  
|**float**|**datetime**|Round|  
|**datetime**|**int**|Round|  
  
例如，10.6496 和 -10.6496 可能会被截断或者在转换到 int 或 numeric 类型期间被舍入 ：
  
```sql
SELECT  CAST(10.6496 AS INT) as trunc1,
        CAST(-10.6496 AS INT) as trunc2,
        CAST(10.6496 AS NUMERIC) as round1,
        CAST(-10.6496 AS NUMERIC) as round2;
 ```
查询结果显示在下表中：
 
|trunc1|trunc2|round1|round2|
|---|---|---|---|
|10|-10|11|-11|
 
在进行数据类型转换时，若目标数据类型的小数位数小于源数据类型的小数位数，则该值将进行舍入。 例如，此转换返回 `$10.3497`：
  
`SELECT CAST(10.3496847 AS money);`
  
将非数字 char、nchar、nvarchar 或 varchar 数据转换为 decimal、float、int、numeric 时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回错误消息       。 当空字符串 (" ") 转换为 numeric 或 decimal 时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 也返回错误 。
  
## <a name="certain-datetime-conversions-are-nondeterministic"></a>某些日期时间的转换具有不确定性

从 string 到 datetime 的转换为不确定性转换的样式如下所示：
  
- 低于 100 的所有样式<sup>1</sup>
- 106  
- 107
- 109
- 113
- 130  
  
<sup>1</sup> 样式 20 和 21 除外

有关详细信息，请参阅[文字日期字符串转换为日期值的不确定性转换](../data-types/nondeterministic-convert-date-literals.md)。

## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）
从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，使用增补字符 (SC) 排序规则时，从 nchar 或 nvarchar 到更小长度的 nchar 或 nvarchar 类型的 CAST 操作将不会在代理项对内截断   。 相反，该操作会在增补字符前面截断。 例如，以下代码段导致 `@x` 仅保留 `'ab'`。 没有足够的空间来保留增补字符。
  
```sql
DECLARE @x NVARCHAR(10) = 'ab' + NCHAR(0x10000);  
SELECT CAST (@x AS NVARCHAR(3));  
```  
  
在使用 SC 排序规则时，`CONVERT` 行为类似于 `CAST`。 有关详细信息，请参阅[排序规则和 Unicode 支持 - 补充字符](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters)。
  
## <a name="compatibility-support"></a>兼容性支持
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本中，对 time 或 datetime2 数据类型的 CAST 和 CONVERT 操作的默认样式为 121，当在计算列表达式中使用这些类型时除外 。 对于计算列，默认样式为 0。 当创建用于涉及自动参数化的查询中或约束定义中的计算列时，此行为会影响计算列。
  
[兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)为 110 或更高时，对 time 和 datetime2 数据类型的 CAST 和 CONVERT 操作的默认样式始终为 121 。 如果查询依赖旧行为，请使用低于 110 的兼容性级别或在受影响的查询中显式指定 0 样式。

|[兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)值|CAST 和 CONVERT 的默认样式<sup>1</sup>|计算列的默认样式|  
|------------|------------|------------|
|< 110|121|0|  
|> = 110|121|121|  

<sup>1</sup> 计算列除外

将数据库升级到兼容级别 110 或更高将不更改已存储到磁盘的用户数据。 您必须相应手动更正此数据。 例如，如果使用了 SELECT INTO 来从包含上述计算列表达式的源创建表，将存储数据（使用样式 0）而非存储计算列定义本身。 必须手动更新此数据，以匹配样式 121。
  
## <a name="examples"></a><a name="BKMK_examples"></a> 示例  
  
### <a name="a-using-both-cast-and-convert"></a>A. 同时使用 CAST 和 CONVERT  
每个示例检索标价的第一位是 `3` 的产品的名称，并将其 `ListPrice` 值转换为 `int`。
  
```sql
-- Use CAST  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CAST(ListPrice AS int) LIKE '33%';  
GO  
  
-- Use CONVERT.  
USE AdventureWorks2012;  
GO  
SELECT SUBSTRING(Name, 1, 30) AS ProductName, ListPrice  
FROM Production.Product  
WHERE CONVERT(int, ListPrice) LIKE '33%';  
GO  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] CAST 和 CONVERT 的示例结果集相同。 

```
ProductName                    ListPrice
------------------------------ ---------------------
LL Road Frame - Black, 58      337.22
LL Road Frame - Black, 60      337.22
LL Road Frame - Black, 62      337.22
LL Road Frame - Red, 44        337.22
LL Road Frame - Red, 48        337.22
LL Road Frame - Red, 52        337.22
LL Road Frame - Red, 58        337.22
LL Road Frame - Red, 60        337.22
LL Road Frame - Red, 62        337.22
LL Road Frame - Black, 44      337.22
LL Road Frame - Black, 48      337.22
LL Road Frame - Black, 52      337.22
Mountain-100 Black, 38         3374.99
Mountain-100 Black, 42         3374.99
Mountain-100 Black, 44         3374.99
Mountain-100 Black, 48         3374.99
HL Road Front Wheel            330.06
LL Touring Frame - Yellow, 62  333.42
LL Touring Frame - Blue, 50    333.42
LL Touring Frame - Blue, 54    333.42
LL Touring Frame - Blue, 58    333.42
LL Touring Frame - Blue, 62    333.42
LL Touring Frame - Yellow, 44  333.42
LL Touring Frame - Yellow, 50  333.42
LL Touring Frame - Yellow, 54  333.42
LL Touring Frame - Yellow, 58  333.42
LL Touring Frame - Blue, 44    333.42
HL Road Tire                   32.60

(28 rows affected)
```
  
### <a name="b-using-cast-with-arithmetic-operators"></a>B. 将 CAST 与算术运算符结合使用  
此示例将本年度截止到现在的全部销售额 (`Computed`) 除以佣金百分比 (`SalesYTD`)，从而得出单列计算结果 (`CommissionPCT`)。 此值舍入为最接近的整数，然后 CAST 为 `int` 数据类型。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT CAST(ROUND(SalesYTD/CommissionPCT, 0) AS INT) AS Computed  
FROM Sales.SalesPerson   
WHERE CommissionPCT != 0;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```  
Computed
------
379753754
346698349
257144242
176493899
281101272
0  
301872549
212623750
298948202
250784119
239246890
101664220
124511336
97688107

(14 row(s) affected)  
```  
  
### <a name="c-using-cast-to-concatenate"></a>C. 使用 CAST 进行连接  
此示例使用 CAST 连接非字符型表达式。 它使用 AdventureWorksDW 数据库。
  
```sql
SELECT 'The list price is ' + CAST(ListPrice AS VARCHAR(12)) AS ListPrice  
FROM dbo.DimProduct  
WHERE ListPrice BETWEEN 350.00 AND 400.00;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ListPrice
------------------------
The list price is 357.06
The list price is 364.09
The list price is 364.09
The list price is 364.09
The list price is 364.09  
```  
  
### <a name="d-using-cast-to-produce-more-readable-text"></a>D. 使用 CAST 生成可读性更高的文本  
此示例使用 SELECT 列表中的 CAST 将 `Name` 列转换为 char(10) 列。 它使用 AdventureWorksDW 数据库。
  
```sql
SELECT DISTINCT CAST(EnglishProductName AS CHAR(10)) AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE EnglishProductName LIKE 'Long-Sleeve Logo Jersey, M';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Name        ListPrice
----------  ---------
Long-Sleev  31.2437
Long-Sleev  32.4935
Long-Sleev  49.99  
```  
  
### <a name="e-using-cast-with-the-like-clause"></a>E. 将 CAST 与 LIKE 子句一起作用  
此示例将 `money` 列 `SalesYTD` 值转换为数据类型 `int`，然后转换为数据类型 `char(20)`，以便 `LIKE` 子句可以使用。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName, s.SalesYTD, s.BusinessEntityID  
FROM Person.Person AS p   
JOIN Sales.SalesPerson AS s   
    ON p.BusinessEntityID = s.BusinessEntityID  
WHERE CAST(CAST(s.SalesYTD AS INT) AS char(20)) LIKE '2%';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
FirstName        LastName            SalesYTD         BusinessEntityID
---------------- ------------------- ---------------- -------------
Tsvi             Reiter              2811012.7151      279
Syed             Abbas               219088.8836       288
Rachel           Valdez              2241204.0424      289

(3 row(s) affected)  
```
  
### <a name="f-using-convert-or-cast-with-typed-xml"></a>F. 使用 CONVERT 或 CAST 转换为类型化的 XML  
这些示例说明如何通过 [XML 数据类型和列 (SQL Server)](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md) 使用 CONVERT 将数据转换为类型化的 XML。
  
此示例将包含空格、文本和标记的字符串转换为类型化的 XML，并删除所有无用空格（节点之间的边界空格）：
  
```sql
SELECT CONVERT(XML, '<root><child/></root>')  
```  
  
此示例将包含空格、文本和标记的类似字符串转换为类型化的 XML，并保留无用空格（节点之间的边界空格）：
  
```sql
SELECT CONVERT(XML, '<root>          <child/>         </root>', 1)  
```  
  
此示例将包含空格、文本和标记的字符串转换为类型化的 XML：
  
```sql
SELECT CAST('<Name><FName>Carol</FName><LName>Elliot</LName></Name>'  AS XML)  
```  
  
有关更多示例，请参阅[创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)。
  
### <a name="g-using-cast-and-convert-with-datetime-data"></a>G. 对 datetime 数据使用 CAST 和 CONVERT  
从 `GETDATE()` 值开始，此示例显示当前日期和时间，使用 `CAST` 将当前日期和时间更改为字符数据类型，然后使用 `CONVERT` 以 `ISO 8601` 格式显示日期和时间。
  
```sql
SELECT   
   GETDATE() AS UnconvertedDateTime,  
   CAST(GETDATE() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(nvarchar(30), GETDATE(), 126) AS UsingConvertTo_ISO8601  ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast              UsingConvertTo_ISO8601
----------------------- ---------------------- ------------------------------
2006-04-18 09:58:04.570 Apr 18 2006  9:58AM    2006-04-18T09:58:04.570

(1 row(s) affected)  
```
  
此示例与上述示例几乎完全相反。 该示例将日期和时间显示为字符数据，使用 `CAST` 将字符数据更改为 `datetime` 数据类型，然后使用 `CONVERT` 将字符数据更改为 `datetime` 数据类型。
  
```sql
SELECT   
   '2006-04-25T15:50:59.997' AS UnconvertedText,  
   CAST('2006-04-25T15:50:59.997' AS datetime) AS UsingCast,  
   CONVERT(datetime, '2006-04-25T15:50:59.997', 126) AS UsingConvertFrom_ISO8601 ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2006-04-25T15:50:59.997 2006-04-25 15:50:59.997 2006-04-25 15:50:59.997

(1 row(s) affected)  
```
  
### <a name="h-using-convert-with-binary-and-character-data"></a>H. 使用 CONVERT 处理二进制和字符数据  
这些示例使用不同样式显示二进制和字符数据转换的结果。
  
```sql
--Convert the binary value 0x4E616d65 to a character value.  
SELECT CONVERT(CHAR(8), 0x4E616d65, 0) AS [Style 0, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, binary to character
----------------------------
Name  

(1 row(s) affected)  
```
 
此示例显示 Style 1 可以强制截断结果。 结果集中的字符 0x 强制实施截断。  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 1) AS [Style 1, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, binary to character
------------------------------
0x4E616D

(1 row(s) affected)  
```  
 
该例显示 Style 2 不截断结果，因为结果中不包括字符 0x。  
```sql  
SELECT CONVERT(CHAR(8), 0x4E616d65, 2) AS [Style 2, binary to character];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, binary to character
------------------------------
4E616D65

(1 row(s) affected)  
```
  
将字符值“Name”转换为二进制值。  
```sql
SELECT CONVERT(BINARY(8), 'Name', 0) AS [Style 0, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 0, character to binary
----------------------------
0x4E616D6500000000

(1 row(s) affected)  
```
  
```sql
SELECT CONVERT(BINARY(4), '0x4E616D65', 1) AS [Style 1, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 1, character to binary
---------------------------- 
0x4E616D65

(1 row(s) affected)  
```  

```sql
SELECT CONVERT(BINARY(4), '4E616D65', 2) AS [Style 2, character to binary];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Style 2, character to binary  
----------------------------------  
0x4E616D65

(1 row(s) affected)  
```  
  
### <a name="i-converting-date-and-time-data-types"></a>I. 转换日期和时间数据类型  
此示例显示了日期、时间及日期/时间数据类型的转换。
  
```sql
DECLARE @d1 DATE, @t1 TIME, @dt1 DATETIME;  
SET @d1 = GETDATE();  
SET @t1 = GETDATE();  
SET @dt1 = GETDATE();  
SET @d1 = GETDATE();  
-- When converting date to datetime the minutes portion becomes zero.  
SELECT @d1 AS [DATE], CAST (@d1 AS DATETIME) AS [date as datetime];  
-- When converting time to datetime the date portion becomes zero   
-- which converts to January 1, 1900.  
SELECT @t1 AS [TIME], CAST (@t1 AS DATETIME) AS [time as datetime];  
-- When converting datetime to date or time non-applicable portion is dropped.  
SELECT @dt1 AS [DATETIME], CAST (@dt1 AS DATE) AS [datetime as date], 
   CAST (@dt1 AS TIME) AS [datetime as time];  
```  

### <a name="j-using-convert-with-datetime-data-in-different-formats"></a>J. 使用 CONVERT 处理不同格式的 datetime 数据  
从 `GETDATE()` 值开始，此示例使用`CONVERT` 显示本文[日期和时间样式](#date-and-time-styles)部分的所有日期和时间样式。

|格式编号|示例查询|示例结果|
|--------|------------------------------------|------------------|
|0|`SELECT CONVERT(NVARCHAR, GETDATE(), 0)`|Aug 23 2019  1:39PM|
|1|`SELECT CONVERT(NVARCHAR, GETDATE(), 1)`|08/23/19|
|2|`SELECT CONVERT(NVARCHAR, GETDATE(), 2)`|19.08.23|
|3|`SELECT CONVERT(NVARCHAR, GETDATE(), 3)`|23/08/19|
|4|`SELECT CONVERT(NVARCHAR, GETDATE(), 4)`|23.08.19|
|5|`SELECT CONVERT(NVARCHAR, GETDATE(), 5)`|23-08-19|
|6|`SELECT CONVERT(NVARCHAR, GETDATE(), 6)`|23 Aug 19|
|7|`SELECT CONVERT(NVARCHAR, GETDATE(), 7)`|Aug 23, 19|
|8、24 或 108|`SELECT CONVERT(NVARCHAR, GETDATE(), 8)`|13:39:17|
|9 或 109|`SELECT CONVERT(NVARCHAR, GETDATE(), 9)`|Aug 23 2019  1:39:17:090PM|
|10|`SELECT CONVERT(NVARCHAR, GETDATE(), 10)`|08-23-19|
|11|`SELECT CONVERT(NVARCHAR, GETDATE(), 11)`|19/08/23|
|12|`SELECT CONVERT(NVARCHAR, GETDATE(), 12)`|190823|
|13 或 113|`SELECT CONVERT(NVARCHAR, GETDATE(), 13)`|23 Aug 2019 13:39:17:090|
|14 或 114|`SELECT CONVERT(NVARCHAR, GETDATE(), 14)`|13:39:17:090|
|20 或 120|`SELECT CONVERT(NVARCHAR, GETDATE(), 20)`|2019-08-23 13:39:17|
|21、25 或 121|`SELECT CONVERT(NVARCHAR, GETDATE(), 21)`|2019-08-23 13:39:17.090|
|22|`SELECT CONVERT(NVARCHAR, GETDATE(), 22)`|08/23/19  1:39:17 PM|
|23|`SELECT CONVERT(NVARCHAR, GETDATE(), 23)`|2019-08-23|
|101|`SELECT CONVERT(NVARCHAR, GETDATE(), 101)`|08/23/2019|
|102|`SELECT CONVERT(NVARCHAR, GETDATE(), 102)`|2019.08.23|
|103|`SELECT CONVERT(NVARCHAR, GETDATE(), 103)`|23/08/2019|
|104|`SELECT CONVERT(NVARCHAR, GETDATE(), 104)`|23.08.2019|
|105|`SELECT CONVERT(NVARCHAR, GETDATE(), 105)`|23-08-2019|
|106|`SELECT CONVERT(NVARCHAR, GETDATE(), 106)`|23 Aug 2019|
|107|`SELECT CONVERT(NVARCHAR, GETDATE(), 107)`|Aug 23, 2019|
|110|`SELECT CONVERT(NVARCHAR, GETDATE(), 110)`|08-23-2019|
|111|`SELECT CONVERT(NVARCHAR, GETDATE(), 111)`|2019/08/23|
|112|`SELECT CONVERT(NVARCHAR, GETDATE(), 112)`|20190823|
|113|`SELECT CONVERT(NVARCHAR, GETDATE(), 113)`|23 Aug 2019 13:39:17.090|
|120|`SELECT CONVERT(NVARCHAR, GETDATE(), 120)`|2019-08-23 13:39:17|
|121|`SELECT CONVERT(NVARCHAR, GETDATE(), 121)`|2019-08-23 13:39:17.090|
|126|`SELECT CONVERT(NVARCHAR, GETDATE(), 126)`|2019-08-23T13:39:17.090|
|127|`SELECT CONVERT(NVARCHAR, GETDATE(), 127)`|2019-08-23T13:39:17.090|
|130|`SELECT CONVERT(NVARCHAR, GETDATE(), 130)`|22 ذو الحجة 1440  1:39:17.090P|
|131|`SELECT CONVERT(NVARCHAR, GETDATE(), 131)`|22/12/1440  1:39:17.090PM|

### <a name="k-effects-of-data-type-precedence-in-allowed-conversions"></a><a name="precedence-example"></a> K. 允许的转换中数据类型优先级的影响  
以下示例定义一个类型为 VARCHAR 的变量，将整数值赋给该变量，然后选择该变量与字符串的串联。

```sql
DECLARE @string VARCHAR(10);
SET @string = 1;
SELECT @string + ' is a string.' AS Result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Result
-----------------------
1 is a string.
```  

int 值 1 已转换为 VARCHAR。

此示例显示了一个类似的查询，但它使用的是 int 变量：

```sql
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + ' is not a string.' AS Result
```

在此例中，SELECT 语句会引发以下错误：

```
Msg 245, Level 16, State 1, Line 3
Conversion failed when converting the varchar value ' is not a string.' to data type int.
```

为了计算表达式 `@notastring + ' is not a string.'`，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要先遵循数据类型优先级的规则来完成隐式转换，然后才能计算表达式的结果。 由于 int 的优先级高于 VARCHAR，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会尝试将字符串转换为整数，但是会失败，因为此字符串无法转换为整数。 

如果我们提供可转换的字符串，则该语句将成功，如以下示例中所示：

```SQL
DECLARE @notastring INT;
SET @notastring = '1';
SELECT @notastring + '1'
```

在此例中，字符串 `'1'` 可以转换为整数值 1，因而此 SELECT 语句会返回值 2。 当提供的数据类型为整数时，+ 运算符会成为加法运算符而不是字符串串联。

## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="l-using-cast-and-convert"></a>L. 使用 CAST 和 CONVERT  
此示例检索标价的第一位是 `3` 的产品的名称，并将这些产品的 `ListPrice` 转换为 int。它使用 `AdventureWorksDW2016` 数据库。
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(ListPrice AS int) LIKE '3%';  
```  
  
此示例显示使用 CONVERT 而不是 CAST 的相同查询。 它使用 `AdventureWorksDW2016` 数据库。
  
```sql
SELECT EnglishProductName AS ProductName, ListPrice  
FROM dbo.DimProduct  
WHERE CONVERT(INT, ListPrice) LIKE '3%';  
```  
  
### <a name="m-using-cast-with-arithmetic-operators"></a>M. 将 CAST 与算术运算符结合使用  
此例通过产品单价 (`UnitPrice`) 除以折扣率 (`UnitPriceDiscountPct`) 计算单列值。 然后，此结果会舍入到最接近的整数，并最终转换为 `int` 数据类型。 此示例使用 `AdventureWorksDW2016` 数据库。
  
```sql
SELECT ProductKey, UnitPrice,UnitPriceDiscountPct,  
       CAST(ROUND (UnitPrice*UnitPriceDiscountPct,0) AS int) AS DiscountPrice  
FROM dbo.FactResellerSales  
WHERE SalesOrderNumber = 'SO47355'   
      AND UnitPriceDiscountPct > .02;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
ProductKey  UnitPrice  UnitPriceDiscountPct  DiscountPrice
----------  ---------  --------------------  -------------
323         430.6445   0.05                  22
213         18.5043    0.05                  1
456         37.4950    0.10                  4
456         37.4950    0.10                  4
216         18.5043    0.05                  1  
```  
  
### <a name="n-using-cast-with-the-like-clause"></a>N. 将 CAST 与 LIKE 子句一起作用  
此例将 money 列 `ListPrice` 转换为 int 类型，然后转换为 char(20) 类型，以便 LIKE 子句可以使用它  。 此示例使用 `AdventureWorksDW2016` 数据库。  
  
```sql
SELECT EnglishProductName AS Name, ListPrice  
FROM dbo.DimProduct  
WHERE CAST(CAST(ListPrice AS INT) AS CHAR(20)) LIKE '2%';  
```  
  
### <a name="o-using-cast-and-convert-with-datetime-data"></a>O. 对 datetime 数据使用 CAST 和 CONVERT  
此示例显示当前日期和时间，使用 CAST 将当前日期和时间更改为字符数据类型，最终使用 CONVERT 以 ISO 8601 格式显示日期和时间。 此示例使用 `AdventureWorksDW2016` 数据库。
  
```sql
SELECT TOP(1)  
   SYSDATETIME() AS UnconvertedDateTime,  
   CAST(SYSDATETIME() AS NVARCHAR(30)) AS UsingCast,  
   CONVERT(NVARCHAR(30), SYSDATETIME(), 126) AS UsingConvertTo_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedDateTime     UsingCast                     UsingConvertTo_ISO8601  
---------------------   ---------------------------   ---------------------------  
07/20/2010 1:44:31 PM   2010-07-20 13:44:31.5879025   2010-07-20T13:44:31.5879025  
```  
  
此示例与上述示例大致相反。 此示例将日期和时间显示为字符数据，使用 CAST 将字符数据更改为 datetime 数据类型，然后使用 CONVERT 将字符数据更改为 datetime 数据类型 。 此示例使用 `AdventureWorksDW2016` 数据库。
  
```sql
SELECT TOP(1)   
   '2010-07-25T13:50:38.544' AS UnconvertedText,  
CAST('2010-07-25T13:50:38.544' AS DATETIME) AS UsingCast,  
   CONVERT(DATETIME, '2010-07-25T13:50:38.544', 126) AS UsingConvertFrom_ISO8601  
FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
UnconvertedText         UsingCast               UsingConvertFrom_ISO8601
----------------------- ----------------------- ------------------------
2010-07-25T13:50:38.544 07/25/2010 1:50:38 PM   07/25/2010 1:50:38 PM  
```  

## <a name="see-also"></a>另请参阅
[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)       
[数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)     
[格式 (Transact-SQL)](../../t-sql/functions/format-transact-sql.md)      
[STR (Transact-SQL)](../../t-sql/functions/str-transact-sql.md)     
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)      
[System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)      
[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)      
[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)       
  
