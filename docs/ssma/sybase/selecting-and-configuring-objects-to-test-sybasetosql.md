---
description: 选择并配置要测试的对象 (SybaseToSQL)
title: 选择并配置要测试 (SybaseToSQL) 的对象 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Parameter Comparision Setting
- Tester Component,Selecting Objects
ms.assetid: 89c23aad-bfee-4917-bc16-175288390ac0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a43c4d8b1448949725820d69c6489aff26770d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060562"
---
# <a name="selecting-and-configuring-objects-to-test-sybasetosql"></a>选择并配置要测试的对象 (SybaseToSQL)
在此步骤中，您将选择要测试的对象，并配置用于比较过程的输出参数和函数的返回值的设置。  
  
## <a name="selection-of-objects-to-test"></a>选择要测试的对象  
在位于窗口左侧的 Sybase 对象树中，检查要在测试过程中调用的对象。 请参阅 [测试迁移的数据库对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md) 主题中的可测试对象的完整列表。  
  
如果 SSMA 测试人员不支持选择进行测试的任何对象，则会看到标记为 **某些选定对象** 在对象树下包含错误的链接。 单击此链接可查看无法测试这些对象的原因，以及清除错误对象的选择。  
  
在右侧，可以查看多个页面， **SQL** 页面显示当前对象的定义。 在 SQL 和 **Post** 后的 **sql** 页中，可以指定在调用测试对象开始之前和之后运行的脚本。 当对象需要其他对象（如临时表或游标）时，这可能很有用。 如果对象是存储过程或函数，则 " **参数** " 页将列出这些参数。 " **属性** " 页显示对象的其他特征。 请参阅下面的 **参数 Comparsions** 和 **调用值** 页的说明。  
  
## <a name="parameter-comparison-settings"></a>参数比较设置  
在 " **参数比较** " 页中为输出参数和返回值建立比较规则。 可以进行以下设置。  
  
### <a name="use-during-comparisons"></a>在比较期间使用  
在测试结果比较中使用选定的参数启用。  
  
-   如果选择 " **True**"，则在对具有相应值的 Sybase 执行此过程后，SSMA 会比较此参数的输出值。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   如果选择 "**False**"，则将从结果验证中排除参数。  
  
### <a name="use-custom-scale"></a>使用自定义缩放  
对于近似和固定长度数值数据类型的参数，可以设置比较的自定义刻度。  
  
-   如果选择 " **True**"，则在比较小数位数之前，将根据 **比较比例** 值对数值进行舍入。  
  
-   如果选择 **False**，则数字比较将是精确的。  
  
### <a name="comparing-scale"></a>比较刻度  
仅当 " **使用自定义缩放** " 选项设置为 **True** 时可用。 这是数值比较的精度。  
  
### <a name="date-time-comparing"></a>比较日期时间  
定义日期/时间值的比较方式。  
  
-   如果选择 " **比较整个日期**"，则将执行两个平台中的值的完全比较。  
  
-   如果选择 " **仅比较日期**"，则将忽略时间部分。  
  
-   如果选择 " **仅比较时间**"，则将忽略日期部分。  
  
-   如果选择 " **忽略毫秒**"，则会将结果与秒进行比较。  
  
-   如果选择 " **忽略日期和毫秒**"，则结果将只按时间部分进行比较，而忽略秒的小数部分。  
  
### <a name="ignore-strings-case"></a>忽略字符串大小写  
控制比较的区分大小写。  
  
-   如果选择 " **True**"，则比较不区分大小写。  
  
-   如果选择 **False**，则比较区分大小写。  
  
### <a name="ignore-trailing-spaces"></a>忽略尾随空格  
控制在比较期间如何处理尾随空格。  
  
-   如果选择 " **True**"，则在比较之前将右剪裁比较的字符串。  
  
-   如果选择 **False**，则比较的字符串将保留尾随空格。  
  
## <a name="specify-input-values-for-procedures-and-functions-call-values"></a> (调用值指定过程和函数的输入值)   
您可以在 " **调用值** " 页上指定输入参数值。 " **添加调用** " 按钮添加一个具有空参数值的新调用。 " **删除调用** " 按钮删除当前调用。  
  
## <a name="next-step"></a>下一步  
[选择并配置受影响的对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
  
## <a name="see-also"></a>另请参阅  
[测试迁移的数据库对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
