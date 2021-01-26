---
description: 分析数据
title: 分析数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- parsing [Integration Services]
- data parsing [Integration Services]
ms.assetid: 8893ea9d-634c-4309-b52c-6337222dcb39
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 905c5988860aef1a2e495bb17d98a48eaf91c5ce
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594831"
---
# <a name="parsing-data"></a>分析数据

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  包中的数据流在异类数据存储区之间提取和加载数据，这些存储区可能使用多种标准数据类型和自定义数据类型。 在数据流中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 源完成提取数据、分析字符串数据以及将数据转换成 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型的工作。 后续转换可以分析数据，以将其转换为不同的数据类型，或者创建不同数据类型的列副本。 在组件中使用的表达式还可以将参数和操作数转换为不同的数据类型。 最后，在将数据加载到数据存储区时，目标可以分析该数据，以将其转换为目标所使用的数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="two-types-of-parsing"></a>两种分析类型  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了两种用于转换数据的分析类型：快速分析和标准分析。  
  
-   快速分析是一组简单快捷的分析例程，不支持区域设置特定的数据类型转换，仅支持最常用的日期和时间格式。 
  
-   标准分析是一组功能齐全的分析例程，支持可在 Oleaut32.dll 和 Ole2dsip.dll 中使用的自动数据类型转换 API 所提供的所有数据类型转换。   
  
## <a name="fast-parse"></a>Fast Parse
快速分析提供一组快速、简单的数据分析例程。 这些例程是不区分区域设置的，并且它们只支持日期、时间和整数格式的一个子集。  
  
### <a name="requirements-and-limitations"></a>要求和限制  
 通过实现快速分析，包失去了以特定于区域设置的格式和很多常用的 ISO 8601 基本及扩展格式来转换日期、时间和数值数据的能力，但包的性能得到了提高。 例如，快速分析仅支持最常用的日期格式表示形式（比如，YYYYMMDD 和 YYYY-MM-DD），不执行区域设置特定的分析，不能识别货币数据中的特殊字符，也不能转换对整数的十六进制或科学表示形式。  
  
 快速分析仅在使用平面文件源或数据转换时才可用。 包的性能可能会得到显著提高，您应尽可能考虑在这些数据流组件中使用快速分析。  
  
 如果包中的数据流需要受区域设置影响的分析，则应使用标准分析，而不是快速分析。 例如，快速分析不能识别受区域设置影响的数据，包括小数符号（如逗号）、与“年-月-日”格式不同的日期格式以及货币符号。  
  
 快速分析不能识别隐含一个或多个日期部分（如世纪、年、月）的截断的表示形式。 例如，快速分析既不能识别“**-YYMM**”格式（指定某个隐含世纪中的年和月），也不能识别“**--MM**”格式（指定某个隐含年中的月）。 但是，某些降低了精度的表示形式可以被识别。 例如，快速分析可以识别“hhmm;”格式（仅指示时和分）和“**YYYY**”格式（仅指示年）。  
  
 快速分析在列级指定。 在平面文件源和数据转换中，可以指定对输出列进行快速分析。 输入和输出可以同时包含受区域设置影响的列和不受区域设置影响的列。  
 
## <a name="numeric-data-formats-fast-parse"></a>数字数据格式（快速分析）
快速分析提供了一组简单、快捷、不受区域设置影响的数据分析例程。 快速分析仅支持有限的一组整数数据类型格式。  
  
### <a name="integer-data-type"></a>整数数据类型
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 所提供的整数数据类型有：DT_I1、DT_UI1、DT_I2、DT_UI2、DT_I4、DT_UI4、DT_I8 和 DT_UI8。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
 快速分析支持下列整数数据类型的格式：  
  
-   零个或多个前导及尾随空格或制表位。 例如，“  123  ”是有效值。 全部是空格的值计算结果为零。  
  
-   前导加号、减号或无加减号。 例如，+123、-123 和 123 都是有效值。  
  
-   一位或多位阿拉伯数字 (0-9)。 例如，345 是有效值。 不支持其他语言的数字。  
  
 不支持下列数据格式：  
  
-   特殊字符。 例如，不支持货币字符“$”，因而无法分析值“$20”。  
  
-   空白字符，如换行符、回车符和不间断空格符。 例如，无法分析值“ 123”。  
  
-   以十六进制形式表示的整数。 例如，无法分析值 2EE。  
  
-   以科学记数法形式表示的整数。 例如，无法分析值 1E+10。  
  
 下列格式为整数的输出数据格式：  
  
-   负数用减号，正数不用符号。  
  
-   没有空白。  
  
-   一位或多位阿拉伯数字 (0-9)。  

## <a name="date-and-time-formats-fast-parse"></a>日期和时间格式（快速分析）
快速分析提供一组快速、简单的数据分析例程。 快速分析支持下列日期和时间数据类型格式。  
  
### <a name="date-data-type"></a>Date 数据类型 
 快速分析支持日期数据的下列字符串格式：  
  
-   包含前导空格的日期格式。 例如，值“2004- 02-03”有效。  
  
-   ISO 8601 格式，如下表中所示：  
  
    |格式|描述|  
    |------------|-----------------|  
    |YYYYMMDD<br /><br /> YYYY-MM-DD|用四位数表示年、两位数表示月和两位数表示日的基本和扩展格式。 在扩展格式中，日期部分以连字符 (-) 分隔。|  
    |YYYY-MM|用四位数表示年和两位数表示月的基本和扩展简化精度格式。 在扩展格式中，日期部分以连字符 (-) 分隔。|  
    |YYYY|用四位数表示年的简化精度格式。|  
  
 快速分析不支持日期数据的下列格式：  
  
-   用字母表示的月份值。 例如，日期格式 Oct-31-2003 无效。  
  
-   不明确的格式，如 DD-MM-YYYY 和 MM-DD-YYYY。 例如，日期 03-04-1995 和 04-03-1995 都无效。  
  
-   用四位数表示日历年和三位数表示一年中的第几天的基本和扩展截断格式，YYYYDDD 和 YYYY-DDD。  
  
-   用四位数表示年、用两位数表示一年中第几周和一位数表示一周中星期几的基本和扩展格式，YYYYWwwD 和 YYYY-Www-D。  
  
-   年和周日期的基本与扩展截断格式是用四位数表示年和两位数表示周，YYYWww 和 YYYY-Www。  
  
 快速分析将数据输出为 DT_DBDATE。 对截断格式的日期值进行填充。 例如，YYYY 变为 YYYY0101。  
  
 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
### <a name="time-data-type"></a>时间数据类型
 快速分析支持时间数据的下列字符串格式：  
  
-   包含前导空格的时间格式。 例如，值“ 10:24”有效。  
  
-   24 小时制格式。 快速分析不支持 AM 和 PM 表示法。  
  
-   ISO 8601 时间格式，如下表中所示：  
  
    |格式|描述|  
    |------------|-----------------|  
    |HHMISS<br /><br /> HH:MI:SS|用两位数表示小时、两位数表示分钟和两位数表示秒的基本和扩展格式。 在扩展格式中，时间部分以冒号 (:) 分隔。|  
    |HHMI<br /><br /> HH:MI|用两位数表示小时和两位数表示分钟的基本和扩展截断格式。 在扩展格式中，时间部分以冒号 (:) 分隔。|  
    |HH|用两位数表示小时的截断格式。|  
    |00:00:00<br /><br /> 000000<br /><br /> 0000<br /><br /> 00<br /><br /> 240000<br /><br /> 24:00:00<br /><br /> 2400<br /><br /> 24|午夜的格式。|  
  
-   指定时区的时间格式，如下表所示：  
  
    |格式|描述|  
    |------------|-----------------|  
    |+HH:MI<br /><br /> +HHMI|指示为得出本地时间而在协调世界时 (UTC) 基础上加上的小时和分钟数的基本和扩展格式。|  
    |-HH:MI<br /><br /> -HHMI|指示为得出本地时间而从 UTC 减去的小时和分钟数的基本和扩展格式。|  
    |+HH|指示为得出本地时间而在 UTC 基础上加上的小时数的截断格式。|  
    |-HH|指示为得出本地时间而从 UTC 减去的小时数的截断格式。|  
    |Z|值为 0 表示采用 UTC 表示时间。|  
  
     所有时间和日期/时间数据的格式都可以包括时区元素。 不过，如果数据的类型不是 DT_DBTIMESTAMPOFFSET，系统将忽略该时区值。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
     在包括时区元素的格式中，时间元素和时区元素之间没有空格，如下面的示例所示：  
  
     HH:MI:SS[+HH:MI]  
  
     上例中的括号表明时区值是可选的。  
  
-   包含小数的时间格式，如下表所示：  
  
    |格式|描述|  
    |------------|-----------------|  
    |HH[.nnnnnnn]|n 是介于 0 和 9999999 之间的值，表示小时的小数部分。 方括号表明该值是可选的。<br /><br /> 例如，值 12.750 表示 12:45。|  
    |HHMI[.nnnnnnn]<br /><br /> HH:MI[.nnnnnnn]|n 是介于 0 和 9999999 之间的值，表示分钟的小数部分。 方括号表明该值是可选的。<br /><br /> 例如，值 1220.500 表示 12:20:30。|  
    |HHMISS[.nnnnnnn]<br /><br /> HH:MI:SS[.nnnnnnn]|n 是介于 0 和 9999999 之间的值，表示秒的小数部分。 方括号表明该值是可选的。<br /><br /> 例如，值 122040.250 表示 12:20:40.15。|  
  
    > [!NOTE]  
    >  上表中时间格式的小数分隔符可以是小数点或逗号。  
  
-   包括闰秒的时间值，如下例所示：  
  
     23:59:60[.0000000]  
  
     235960[.0000000]  
  
 快速分析将字符串输出为 DT_DBTIME 和 DT_DBTIME2。 对截断格式的时间值进行填充。 例如，HH:MI 变为 HH:MM:00.000。  
  
 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
### <a name="datetime-data-type"></a>日期/时间数据类型  
 快速分析支持日期/时间数据的下列字符串格式：  
  
-   包含前导空格的格式。 例如，值“  2003-01-10T203910”有效。  
  
-   以大写字母 T 分隔的有效日期格式与有效时间格式以及有效时区格式的组合，例如 YYYYMMDDT[HHMISS][+HH:MI]。 时间和时区值不是必需的。 例如，“2003-10-14”有效。  
  
 快速分析不支持时间间隔。 例如，无法分析起始和结束日期和时间以 YYYYMMDDThhmmss/YYYYMMDDThhmmss 格式标识的时间间隔。  
  
 快速分析将字符串输出为 DT_DATE、DT_DBTIMESTAMP、DT_DBTIMESTAMP2 和 DT_DBTIMESTAMPOFFSET。 对截断格式的日期/时间值进行填充。 下表列出为缺少的日期和时间部分添加的值。  
  
|日期/时间部分|填充|  
|---------------------|-------------|  
|秒|添加 00。|  
|分钟数|添加 00:00。|  
|小时|添加 00:00:00。|  
|天|添加 01，表示一个月中的第几天。|  
|月份|添加 01，表示一年中的第几个月。|  
  
 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="enable-fast-parse"></a>启用快速分析
必须为使用快速分析的源或转换的每个列设置快速分析属性。 若要设置该属性，请使用平面文件源和数据转换的高级编辑器。  
  
1.  右键单击平面文件源或数据转换，然后单击“显示高级编辑器”。  
  
2.  在 **“高级编辑器”** 对话框中，单击 **“输入属性和输出属性”** 选项卡。  
  
3.  在 **“输入和输出”** 窗格中，单击要为其启用快速分析的列。  
  
4.  在“属性”窗口中，展开 **“自定义属性”** 节点，然后将 **FastParse** 属性设置为 **True**。  
  
5.  单击“确定”。   

## <a name="standard-parse"></a>Standard Parse
标准分析是一组受区域设置影响的分析例程，这些例程支持 Oleaut32.dll 和 Ole2dsip.dll 中可用的自动数据类型转换 API 所提供的全部数据类型转换。 标准分析相当于 OLE DB 分析 API。  
  
 标准分析支持国际数据的数据类型转换，且快速分析不支持数据格式时应使用标准分析。 有关自动化数据类型转换 API 的详细信息，请参阅 [MSDN Library](/documentation/)中的“Data Type Conversion APIs”（数据类型转换 API）。 
