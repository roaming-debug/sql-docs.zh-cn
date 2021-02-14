---
description: 完成测试用例准备 (OracleToSQL)
title: 完成测试用例准备 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 32f38713-7ae4-48d3-980d-74cadc8545a0
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: f1d0f99987a58a8d942c9d7601124c3cfc39b09d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014618"
---
# <a name="finishing-test-case-preparation-oracletosql"></a>完成测试用例准备 (OracleToSQL)
向导的最后一页显示测试用例说明以及有关测试中涉及的对象的信息。 此外，在此页上，您可以设置测试执行选项。  
  
**测试用例信息** 部分显示了测试用例的名称和说明。  
  
" **选择要测试的对象** " 部分包含按对象类型分组的已测试对象的命名列表。  
  
" **受分析的测试影响的对象** " 部分显示指定的对象列表，这些对象在经过测试的对象执行后应比较数据更改。  
  
## <a name="test-case-settings"></a>测试用例设置  
在 " **测试用例设置** " 部分中，可以设置以下执行测试选项：  
  
### <a name="stop-test-execution-after-first-failure"></a>在第一次失败后停止测试执行  
指定在测试执行过程中发生错误时中断测试。  
  
-   如果选择 **"是"**，则在发生错误时，测试执行将中断。  
  
-   如果选择 " **否**"，则在出现错误后将继续执行测试。  
  
### <a name="perform-data-rollback"></a>执行数据回滚  
在测试执行后启用自动数据回滚。  
  
-   如果选择 **"是"**，则在执行测试后将丢失数据更改。  
  
-   如果选择 " **否**"，则将保存所有测试执行数据更改。  
  
### <a name="auxiliary-tables-saving-mode"></a>辅助表保存模式  
定义在测试执行过程中创建的辅助表的保存模式。 请参阅 [运行测试用例 ](../../ssma/oracle/running-test-cases-oracletosql.md) 中的辅助表的说明 &#40;OracleToSQL&#41;主题。  
  
-   如果选择 " **始终保存**"，则将始终存储辅助表数据以供以后使用。  
  
-   如果选择 " **如果表比较失败**，则仅在发生错误时才存储辅助表数据"。  
  
-   如果选择 " **始终删除**"，则测试执行后将始终删除辅助表。  
  
-   如果选择 " **在表比较失败时询问用户**"，则在发生错误时，用户可以选择必要的操作。  
  
单击 " **完成** " 按钮，将准备好的测试用例保存到 [使用测试存储库 (OracleToSQL)](./using-test-repositories-oracletosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[使用测试存储库 &#40;OracleToSQL&#41;](../../ssma/oracle/using-test-repositories-oracletosql.md)  
[&#40;OracleToSQL&#41;运行测试用例 ](../../ssma/oracle/running-test-cases-oracletosql.md)  
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
