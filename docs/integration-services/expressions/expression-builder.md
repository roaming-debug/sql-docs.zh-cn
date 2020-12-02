---
description: 表达式生成器
title: 表达式生成器 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.expressionbuilder.f1
helpviewer_keywords:
- Expression Builder dialog box
ms.assetid: 4717ce33-bd4e-44bc-81e0-002de075b4d1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f8eb6c6771e2684d03380a721c7ef96dcb08488
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123287"
---
# <a name="expression-builder"></a>表达式生成器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用“表达式生成器”对话框创建和编辑属性表达式，或者编写使用图形用户界面设置变量值的表达式，此类表达式列出不同的变量并提供对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 表达式语言包含的函数、类型转换和运算符的内置引用。  
  
 属性表达式是分配给某个属性的表达式。 在对此类表达式求值时，属性将动态更新，以使用表达式的计算结果。 同样，在变量中使用表达式时，也可以使用该表达式的计算结果来更新该变量值。  
  
 使用表达式的方法很多：  
  
-   **任务** ：通过插入存储在变量中的电子邮件地址来更新发送邮件任务使用的“收件人”行；或是将字符串（如“Sales for:”）与 GETDATE 函数返回的当前日期连接在一起以更新“主题”行。  
  
-   **变量** ：使用 `DATEPART("mm",GETDATE())`等表达式将变量值设置为当前月；或通过使用表达式 `"Today's date is " + (DT_WSTR,30)(GETDATE())`连接字符串文字与当前日期来设置字符串的值。  
  
-   **连接管理器** ：通过使用包含不同代码页标识符的变量来设置平面文件连接管理器的代码页；或通过在表达式中输入正整数（如 3）来指定在数据文件中跳过的行数。  
  
 了解有关属性表达式和编写表达式的详细信息，请参阅[在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)和 [Integration Services (SSIS) 表达式](../../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
## <a name="options"></a>选项  
  
|术语|定义|  
|----------|----------------|  
|**变量**|展开 **“变量”** 文件夹，再将变量拖至 **“表达式”** 框。|  
|**数学函数**<br /><br /> **字符串函数**<br /><br /> **日期/时间函数**<br /><br /> **NULL 函数**<br /><br /> **类型转换**<br /><br /> **运算符**|展开相应的文件夹，再将函数、类型转换和运算符拖至 **“表达式”** 框。|  
|**表达式**|编辑或键入表达式。|  
|**计算结果值**|列出表达式的计算结果。|  
|**计算表达式**|单击 **“计算表达式”** 可以查看表达式的计算结果。|  
  
## <a name="see-also"></a>另请参阅  
 [“表达式”页](../../integration-services/expressions/expressions-page.md)   
 [属性表达式编辑器](../../integration-services/expressions/property-expressions-editor.md)   
 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)   
 [系统变量](../../integration-services/system-variables.md)  
  
  
