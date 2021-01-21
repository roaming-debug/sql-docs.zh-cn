---
title: 表达式中的数据类型（报表生成器）| Microsoft Docs
description: 了解在报表生成器中数据类型如何表示不同种类的数据，以便能够存储和处理数据。
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 94fdf921-270c-4c12-87b3-46b1cc98fae5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0202703dec933871d6b6a611ff1139a68ee2c938
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595365"
---
# <a name="data-types-in-expressions-report-builder-and-ssrs"></a>表达式中的数据类型（报表生成器和 SSRS）
  数据类型表示不同种类的数据，以便能够有效地进行存储和处理。 典型的数据类型包括文本（也称为字符串）、带有小数位和不带小数位的数字、日期和时间以及图像。 报表中的值必须是报表定义语言 (RDL) 数据类型。 在报表中显示某个值时，您可以根据您的喜好设置该值的格式。 例如，表示货币的字段将以浮点数的形式存储在报表定义中，但是可以根据您所选择的格式属性以不同的格式显示该字段。  
  
 有关显示格式的详细信息，请参阅 [设置报表项的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="report-definition-language-rdl-data-types-and-common-language-runtime-clr-data-types"></a>报表定义语言 (RDL) 数据类型和公共语言运行时 (CLR) 数据类型  
 在 RDL 文件中指定的值必须是 RDL 数据类型。 在编译和处理该报表时，RDL 数据类型将转换为 CLR 数据类型。 下表将显示标记为默认值的转换：  
  
|RDL 类型|CLR 类型|  
|--------------|---------------|  
|字符串|默认值：字符串<br /><br /> 图表、GUID、Timespan|  
|Boolean|默认值：Boolean|  
|Integer|默认值：Int64<br /><br /> Int16、Int32、Uint16、Uint64、Byte、Sbyte|  
|DateTime|默认值：DateTime<br /><br /> DateTimeOffset|  
|Float|默认值：Double<br /><br /> Single、Decimal|  
|Binary|默认值：Byte[]|  
|Variant|除了 Byte[] 之外的上述任何值|  
|VariantArray|Variant 的数组|  
|可序列化|标记有“可序列化”或者实现 ISerializable 的变量或类型。|  
  
## <a name="understanding-data-types-and-writing-expressions"></a>理解数据类型和编写表达式  
 编写的表达式用于比较或组合值（例如，定义组或筛选表达式，或者计算聚合）时，了解数据类型是非常重要的。 比较和计算都只在数据类型相同的项之间才有效。 如果数据类型不匹配，必须使用表达式显式转换报表项中的数据类型。  
  
 下面列出可能需要将数据转换为其他数据类型的情况：  
  
-   将一种数据类型的报表参数值与另一种数据类型的数据集字段进行比较。  
  
-   编写用于比较不同数据类型的值的筛选表达式。  
  
-   编写用于组合不同数据类型的字段的排序表达式。  
  
-   编写用于组合不同数据类型的字段的组表达式。  
  
-   将从数据源检索的值从一种数据类型转换为另一种数据类型。  
  
## <a name="determining-the-data-type-of-report-data"></a>确定报表数据的数据类型  
 若要确定报表项的数据类型，可以编写一个返回报表项数据类型的表达式。 例如，若要显示字段 `MyField`的数据类型，可在表单元中添加以下表达式： `=Fields!MyField.Value.GetType().ToString()`。 结果将显示用于表示 `MyField`的 CLR 数据类型，例如 **System.String** 或 **System.DateTime**。  
  
## <a name="converting-dataset-fields-to-a-different-data-type"></a>将数据集字段转换为其他数据类型  
 在报表中使用数据集字段之前，还可以对这些字段进行转换。 下面列出可用于转换现有数据集字段的方法：  
  
-   修改数据集查询，以添加含有已转换数据的新查询字段。 对于关系数据源或多维数据源，将使用数据源资源来执行转换。  
  
-   基于现有报表数据集字段创建计算字段，方法是编写一个表达式，将一个结果集列中的所有数据都转换到具有另一数据类型的新列。 例如，以下表达式将字段 Year 从整数值转换为字符串值： `=CStr(Fields!Year.Value)`。 有关详细信息，请参阅[在“报表数据”窗格中添加、编辑和刷新字段（报表生成器和 SSRS）](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)。  
  
-   检查所使用的数据处理扩展插件是否包括用于检索预先设定格式的数据的元数据。 例如，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] MDX 查询包括 FORMATTED_VALUE 扩展属性，用于已在处理多维数据集时设置了格式的多维数据集值。 有关详细信息，请参阅 [Analysis Services 数据库的扩展字段属性 (SSRS)](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md)。  
  
## <a name="understanding-parameter-data-types"></a>了解参数数据类型  
 报表参数必须是以下五个数据类型之一：Boolean、DateTime、Integer、Float 或 Text（亦称为 String）。 数据集查询包含查询参数时，将会自动创建报表参数，并将其链接到查询参数。 报表参数的默认数据类型是 String。 若要更改报表参数的默认数据类型，请在“报表参数属性”对话框的“常规”页上，从“数据类型”下拉列表中选择正确的值。  
  
> [!NOTE]  
>  DateTime 数据类型的报表参数不支持毫秒。 尽管可以创建基于包含毫秒值的参数，但是不能从包含具有毫秒的 Date 或 Time 值的可用值下拉列表中选择值。  
  
## <a name="writing-expressions-that-convert-data-types-or-extract-parts-of-data"></a>编写用于转换数据类型或提取部分数据的表达式  
 使用连接操作符 (&) 组合文本和数据集字段时，公共语言运行时 (CLR) 通常提供默认格式。 需要将数据集字段或参数显式转换为特定数据类型时，必须使用 CLR 方法或 Visual Basic 运行时库函数来转换数据。  
  
 下表提供了转换数据类型的示例。  
  
|转换类型|示例|  
|------------------------|-------------|  
|从 DateTime 到 String|`=CStr(Fields!Date.Value)`|  
|从 String 到 DateTime|`=DateTime.Parse(Fields!DateTimeinStringFormat.Value)`|  
|从 String 到 DateTimeOffset|`=DateTimeOffset.Parse(Fields!DateTimeOffsetinStringFormat.Value)`|  
|提取 Year|`=Year(Fields!TimeinStringFormat.Value)`<br /><br /> `-- or --`<br /><br /> `=Year(Fields!TimeinDateTimeFormat.Value)`|  
|从 Boolean 到 Integer|`=CInt(Parameters!BooleanField.Value)`<br /><br /> -1 为 True，0 为 False。|  
|从 Boolean 到 Integer|`=System.Convert.ToInt32(Fields!BooleanFormat.Value)`<br /><br /> 1 为 True，0 为 False。|  
|只需 DateTimeOffset 值的 DateTime 部分|`=Fields!MyDatetimeOffset.Value.DateTime`|  
|只需 DateTimeOffset 值的 Offset 部分|`=Fields!MyDatetimeOffset.Value.Offset`|  
  
 还可以使用 Format 函数控制值的显示格式。 有关详细信息，请参阅 [函数 (Visual Basic)](/dotnet/visual-basic/language-reference/functions/)。  
  
## <a name="advanced-examples"></a>高级示例  
 使用不对数据源中所有数据类型提供转换支持的数据访问接口连接到数据源时，不支持的数据源类型的默认数据类型为 String。 下面的示例提供针对作为字符串返回的特定数据类型的解决方案。  
  
### <a name="concatenating-a-string-and-a-clr-datetimeoffset-data-type"></a>串联 String 和 CLR DateTimeOffset 数据类型  
 对于大多数数据类型，CLR 提供默认转换，因此您可以使用 & 运算符将不同数据类型的值串联到一个字符串中。 例如，下面的表达式将文本“The date and time are: ”与数据集字段 StartDate（这是一个 <xref:System.DateTime> 值）相串联： `="The date and time are: " & Fields!StartDate.Value`。  
  
 对于某些数据类型，可能需要包含 ToString 函数。 例如，下面的表达式使用 CLR 数据类型 <xref:System.DateTimeOffset>演示相同示例，该数据类型包括日期、时间和相对于 UTC 时区的时区偏移量： `="The time is: " & Fields!StartDate.Value.ToString()`。  
  
### <a name="converting-a-string-data-type-to-a-clr-datetime-data-type"></a>将 String 数据类型转换为 CLR DateTime 数据类型  
 如果数据处理扩展插件不支持对某一数据源定义的所有数据类型，则其中的数据可能会作为文本来检索。 例如， **datetimeoffset(7)** 数据类型值可能会作为 String 数据类型来检索。 在澳大利亚的珀斯（Perth）市，2008 年 7 月 1 日上午 6:05:07.9999999 的字符串值 如下：  
  
 `2008-07-01 06:05:07.9999999 +08:00`  
  
 此示例演示的是日期（2008 年 7 月 1 日），后跟 7 位精度的时间（上午 6:05:07.9999999），再跟以小时和分钟表示的 UTC 时区偏移量（加 8 小时、0 分钟）。 对于以下示例，此值已放入名为 **的** 字符串 `MyDateTime.Value`字段中。  
  
 可以使用下列策略之一将此数据转换为一个或多个 CLR 值：  
  
-   在文本框中，使用表达式提取部分字符串。 例如：  
  
    -   下面的表达式只提取 UTC 时区偏移量的小时部分，并将其转换为分钟： `=CInt(Fields!MyDateTime.Value.Substring(Fields!MyDateTime.Value.Length-5,2)) * 60`  
  
         结果为 `480`。  
  
    -   下面的表达式将该字符串转换为日期和时间值： `=DateTime.Parse(Fields!MyDateTime.Value)`  
  
         如果 `MyDateTime.Value` 字符串具有 UTC 偏移量，则 `DateTime.Parse` 函数将首先针对 UTC 偏移量进行调整（将上午 7 点 - [`+08:00`] 调整为 UTC 时间的前一天 晚上 11 点）。 随后， `DateTime.Parse` 函数将应用本地报表服务器的 UTC 偏移量，如有必要，将针对夏时制再次调整时间。 例如，在华盛顿州的雷德蒙德（Redmond），针对夏时制调整的本地时间偏移量为 `[-07:00]`，即比晚上 11 点早 7 个小时。 结果为以下 DateTime 值：`2007-07-06 04:07:07 PM`（2007 年 7 月 6 日下午 4:07）。  
  
 有关将字符串转换为 **DateTime** 数据类型的详细信息，请参阅 MSDN 上的 [分析日期和时间字符串](https://go.microsoft.com/fwlink/?LinkId=89703)、 [针对特定区域性格式化日期和时间](https://go.microsoft.com/fwlink/?LinkId=89704)、 and [Choosing Between DateTime、 DateTimeOffset、 and TimeZoneInfo](/dotnet/standard/datetime/choosing-between-datetime) （在 DateTime、DateTimeOffset 和 TimeZoneInfo 之间进行选择）。  
  
-   向报表数据集添加使用表达式提取部分字符串的新计算字段。 有关详细信息，请参阅[在“报表数据”窗格中添加、编辑和刷新字段（报表生成器和 SSRS）](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)。  
  
-   将报表数据集查询改为使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数分别提取日期和时间值，从而创建单独的列。 下面的示例演示如何使用 **DatePart** 函数添加年度列和已转换为分钟的 UTC 时区列：  
  
     `SELECT`  
  
     `MyDateTime,`  
  
     `DATEPART(year, MyDateTime) AS Year,`  
  
     `DATEPART(tz, MyDateTime) AS OffsetinMinutes`  
  
     `FROM MyDates`  
  
     结果集包含三列。 第一列是日期和时间，第二列是年度，第三列是以分钟表示的 UTC 偏移量。 下面一行显示了示例数据：  
  
     `2008-07-01 06:05:07             2008                   480`  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库数据类型的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md) 和[日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)。  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据类型的详细信息，请参阅 [Analysis Services 中的数据类型](/analysis-services/multidimensional-models/olap-physical/data-types-in-analysis-services)。  
  
## <a name="see-also"></a>另请参阅  
 [设置报表项的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)  
  
