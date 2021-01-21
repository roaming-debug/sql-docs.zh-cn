---
title: 表达式示例（报表生成器）| Microsoft Docs
description: 使用内置函数、自定义代码、报表和组变量以及用户定义变量来控制报表生成器中的内容和报表外观。
ms.date: 04/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- page breaks [Reporting Services], expressions
- green-bar reports [Reporting Services]
- Visual Basic [Reporting Services]
- functions [Reporting Services], examples
- custom code [Reporting Services]
- appearance of reports
- formatting reports [Reporting Services], expressions
- show/hide [Reporting Services]
- parameters [Reporting Services], expressions
- visibility [Reporting Services], expressions
- page headers [Reporting Services]
- page footers [Reporting Services]
- dates [Reporting Services], expressions
- expressions [Reporting Services], examples
ms.assetid: 87ddb651-a1d0-4a42-8ea9-04dea3f6afa4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 40a05ec1ed6b7ffa05132e0cefbd5157b7d3ce92
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596331"
---
# <a name="expression-examples-report-builder-and-ssrs"></a>表达式示例（报表生成器和 SSRS）
表达式通常在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 分页报表中使用，以控制报表的内容和外观。 表达式以 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 编写，可以使用内置函数、自定义代码、报表变量和组变量以及用户定义的变量。 表达式通常以等号 (=) 开头。 有关表达式编辑器和可以包括的引用类型的详细信息，请参阅[在报表中使用表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)和[添加表达式（报表生成器和 SSRS）](../../reporting-services/report-design/add-an-expression-report-builder-and-ssrs.md)。  
  
> [!IMPORTANT]  
>  启用 RDL 沙盒处理后，在报表发布时，只能在表达式文本中使用某些类型与成员。 有关详细信息，请参阅 [Enable and Disable RDL Sandboxing](../../reporting-services/report-server-sharepoint/enable-and-disable-rdl-sandboxing.md)。  
  
本主题提供了可用于报表中常见任务的表达式的示例。  
  
-   [Visual Basic 函数](#VisualBasicFunctions) ：日期、字符串、转换和条件 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 函数的示例。  
  
-   [报表函数](#ReportFunctions) ：聚合函数和其他内置报表函数的示例。  
  
-   [报表数据的外观](#AppearanceofReportData) ：更改报表外观的示例。  
  
-   [属性](#Properties) ：通过设置报表项属性来控制格式或可见性的示例。  
  
-   [参数](#Parameters) ：在表达式中使用参数的示例。  
  
-   [自定义代码](#CustomCode) ：嵌入自定义代码的示例。  
  
有关特定用途的表达式示例，请参阅以下主题：  
  
-   [组表达式示例（报表生成器和 SSRS）](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)  
  
-   [筛选器公式示例（报表生成器和 SSRS）](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)  
  
-   [常用筛选器（报表生成器和 SSRS）](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md)  
  
-   [报表和组变量集合引用（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md)  
  
有关简单表达式和复杂表达式、使用表达式的位置、以及表达式中可以包含的引用类型的详细信息，请参阅 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)。 有关为计算聚合而计算表达式时所处上下文的详细信息，请参阅[总计、聚合和内置集合的表达式作用域（报表生成器和 SSRS）](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)。  
  
若要了解如何编写使用许多本主题中的表达式示例所用的函数和运算符的表达式，请参阅 [Tutorial: Introducing Expressions](../../reporting-services/tutorial-introducing-expressions.md)。  

  
## <a name="functions"></a>函数  
 报表中的许多表达式都包含函数。 您可以使用这些函数来设置数据格式、应用逻辑和访问报表元数据。 可以编写使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 运行时库、<xref:System.Convert> 和 <xref:System.Math> 命名空间中的函数的表达式。 您可以从其他程序集或自定义代码中向函数添加引用。 还可以使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 中的类，其中包括 <xref:System.Text.RegularExpressions>。  
  
##  <a name="visual-basic-functions"></a><a name="VisualBasicFunctions"></a> Visual Basic 函数  
 您可以使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 函数来处理文本框中所显示的数据，或者处理参数、属性或报表其他区域中所用的数据。 本部分举例说明了其中的一些函数。 有关详细信息，请参阅 [Visual Basic Runtime Library Members](/dotnet/visual-basic/language-reference/runtime-library-members) （Visual Basic 运行时库成员）。  
  
 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 提供了许多自定义格式选项，例如，特定日期格式。 有关详细信息，请参阅 MSDN 上的 [格式化类型](/previous-versions/) 。  
  
### <a name="math-functions"></a>数学函数  
  
-   **Round** 函数可用于将数字舍入为最接近的整数。 下面的表达式将 1.3 舍入为 1：  
  
    ```  
    = Round(1.3)  
    ```  
  
     您也可以编写表达式以将某个值舍入到您指定的倍数，类似于 Excel 中的 **MRound** 函数。 用某个生成整数的因子乘以该值，对数字进行舍入，然后除以同一因子。 例如，若要将 1.3 舍入到 .2 (1.4) 的最接近的倍数，请使用下面的表达式：  
  
    ```  
    = Round(1.3*5)/5  
    ```  
  
###  <a name="date-functions"></a><a name="DateFunctions"></a> 日期函数  
  
-   **Today** 函数可提供当前日期。 此表达式可用在文本框中以在报表上显示日期，或用在参数中以根据当前日期筛选数据。  
  
    ```  
    =Today()  
    ```  
  
-   使用 DateInterval 函数拉取某日期的特定部分。 以下是一些有效的 DateInterval 参数：

    -   DateInterval.Second
    -   DateInterval.Minute
    -   DateInterval.Hour
    -   DateInterval.Weekday
    -   DateInterval.Day
    -   DateInterval.DayOfYear
    -   DateInterval.WeekOfYear
    -   DateInterval.Month
    -   DateInterval.Quarter
    -   DateInterval.Year

    例如，此表达式可显示当前日期在该年中位于哪一周：
  
    ```  
    =DatePart(DateInterval.WeekOfYear, today()) 
    ```  
  
-   若要基于单个参数提供日期范围，可使用 **DateAdd** 函数。 下面的表达式提供名为 *StartDate* 的参数日期之后六个月的日期。  
  
    ```  
    =DateAdd(DateInterval.Month, 6, Parameters!StartDate.Value)  
    ```  
  
-   **Year** 函数可显示某个特定的日期的年份。 您可以使用此表达式将日期组合在一起，或者将年份显示为一组日期的标签。 此表达式可以提供一组给定的销售订单日期的年份。 **Month** 函数和其他函数也可用于日期操作。 有关详细信息，请参阅 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 文档。  
  
    ```  
    =Year(Fields!OrderDate.Value)  
    ```  
  
-   您可以在表达式中组合函数，以自定义格式。 下面的表达式将“月-日-年”的日期格式更改为“月-周-周数”。 例如，将“12/23/2009”更改为“December Week 3”：  
  
    ```  
    =Format(Fields!MyDate.Value, "MMMM") & " Week " &   
    (Int(DateDiff("d", DateSerial(Year(Fields!MyDate.Value),   
    Month(Fields!MyDate.Value),1), Fields!FullDateAlternateKey.Value)/7)+1).ToString  
    ```  
  
     当此表达式在数据集中用作计算字段时，您可以在图表上使用该表达式来按每个月内的周聚合值。  
  
-   以下表达式将 SellStartDate 值的格式设置为 MMM-YY。 SellStartDate 字段为 datetime 数据类型。  
  
    ```  
    =FORMAT(Fields!SellStartDate.Value, "MMM-yy")  
    ```  
  
-   以下表达式将 SellStartDate 值的格式设置为 dd/MM/yyyy。 SellStartDate 字段为 datetime 数据类型。  
  
    ```  
    =FORMAT(Fields!SellStartDate.Value, "dd/MM/yyyy")  
    ```  
  
-   **CDate** 函数将值转换为日期。 **Now** 函数根据您的系统返回包含当前日期和时间的日期值。 **DateDiff** 返回一个指定两个日期值之间的时间间隔数的长整型值。  
  
     以下示例显示了当前年份的开始日期  
  
    ```  
    =DateAdd(DateInterval.Year,DateDiff(DateInterval.Year,CDate("01/01/1900"),Now()),CDate("01/01/1900"))  
    ```  
  
-   以下示例显示了基于当前月的上一个月的开始日期。  
  
    ```  
    =DateAdd(DateInterval.Month,DateDiff(DateInterval.Month,CDate("01/01/1900"),Now())-1,CDate("01/01/1900"))  
    ```  
  
-   以下表达式生成介于 SellStartDate 和 LastReceiptDate 之间的间隔年。 这些字段在两个不同的数据集内，即 DataSet1 和 DataSet2。 [First 函数（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-first-function.md)是一个聚合函数，它返回 DataSet1 中 SellStartDate 的第一个值和 DataSet2 中 LastReceiptDate 的第一个值。  
  
    ```  
    =DATEDIFF("yyyy", First(Fields!SellStartDate.Value, "DataSet1"), First(Fields!LastReceiptDate.Value, "DataSet2"))  
    ```  
  
-   **DatePart** 函数返回一个整型值，该值包含给定日期值的指定组件。以下表达式返回 DataSet1 中 SellStartDate 的第一个值的年份。 指定数据集作用域是因为报表中有多个数据集。  
  
    ```  
    =Datepart("yyyy", First(Fields!SellStartDate.Value, "DataSet1"))  
  
    ```  
  
-   **DateSerial** 函数返回一个代表指定年、月和日的日期值，其时间信息设置为午夜。 以下示例显示了基于当前月的上一个月的结束日期。  
  
    ```  
    =DateSerial(Year(Now()), Month(Now()), "1").AddDays(-1)  
    ```  
  
-   以下表达式基于用户选定的日期参数值显示不同日期。  
  
|示例说明|示例|  
|-------------------------|-------------|  
|昨天|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value)-1)`|  
|两天前|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value)-2)`|  
|一个月前|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value)-1,Day(Parameters!TodaysDate.Value))`|  
|两个月前|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value)-2,Day(Parameters!TodaysDate.Value))`|  
|一年前|`=DateSerial(Year(Parameters!TodaysDate.Value)-1,Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value))`|  
|两年前|`=DateSerial(Year(Parameters!TodaysDate.Value)-2,Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value))`|  
  
###  <a name="string-functions"></a><a name="StringFunctions"></a> 字符串函数  
  
-   使用串联运算符和 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 常量可将多个字段组合在一起。 以下表达式返回两个字段，它们分别位于同一文本框的不同行中：  
  
    ```  
    =Fields!FirstName.Value & vbCrLf & Fields!LastName.Value   
    ```  
  
-   使用 **Format** 函数可设置字符串中日期和数字的格式。 下面的表达式以长日期格式显示 *StartDate* 和 *EndDate* 参数的值：  
  
    ```  
    =Format(Parameters!StartDate.Value, "D") & " through " &  Format(Parameters!EndDate.Value, "D")    
    ```  
  
     如果文本框仅包含日期或数字，则应使用文本框的 Format 属性来应用格式设置，而不应在文本框中使用 **Format** 函数。  
  
-   Right、Len 和 InStr 函数对于返回子字符串十分有用，例如将 DOMAIN\\username 剪裁为用户名   。 下面的表达式从名为 User 的参数返回反斜杠 (\\) 字符右侧的字符串部分：  
  
    ```  
    =Right(Parameters!User.Value, Len(Parameters!User.Value) - InStr(Parameters!User.Value, "\"))  
    ```  
  
     下面的表达式使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.String> 类的成员而不是 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 函数，可得到与上一个表达式相同的值：  
  
    ```  
    =Parameters!User.Value.Substring(Parameters!User.Value.IndexOf("\")+1, Parameters!User.Value.Length-Parameters!User.Value.IndexOf("\")-1)  
    ```  
  
-   显示多值参数的所选值。 下面的示例使用 **Join** 函数，将参数 *MySelection* 的所选值串联到单个字符串中，该字符串可设置为报表项中文本框值的表达式：  
  
    ```  
    = Join(Parameters!MySelection.Value)  
    ```  
  
     以下示例的作用与上面的示例相同，另外在所选值列表之前显示文本字符串。  
  
    ```  
    ="Report for " & JOIN(Parameters!MySelection.Value, " & ")  
  
    ```  
  
-   若要基于单个参数提供日期范围，可使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.Text.RegularExpressions> 中的 Regex 函数对更改现有字符串的格式很有用，例如设置电话号码格式。 以下表达式使用 **Replace** 函数将字段中的十位电话号码格式从“nnn-nnn-nnnn”更改为“(nnn) nnn-nnnn”：  
  
    ```  
    =System.Text.RegularExpressions.Regex.Replace(Fields!Phone.Value, "(\d{3})[ -.]*(\d{3})[ -.]*(\d{4})", "($1) $2-$3")  
    ```  
  
    > [!NOTE]  
    >  验证 Fields!Phone.Value 的值没有多余的空格并且类型为 <xref:System.String>。  
  
### <a name="lookup"></a>查找  
  
-   通过指定键字段，可以使用 **Lookup** 函数为一对一关系（例如键值对）从数据集检索值。 下面的表达式通过提供用于匹配的产品标识符，显示来自数据集（“Product”）的产品名称：  
  
    ```  
    =Lookup(Fields!PID.Value, Fields!ProductID.Value, Fields!ProductName.Value, "Product")  
    ```  
  
### <a name="lookupset"></a>LookupSet  
  
-   通过指定键字段，可以使用 **LookupSet** 函数为一对多关系从数据集检索一组值。 例如，一个人可以有多个电话号码。 在下面的示例中，假定数据集 PhoneList 在每一行中包含一个人员标识符和电话号码。 **LookupSet** 返回值的数组。 下面的表达式将返回值合并到单个字符串中，并且显示 ContactID 指定的人士的电话号码的列表：  
  
    ```  
    =Join(LookupSet(Fields!ContactID.Value, Fields!PersonID.Value, Fields!PhoneNumber.Value, "PhoneList"),",")  
    ```  
  
###  <a name="conversion-functions"></a><a name="ConversionFunctions"></a> 转换函数  
 使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 函数可以将字段从一种数据类型转换为另一种不同的数据类型。 转换函数可用于将字段的默认数据类型转换为计算所需的数据类型或用于组合文本。  
  
-   以下表达式将常量 500 转换为十进制类型，以便将其与筛选表达式值字段中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] money 数据类型进行比较。  
  
    ```  
    =CDec(500)  
    ```  
  
-   下面的表达式显示为多值参数 *MySelection* 选择的值的数目。  
  
    ```  
    =CStr(Parameters!MySelection.Count)  
    ```  
  
###  <a name="decision-functions"></a><a name="DecisionFunctions"></a> 决策函数  
  
-   **Iif** 函数可根据表达式的计算结果（True 或 False）返回两个值中的一个。 下面的表达式使用 **Iif** 函数在 **的值超过 100 时返回布尔值** True `LineTotal` 。 否则，它将返回 **False**：  
  
    ```  
    =IIF(Fields!LineTotal.Value > 100, True, False)  
    ```  
  
-   使用多个 **IIF** 函数（也称为“嵌套 IIF”）可以根据 `PctComplete`的值返回三个值中的一个。 下面的表达式可放置在文本框的填充颜色中，从而根据文本框中的值更改背景色。  
  
    ```  
    =IIF(Fields!PctComplete.Value >= 10, "Green", IIF(Fields!PctComplete.Value >= 1, "Blue", "Red"))  
    ```  
  
     值大于或等于 10 时，显示绿色背景；介于 1 和 9 之间时，显示蓝色背景；小于 1 时，显示红色背景。  
  
-   还有另一种方法可以实现相同功能，即使用 **Switch** 函数。 如果您要测试三个或更多条件， **Switch** 函数将非常有用。 **Switch** 函数可返回与序列中计算结果为 True 的第一个表达式相关联的值：  
  
    ```  
    =Switch(Fields!PctComplete.Value >= 10, "Green", Fields!PctComplete.Value >= 1, "Blue", Fields!PctComplete.Value = 1, "Yellow", Fields!PctComplete.Value <= 0, "Red")  
    ```  
  
     值大于或等于 10 时，显示绿色背景；介于 1 和 9 之间时，显示蓝色背景；等于 1 时显示黄色背景；小于或等于 0 时，显示红色背景。  
  
-   测试 `ImportantDate` 字段的值，如果该值大于一周，则返回“Red”；否则返回“Blue”。 此表达式可用于控制报表项中的文本框的 Color 属性：  
  
    ```  
    =IIF(DateDiff("d",Fields!ImportantDate.Value, Now())>7,"Red","Blue")  
    ```  
  
-   测试 `PhoneNumber` 字段的值，如果为 **null** （在 **中为** Nothing [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]），则返回“无值”；否则返回电话号码值。 此表达式可用于控制报表项中的文本框的值。  
  
    ```  
    =IIF(Fields!PhoneNumber.Value Is Nothing,"No Value",Fields!PhoneNumber.Value)  
    ```  
  
-   测试 `Department` 字段的值，然后返回子报表名称或 **null** （在 **中为** Nothing [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]）。 此表达式可用于条件性钻取子报表。  
  
    ```  
    =IIF(Fields!Department.Value = "Development", "EmployeeReport", Nothing)  
    ```  
  
-   测试字段值是否为空。 此表达式可用于控制图像报表项的 **Hidden** 属性。 在下面的示例中，字段 [LargePhoto] 指定的图像仅当字段值非空时才会显示。  
  
    ```  
    =IIF(IsNothing(Fields!LargePhoto.Value),True,False)  
    ```  
  
-   **MonthName** 函数返回一个包含指定月的名称的字符串值。 以下示例在“月”字段（当该字段包含的值为 0 时）中显示 NA。  
  
    ```  
    IIF(Fields!Month.Value=0,"NA",MonthName(IIF(Fields!Month.Value=0,1,Fields!Month.Value)))  
  
    ```  
  
##  <a name="report-functions"></a><a name="ReportFunctions"></a> 报表函数  
 在表达式中，您可以添加对使用报表中数据的附加报表函数的引用。 本部分举例说明了其中两个函数。 有关报表函数和示例的详细信息，请参阅[聚合函数引用（报表生成器和 SSRS）](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md)。  
  
###  <a name="sum"></a><a name="Sum"></a> Sum  
  
-   **Sum** 函数可以对某个组或数据区域中的值求和。 此函数在组的组头或组尾中非常有用。 下面的表达式显示 Order 组或数据区域中的数据之和：  
  
    ```  
    =Sum(Fields!LineTotal.Value, "Order")  
    ```  
  
-   还可以使用 **Sum** 函数进行条件聚合计算。 例如，如果数据集包含名为 State 的字段，其可能的值为 Not Started、Started、Finished，则将下列表达式放置在组头中时将只计算 Finished 值的聚合总数。  
  
    ```  
    =Sum(IIF(Fields!State.Value = "Finished", 1, 0))  
    ```  
  
###  <a name="rownumber"></a><a name="RowNumber"></a> RowNumber  
  
-   **RowNumber** 函数，如果用在数据区域内的文本框中，则显示表达式所在文本框中的每个实例的行号。 此函数可用于为表中的各行编号。 还可以用于更复杂的情况，如根据行数插入分页符。 有关详细信息，请参阅本主题中的 [分页符](#PageBreaks) 。  
  
     为 **RowNumber** 指定的作用域可控制开始重新计数的时间。 **Nothing** 关键字表示该函数将从最外面的数据区域中的第一行开始计数。 若要在嵌套数据区域中开始计数，可使用该数据区域的名称。 若要在某个组中开始计数，可使用该组的名称。  
  
    ```  
    =RowNumber(Nothing)  
    ```  
  
##  <a name="appearance-of-report-data"></a><a name="AppearanceofReportData"></a> 报表数据的外观  
 您可以使用表达式来控制数据在报表中的显示形式。 例如，可以在一个文本框中显示两个字段的值，显示报表的相关信息，或设置报表中分页符的插入方式。  
  
###  <a name="page-headers-and-footers"></a><a name="PageHeadersandFooters"></a> 页眉和页脚  
 在设计报表时，可能需要在报表表尾中显示报表名称和页码。 为此，可使用以下表达式：  
  
-   下面的表达式提供报表的名称及其运行时间。 可以将该表达式放置在报表表尾或表体的文本框中。 其时间格式为短日期形式的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 格式设置字符串：  
  
    ```  
    =Globals.ReportName & ", dated " & Format(Globals.ExecutionTime, "d")  
    ```  
  
-   下面的表达式放置在报表表尾的文本框中，提供报表的页码和总页数：  
  
    ```  
    =Globals.PageNumber & " of " & Globals.TotalPages  
    ```  
  
 下面的示例说明如何在表头中显示页面中的第一个值和最后一个值，类似于目录列表的形式。 该示例假设存在一个包含名为 `LastName`的文本框的数据区域。  
  
-   下面的表达式放置在表头左侧的文本框中，提供页面中 `LastName` 文本框的第一个值：  
  
    ```  
    =First(ReportItems("LastName").Value)  
    ```  
  
-   下面的表达式放置在表头右侧的文本框中，提供页面中的 `LastName` 文本框的最后一个值：  
  
    ```  
    =Last(ReportItems("LastName").Value)  
    ```  
  
 下面的示例说明如何显示页总页数。 该示例假设存在一个包含名为 `Cost`的文本框的数据区域。  
  
-   下面的表达式放置在表头或表尾中，提供页面上 `Cost` 文本框中的值的总和：  
  
    ```  
    =Sum(ReportItems("Cost").Value)  
    ```  
  
> [!NOTE]  
>  对于表头或表尾中的每个表达式，只能引用一个报表项。 还可以引用表头和表尾表达式中的文本框名称，但不能引用文本框中的实际数据表达式。  
  
###  <a name="page-breaks"></a><a name="PageBreaks"></a> 分页符  
 在某些报表中，可能需要在指定行数之后、特定的组或报表项中插入分页符。 为此，您可以创建一个包含组或您希望的详细记录的组，然后在该组中添加一个分页符，再添加一个组表达式以按指定的行数进行分组。  
  
-   下面的表达式放置在组表达式中，为每 25 行指定一个编号。 如果为组定义了分页符，则此表达式会每隔 25 行插入一个分页符。  
  
    ```  
    =Ceiling(RowNumber(Nothing)/25)  
    ```  
  
     若要允许用户设置每页行数的值，请按创建一个名称为 `RowsPerPage` 的参数，并基于该参数生成组表达式，如下面的表达式所示：  
  
    ```  
    =Ceiling(RowNumber(Nothing)/Parameters!RowsPerPage.Value)  
    ```  
  
     有关为组设置分页符的详细信息，请参阅[添加分页符（报表生成器和 SSRS）](../../reporting-services/report-design/add-a-page-break-report-builder-and-ssrs.md)。  
  
##  <a name="properties"></a><a name="Properties"></a> 属性  
 表达式不仅用于显示文本框中的数据。 还可以用于更改将属性应用于报表项的方式。 您可以更改报表项的样式信息，或更改其可见性。  
  
###  <a name="formatting"></a><a name="Formatting"></a> 格式设置  
  
-   如果下面的表达式用于文本框的 Color 属性中，则可以根据 `Profit` 字段的值更改文本的颜色：  
  
    ```  
    =Iif(Fields!Profit.Value < 0, "Red", "Black")  
    ```  
  
     还可以使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 对象变量 `Me`。 此变量是另一种引用文本框的值的方法。  
  
     `=Iif(Me.Value < 0, "Red", "Black")`  
  
-   如果下面的表达式用于数据区域中的报表项的 BackgroundColor 属性中，则可以将每一行的背景色在淡绿色与白色之间变换：  
  
    ```  
    =Iif(RowNumber(Nothing) Mod 2, "PaleGreen", "White")  
    ```  
  
     如果将表达式用于指定的作用域，则可能必须指明聚合函数的数据集：  
  
    ```  
    =Iif(RowNumber("Employees") Mod 2, "PaleGreen", "White")  
    ```  
  
> [!NOTE]  
>  [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] KnownColor 枚举的可用颜色。  
  
### <a name="chart-colors"></a>图表颜色  
 若要指定形状图的颜色，可以使用自定义代码控制颜色映射为数据点值的顺序。 这有助于您对具有相同类别组的多个图表使用一致的颜色。 有关详细信息，请参阅[对多个形状图指定一致的颜色（报表生成器和 SSRS）](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md)。  
  
###  <a name="visibility"></a><a name="Visibility"></a> 可见性  
 您可以使用报表项的可见性属性来显示和隐藏报表中的项。 在诸如表的数据区域中，可以根据表达式中的值在一开始隐藏详细信息行。  
  
-   如果下面的表达式用于组中详细信息行的初始可见性，则可以在 `PctQuota` 字段中显示超过 90% 的所有销售的详细信息行：  
  
    ```  
    =Iif(Fields!PctQuota.Value>.9, False, True)  
    ```  
  
-   如果在表的 Hidden 属性中设置下面的表达式，则仅当该表多于 12 行时才会显示：  
  
    ```  
    =IIF(CountRows()>12,false,true)  
    ```  
  
-   如果在列的 **Hidden** 属性中设置下面的表达式，则仅当从数据源完成数据检索后，该字段存在于报表数据集中时才显示该列：  
  
    ```  
    =IIF(Fields!Column_1.IsMissing, true, false)  
    ```  
  
###  <a name="urls"></a><a name="Hyperlinks"></a> URL  
 可以使用报表数据自定义 URL，还可以有条件地控制是否将 URL 添加为对文本框的操作。  
  
-   如果将下面的表达式用作对文本框的操作，则可以生成一个自定义 URL，它可将数据集字段 `EmployeeID` 指定为 URL 参数。  
  
    ```  
    ="https://adventure-works/MyInfo?ID=" & Fields!EmployeeID.Value  
    ```  
  
     有关详细信息，请参阅[向 URL 添加超链接（报表生成器和 SSRS）](../../reporting-services/report-design/add-a-hyperlink-to-a-url-report-builder-and-ssrs.md)。  
  
-   下面的表达式可以有条件地控制是否要在文本框中添加 URL。 此表达式的计算结果取决于一个名为 `IncludeURLs` 的参数，该参数允许用户决定是否在报表中包括活动 URL。 此表达式设置为一个对文本框的操作。 通过将参数设置为 False，然后再查看报表，可以导出不包含超链接的 Microsoft Excel 报表。  
  
    ```  
    =IIF(Parameters!IncludeURLs.Value,"https://adventure-works.com/productcatalog",Nothing)  
    ```  
  
##  <a name="report-data"></a><a name="ReportData"></a> 报表数据  
 您可使用表达式来处理报表中所使用的数据。 可以引用参数和其他报表信息。 甚至可以更改用于检索报表数据的查询。  
  
###  <a name="parameters"></a><a name="Parameters"></a> Parameters  
 您可以在参数中使用表达式来更改参数的默认值。 例如，可以根据用于运行报表的用户 ID，使用参数来筛选某个特定用户的数据。  
  
-   下面的表达式如果用作参数的默认值，可以收集运行报表的用户的 ID：  
  
    ```  
    =User!UserID  
    ```  
  
-   若要在查询参数、筛选表达式、文本框或其他报表区域中引用参数，请使用 **Parameters** 全局集合。 此示例假定参数的名称为 *Department*：  
  
    ```  
    =Parameters!Department.Value  
    ```  
  
-   可在报表中创建参数，但需要设置为隐藏。 报表在报表服务器上运行时，该参数不会显示在工具栏中，并且报表读者无法更改默认值。 您可以将设置为默认值的隐藏参数用作自定义常量。 可以在任何表达式中使用此值，包括字段表达式。 下面的表达式标识由 *ParameterField* 参数的默认参数值指定的字段：  
  
    ```  
    =Fields(Parameters!ParameterField.Value).Value  
    ```  
  
##  <a name="custom-code"></a><a name="CustomCode"></a> 自定义代码  
 您可以在报表中使用自定义代码。 自定义代码嵌入在报表中，或存储在报表使用的自定义程序集中。 有关自定义代码的详细信息，请参阅[报表设计器的表达式中的自定义代码和程序集引用 (SSRS)](../../reporting-services/report-design/custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs.md)。  
  
### <a name="using-group-variables-for-custom-aggregation"></a>使用组变量自定义聚合  
 您可以初始化特定组作用域的本地组变量的值，然后在表达式中包含对该变量的引用。 可以将组变量和自定义代码一起使用的方法之一是实现自定义聚合。 有关详细信息，请参阅 [Using Group Variables in Reporting Services 2008 for Custom Aggregation](/archive/blogs/robertbruckner/)（在 Reporting Services 2008 使用组变量自定义聚合）。  
  
 有关变量的详细信息，请参阅 [报表和组变量集合引用（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md)。  
  
## <a name="suppressing-null-or-zero-values-at-run-time"></a>取消运行时的 Null 值或零值  
 处理报表时，表达式中某些值的计算结果可能为 Null 值或未定义。 这将创建运行时错误，从而导致在文本框中显示 **#Error** ，而不是计算后的表达式。 **IIF** 函数对此行为特别敏感，因为它不同于 If-Then-Else 语句， **IIF** 语句的每一部分在传递到测试 **true** 或 **false** 的例程之前，都要进行计算（包括函数调用）。 如果 `=IIF(Fields!Sales.Value is NOTHING, 0, Fields!Sales.Value)` 为 NOTHING，则语句 **将在所呈现的报表中生成** #Error `Fields!Sales.Value` 。  
  
 若要避免此情况，请使用以下策略之一：  
  
-   如果字段 B 的值为 0 或未定义，则将分子设置为 0，分母设置为 1；否则将分子设置为字段 A 的值，将分母设置为字段 B 的值。  
  
    ```  
    =IIF(Field!B.Value=0, 0, Field!A.Value / IIF(Field!B.Value =0, 1, Field!B.Value))  
    ```  
  
-   使用自定义代码函数返回表达式的值。 下面的示例返回当前值和先前值之间的百分比差异。 这不但可用于计算任意两个连续值之间的差异，还可以处理第一次比较（无先前值的情况）的边界情况以及先前值与当前值中有一个为 **null** （在 **中为** Nothing [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]）的情况。  
  
    ```  
    Public Function GetDeltaPercentage(ByVal PreviousValue, ByVal CurrentValue) As Object  
        If IsNothing(PreviousValue) OR IsNothing(CurrentValue) Then  
            Return Nothing  
        Else if PreviousValue = 0 OR CurrentValue = 0 Then  
            Return Nothing  
        Else   
            Return (CurrentValue - PreviousValue) / CurrentValue  
        End If  
    End Function  
    ```  
  
     下面的表达式显示如何针对“ColumnGroupByYear”容器（组或数据区域）从文本框调用此自定义代码。  
  
    ```  
    =Code.GetDeltaPercentage(Previous(Sum(Fields!Sales.Value),"ColumnGroupByYear"), Sum(Fields!Sales.Value))  
    ```  
  
     这有助于避免发生运行时异常。 现在可以在文本框的 `=IIF(Me.Value < 0, "red", "black")` Color **属性中使用** 之类的表达式，以便有条件地基于这些值是大于还是小于 0 来显示文本。  
  
## <a name="see-also"></a>另请参阅  
 [筛选器公式示例（报表生成器和 SSRS）](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)   
 [组表达式示例（报表生成器和 SSRS）](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)   
 [在报表中使用表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [常用筛选器（报表生成器和 SSRS）](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md)  
  
