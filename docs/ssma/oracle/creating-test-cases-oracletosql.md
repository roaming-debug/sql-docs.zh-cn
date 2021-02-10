---
description: 创建测试用例 (OracleToSQL)
title: " (OracleToSQL) 创建测试用例 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Test Case Wizard
ms.assetid: 22f38901-ec35-4707-a911-784e6ad8dafb
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b0465aeb3d014213891796c33c4cacc2ec0e4256
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058532"
---
# <a name="creating-test-cases-oracletosql"></a>创建测试用例 (OracleToSQL)
使用测试用例向导创建测试。 通过此向导，您可以通过选择已测试和已验证的对象并指定测试参数来创建测试用例。  
  
## <a name="starting-the-test-case-wizard"></a>启动测试用例向导  
若要启动测试用例向导，请单击 **"测试器" 菜单上** 的 "**新建测试用例 ...** "。  
  
启动后，向导将在源 Oracle 服务器上查找架构 SSMATESTER_ORACLE。 它是用于存储辅助对象的测试人员扩展架构。 如果测试用例向导找不到 SSMATESTER_ORACLE，则将显示建议创建架构的对话框窗口。  (在首次运行 SSMA 测试器期间通常会发生此情况。 )   
  
如果收到对话框窗口，请单击 **"是"** 在源服务器上创建 SSMATESTER_ORACLE 架构。 请注意，你必须拥有 Oracle 权限才能创建新用户并在此用户的架构中创建对象。  
  
## <a name="overview-of-creating-test-cases-using-the-wizard"></a>使用向导创建测试用例概述  
创建测试用例的过程包括以下五个步骤：  
  
1.  [&#40;OracleToSQL&#41;初始化测试用例 ](../../ssma/oracle/initializing-test-cases-oracletosql.md)  
  
2.  [选择并配置要测试 &#40;OracleToSQL&#41;的对象 ](../../ssma/oracle/selecting-and-configuring-objects-to-test-oracletosql.md)  
  
3.  [选择并配置受影响的对象 &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
  
4.  [自定义调用顺序 &#40;OracleToSQL&#41;](../../ssma/oracle/customizing-calls-order-oracletosql.md)  
  
5.  [完成测试用例准备 &#40;OracleToSQL&#41;](../../ssma/oracle/finishing-test-case-preparation-oracletosql.md)  
  
## <a name="see-also"></a>另请参阅  
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
