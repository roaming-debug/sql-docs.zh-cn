---
description: 查看测试用例报表 (OracleToSQL)
title: 查看测试用例报表 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8da14323-9dd6-4019-bf79-3e8b972a9bc0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2666b73a4ce335504352c2a838ab31ce13838350
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068749"
---
# <a name="viewing-test-case-reports-oracletosql"></a>查看测试用例报表 (OracleToSQL)
"测试用例" 报表显示测试验证结果和常规测试信息。 如果测试失败，还会显示有关已验证对象中的任何不匹配数据的信息。  
  
## <a name="report-structure"></a>报表结构  
报表顶部显示以下统计信息：  
  
-   已测试对象的总数和测试成功的对象数。  
  
-   已验证的表和外键的总数，以及成功匹配的表和外键的数目。  
  
-   测试用例的开始时间、结束时间和执行所用的总时间。  
  
报表的其余部分显示四个类别中的信息：  
  
**先决条件错误**  
显示在 **先决条件步骤** 中发生的任何错误。 通常情况下，会跳过它。  
  
**初始化**  
显示 **成功** 或 **失败** 的执行状态。  
  
**测试对象结果**  
结果比较 (成功或失败) 以及 SSMA 测试人员在失败情况下检测到的不匹配。  
  
**定稿**  
显示 **成功** 或 **失败** 的执行状态。  
  
## <a name="see-also"></a>另请参阅  
[&#40;OracleToSQL&#41;运行测试用例 ](../../ssma/oracle/running-test-cases-oracletosql.md)  
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
