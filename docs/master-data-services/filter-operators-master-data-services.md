---
description: Filter 运算符 (Master Data Services)
title: Filter 运算符
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 27914c8b-8951-4b7d-914d-1cbf528dd248
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 10ccda2cf25a98a21f9ab2e3d343c6b0566d279f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272538"
---
# <a name="filter-operators-master-data-services"></a>Filter 运算符 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  筛选成员列表时，可以使用下列运算符。  
  
> [!NOTE]  
>  当您按多个条件进行筛选时，必须满足所有条件才能返回结果。 例如，SquareFeet = 2000 **AND** Division <> 123。  
  
## <a name="filter-operators"></a>Filter 运算符  
  
|控件名称|说明|  
|------------------|-----------------|  
|**等于**|返回与指定条件完全相同的属性值。 例如，如果要针对 **Mountain-100** 进行筛选，则必须键入 **Mountain-100**。|  
|**不等于**|返回与指定条件不完全相同的属性值。 筛选条件必须与您要在结果中忽略的属性值完全相同。 例如，如果要忽略与 **Mountain-100** 匹配的结果，则必须键入 **Mountain-100**。<br /><br /> <br /><br /> 注意：对某个属性应用带有“Is not equal”子句的筛选条件时，如果 SET ANSI_NULLS 在数据库设置中设置为 ON，则属性为 NULL 的成员将传递该筛选条件并作为结果返回。 若要停止此行为，请在数据库设置中将 SET ANSI_NULLS 设置为 OFF。 如果 SET ANSI_NULLS 设置为 OFF，则在数据值为 NULL 时，所有数据与 Null 值的比较求得的值为 TRUE，最终结果是该成员不会传递“Is not equal”子句。 有关详细信息，请参阅 [SET ANSI_NULLS (Transact-SQL)](../t-sql/statements/set-ansi-nulls-transact-sql.md)。|  
|**类似于**|使用 Transact-SQL 中的 LIKE 运算符来筛选结果。 有关详细信息，请参阅 SQL Server 联机丛书中的 [LIKE (Transact-SQL)](../t-sql/language-elements/like-transact-sql.md)。|  
|**不类似于**|使用 Transact-SQL 中的 NOT 运算符来筛选结果。 有关详细信息，请参阅 SQL Server 联机丛书中的 [NOT (Transact-SQL)](../t-sql/language-elements/not-transact-sql.md)。|  
|**大于**|返回大于指定条件的属性值。 例如，如果要返回以大于 **F** 的字母开头的属性值，请键入 **F**。|  
|**小于**|返回小于指定条件的属性值。 例如，如果要返回以小于 **F** 的字母开头的属性值，请键入 **F**。|  
|**大于或等于**|返回大于或等于指定条件的属性值。 例如，如果要返回以数字 **3** 或更大数字开头的属性值，请键入 **3**。|  
|**小于或等于**|返回小于或等于指定条件的属性值。 例如，如果要返回以数字 **3** 或更小数字开头的属性值，请键入 **3**。|  
|**匹配**|使用模糊查找索引来筛选结果。<br /><br /> 请使用“相似性级别”字段来指定属性值与指定筛选条件匹配的程度（默认值为 30%）。<br /><br /> 请在 **“算法”** 列表框中选择以下之一：<br /><br /> **Levenshtein**：一个字符串与另一个字符串匹配所需的距离（该距离基于编辑次数，例如添加或删除的次数）。 这是默认值。 不需要任何附加参数。<br /><br /> **Jaccard**：尝试匹配多个字符串时最适用的指数。 此搜索支持一个附加的包含偏差参数（见下文）。<br /><br /> **Jaro-Winkler**：最适用于查找重复人名的距离。 此方法返回比任何其他方法都要多的结果。 不支持包含偏差。<br /><br /> **最长公共子序列**：基于子序列，其中，模式中的字母按顺序显示，尽管它们可以分隔 (例如，"MSR" 是 "MaSteR" ) 的子序列。 此搜索支持一个附加的包含偏差参数（见下文）。<br /><br /> <br /><br /> 注意：为“Jaccard”或“最长公共子序列”算法添加“包含偏差”。 这是一个长度阈值，以 0 到 1 之间的小数形式提供，默认值为 .62。 较低的阈值将提高可能返回的匹配项的数量。|  
|**不匹配**|使用模糊查找索引来筛选结果。 请使用 **“相似性级别”** 字段来指定属性值与指定筛选条件不匹配的程度。|  
|**包含模式**|使用 .NET Framework 正则表达式按指定模式筛选结果。 有关正则表达式的详细信息，请参阅 MSDN Library 中的 [正则表达式语言元素](/dotnet/standard/base-types/regular-expression-language-quick-reference) 。|  
|**不包含模式**|使用 .NET Framework 正则表达式筛选与指定模式不匹配的结果。 有关正则表达式的详细信息，请参阅 MSDN Library 中的 [正则表达式语言元素](/dotnet/standard/base-types/regular-expression-language-quick-reference) 。|  
|**为 NULL**|返回为 Null 的属性值。 如果选择 **“为 NULL”** 运算符，则 **“条件”** 字段禁用。|  
|**不为 NULL**|返回不为 Null 的属性值。 如果选择 **“不为 NULL”** 运算符，则禁用 **“条件”** 字段。|  
  
