---
description: 使用测试存储库 (OracleToSQL)
title: 使用测试存储库 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Test Cases Repository
- Test Results Repository
ms.assetid: f941cce4-d3e3-4aeb-a88a-4f101a97a9f4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f73dd1aabecee8625ace5cc4af0d342124306815
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063452"
---
# <a name="using-test-repositories-oracletosql"></a>使用测试存储库 (OracleToSQL)
SSMA 测试储存库存储 SSMA 测试程序测试用例和测试结果以供以后使用。 存储库数据保存在 **ssmatesterdb** 数据库的架构 **ssma_oracle_utilities** 的 SQL Server 表 **TestCaseRepository** 和 **RunTestCaseResultRepository** 中。  
  
以下按钮在 "测试用例的存储库" 对话框中可用：  
  
-   单击 " **刷新** " 按钮刷新测试用例或测试结果列表。  
  
-   单击 " **关闭** " 按钮以关闭 "测试用例的存储库" 对话框。  
  
## <a name="test-cases-repository"></a>测试用例存储库  
可以通过单击 "测试 **程序" 菜单上的 "** **测试用例 ...** " 来查看测试用例存储库。 然后，SSMA 会在 "**测试用** 例" 页上显示包含已保存测试用例的列表的 "**测试用例**" 对话框窗口。  
  
网格显示了有关每个测试用例的以下信息：  
  
-   名称：测试用例名称。  
  
-   已创建：测试用例创建日期。  
  
-   已修改：测试用例上次修改日期。  
  
-   说明：测试用例的说明。  
  
"测试用例" 页上提供了以下按钮：  
  
-   单击 " **添加** " 按钮运行测试用例向导并创建一个新测试。  
  
-   单击 " **删除** " 按钮可从存储库中删除选定的测试。删除测试用例时，还将删除所有相关的测试结果。  
  
-   单击 " **编辑** " 按钮以运行测试用例向导并更改所选测试。  
  
-   单击 " **运行** " 按钮以打开 " [正在运行的测试用例" (OracleToSQL)](./running-test-cases-oracletosql.md) "对话框并执行所选测试。  
  
## <a name="test-results-repository"></a>测试结果存储库  
您可以在 "**测试用例**" 窗口的 "**测试结果**" 页上查看测试结果存储库。 通过单击 "**测试** 器" 菜单上的 "**测试结果 ...** " 打开它。  
  
可以在 **测试结果** 页上使用两个筛选器：  
  
-   测试用例名称筛选器：允许选择测试结果（按测试用例名称）。 此筛选器的 **所有测试用例值都** 允许显示所有测试用例的测试结果。  
  
-   测试用例执行日期筛选器：按保存日期筛选测试结果。此筛选器的 " **所有期间** " 值允许显示任何保存日期的测试结果。  
  
以下有关测试结果的信息将显示在网格中。  
  
-   名称：测试用例名称。  
  
-   已保存：测试用例保存日期。  
  
-   结果：测试执行的简短摘要 (此单元格的工具提示显示测试执行) 的完整摘要。  
  
"测试结果" 页上提供了以下按钮：  
  
-   单击 " **查看** " 按钮以打开 " [查看测试用例" 报表 &#40;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md) 当前测试用例结果的 OracleToSQL&#41;。  
  
-   单击 " **删除** " 按钮以删除选定的测试结果  
  
## <a name="see-also"></a>另请参阅  
[&#40;OracleToSQL&#41;运行测试用例 ](../../ssma/oracle/running-test-cases-oracletosql.md)  
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
