---
description: 高级 Integration Services 表达式的示例
title: 高级 Integration Services 表达式的示例 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- functions [Integration Services]
- operators [Integration Services]
- expressions [Integration Services], examples
- examples [Integration Services]
ms.assetid: c7794ba3-0de5-466b-ae8a-9ddd27360049
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b5a0e9c219a1649385b337ea378dec751f7851d4
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123312"
---
# <a name="examples-of-advanced-integration-services-expressions"></a>高级 Integration Services 表达式的示例

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本节提供合并多个运算符和函数的高级表达式示例。 如果表达式用于优先约束或有条件拆分转换中，则其计算结果必须为布尔值。 但是，这一限制并不适用于属性表达式、变量、派生列转换或 For 循环容器中使用的表达式。  
  
 下列示例使用 AdventureWorks 和 [!INCLUDE[ssSampleDBDWobject](../../includes/sssampledbdwobject-md.md)][!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 每个示例都标识了其使用的表。  
  
## <a name="boolean-expressions"></a>布尔表达式  
  
-   此示例使用 **Product** 表。 表达式计算 **SellStartDate** 列中的月份项，如果月份为六月或更晚，则返回 TRUE。  
  
    ```  
    DATEPART("mm",SellStartDate) > 6  
    ```  
  
-   此示例使用 **Product** 表。 表达式计算 **ListPrice** 列被 **StandardCost** 列除后的舍入结果，如果结果大于 1.5，则返回 TRUE。  
  
    ```  
    ROUND(ListPrice / StandardCost,2) > 1.50  
    ```  
  
-   此示例使用 **Product** 表。 如果三个运算的计算结果都为 TRUE，则表达式返回 TRUE。 如果 **Size** 列和 **BikeSize** 变量具有不兼容的数据类型，则表达式就需要进行显式转换，如第二个示例所示。 到 DT_WSTR 的转换包括字符串的长度。  
  
    ```  
    MakeFlag ==  TRUE && FinishedGoodsFlag == TRUE && Size != @BikeSize  
    MakeFlag ==  TRUE && FinishedGoodsFlag == TRUE  && Size != (DT_WSTR,10)@BikeSize  
    ```  
  
-   此示例使用 **CurrencyRate** 表。 表达式比较表和变量中的值。 如果 **FromCurrencyCode** 或 **ToCurrencyCode** 列中的项等于变量值，且 **AverageRate** 中的值大于 **EndOfDayRate** 中的值，则返回 TRUE。  
  
    ```  
    (FromCurrencyCode == @FromCur || ToCurrencyCode == @ToCur) && AverageRate > EndOfDayRate  
    ```  
  
-   此示例使用 **Currency** 表。 如果 **Name** 列的第一个字符不是 a 或 A，则表达式返回 TRUE。  
  
    ```  
    SUBSTRING(UPPER(Name),1,1) != "A"  
    ```  
  
     下列表达式提供相同的结果，但效率更高（因为只有一个字符被转换为大写）。  
  
    ```  
    UPPER(SUBSTRING(Name,1,1)) != "A"  
    ```  
  
## <a name="non-boolean-expressions"></a>非布尔表达式  
 非布尔表达式用于派生列转换、属性表达式和 For 循环容器中。  
  
-   此示例使用 **Contact** 表。 表达式删除 **FirstName**、 **MiddleName** 和 **LastName** 列中的前导空格和尾随空格。 如果 **MiddleName** 列不为空，则提取其第一个字母，将该中间名首字母与 **FirstName** 和 **LastName** 中的值连接，并在值之间插入适当的空格。  
  
    ```  
    TRIM(FirstName) + " " + (!ISNULL(MiddleName) ? SUBSTRING(MiddleName,1,1) + " " : "") + TRIM(LastName)  
    ```  
  
-   此示例使用 **Contact** 表。 表达式验证 **Salutation** 列中的项。 它返回 **Salutation** 项或空字符串。  
  
    ```  
    (Salutation == "Sr." || Salutation == "Ms." || Salutation == "Sra." || Salutation == "Mr.") ? Salutation : ""  
    ```  
  
-   此示例使用 **Product** 表。 表达式将 **Color** 列中的第一个字符转换为大写，并将其余字符转换为小写。  
  
    ```  
    UPPER(SUBSTRING(Color,1,1)) + LOWER(SUBSTRING(Color,2,15))  
    ```  
  
-   此示例使用 **Product** 表。 表达式计算产品的销售月份数，并在 **SellStartDate** 或 **SellEndDate** 列包含 NULL 时，返回字符串“Unknown”。  
  
    ```  
    !(ISNULL(SellStartDate)) && !(ISNULL(SellEndDate)) ? (DT_WSTR,2)DATEDIFF("mm",SellStartDate,SellEndDate) : "Unknown"  
    ```  
  
-   此示例使用 **Product** 表。 表达式计算 **StandardCost** 列上的加成，并将结果舍入到小数点后两位。 结果以百分比表示。  
  
    ```  
    ROUND(ListPrice / StandardCost,2) * 100  
    ```  
  
## <a name="related-tasks"></a>Related Tasks  
 [在数据流组件中使用表达式](/previous-versions/sql/sql-server-2016/ms141007(v=sql.130))  
  
## <a name="related-content"></a>相关内容  
 pragmaticworks.com 上的技术文章 [SSIS 表达式小抄表](https://go.microsoft.com/fwlink/?LinkId=746575)。  
  
