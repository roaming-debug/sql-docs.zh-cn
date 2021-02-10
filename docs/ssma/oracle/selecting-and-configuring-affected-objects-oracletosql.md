---
description: 选择并配置受影响的对象 (OracleToSQL)
title: 选择并配置受影响的对象 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Columns Comparison Settings
- Selection of Affected Objects
ms.assetid: 545eeda2-9829-4187-a858-619a96b4b71d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b8256909a2ebc460da2dd2db826e809dd6dfb50c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067622"
---
# <a name="selecting-and-configuring-affected-objects-oracletosql"></a>选择并配置受影响的对象 (OracleToSQL)
在此页上，您可以选择表和外键，在 SSMA 验证在上一步中选择的对象的执行结果时应比较的更改。 此外，还可以自定义验证参数。  
  
## <a name="selection-of-affected-objects"></a>选择受影响的对象  
在位于窗口左侧的 Oracle 对象树中，检查表和外键，其中应比较的更改是否相同。  
  
如果 SSMA 测试人员无法验证这些对象中的任何一个，你将看到标记为 **某些选定对象** 的链接在对象树下包含错误。 单击此链接可查看无法比较这些对象的原因，以及清除错误对象的选择。  
  
## <a name="table"></a>表  
"表" 选项卡包含所选表的网格视图。 该网格包含有关所选表的下列信息：  
  
-   列名  
  
-   数据类型  
  
-   精度  
  
-   缩放  
  
-   规则  
  
-   默认  
  
-   身份  
  
-   Nullable  
  
## <a name="sql"></a>Sql  
SQL 选项卡包含所选表的 "创建表" SQL。  
  
## <a name="data"></a>数据  
"数据" 选项卡显示所选表中的数据。  
  
## <a name="properties"></a>属性  
"属性" 选项卡显示所选表的属性。 "属性" 选项卡下提供以下字段：  
  
-   已创建或上次修改  
  
-   对象名称  
  
## <a name="columns-comparison-settings"></a>列比较设置  
为 " **列比较** " 页上的表列建立比较规则。 可以进行以下设置。  
  
### <a name="use-during-test-comparisons"></a>在测试比较期间使用  
确定此列是否将参与测试结果验证。  
  
-   如果选择 " **True**"，则 SSMA 将在 Oracle 上执行测试后将此列的内容与 SQL Server 中列的内容进行比较。 
  
-   如果选择 "**False**"，则将从结果验证中排除列。  
  
### <a name="use-custom-scale"></a>使用自定义缩放  
对于数值数据类型的列，可以设置比较的自定义刻度。  
  
-   如果选择 " **True**"，则在比较小数位数之前，将根据 **比较比例** 值对数值进行舍入。  
  
-   如果选择 **False**，则数字比较将是精确的。  
  
### <a name="comparing-scale"></a>比较刻度  
  
-   仅当 " **使用自定义缩放** " 选项设置为 **True** 时可用。 这是数值比较的精度。  
  
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
  
-   如果选择 " **False**"，则比较会考虑字母大小写。  
  
## <a name="comparing-sql"></a>比较 SQL  
您可以在 **比较 SQL** 页面上查看 SSMA 测试器生成的 SELECT 语句。 测试人员将逐行比较这些语句的结果集。 Oracle 结果集的每个后续行应等于 SQL Server 中生成的结果集的下一行。
  
您可以编辑这些 SELECT 语句以提供自定义验证。 若要在 Oracle 和 SQL Server 语句中保存更改，请相应地使用 source 和 target SQL 下的 " **应用** " 按钮。  
  
## <a name="next-step"></a>下一步  
[自定义调用顺序 &#40;OracleToSQL&#41;](../../ssma/oracle/customizing-calls-order-oracletosql.md)  
  
## <a name="see-also"></a>另请参阅  
[完成测试用例准备 &#40;OracleToSQL&#41;](../../ssma/oracle/finishing-test-case-preparation-oracletosql.md)  
[&#40;OracleToSQL&#41;运行测试用例 ](../../ssma/oracle/running-test-cases-oracletosql.md)  
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
