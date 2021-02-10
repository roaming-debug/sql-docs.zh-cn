---
description: 运行测试用例 (SybaseToSQL)
title: " (SybaseToSQL) 运行测试用例 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Execution Steps
ms.assetid: 195ffdef-cfde-4bf4-a3ae-e7402bb07972
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8cb7521c0f58526c6dbc9e0f5fa7bfae848f6d38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017608"
---
# <a name="running-test-cases-sybasetosql"></a>运行测试用例 (SybaseToSQL)
当 SSMA 测试人员运行测试用例时，它将执行选择用于测试的对象，并创建有关验证结果的报表。 如果两个平台上的结果相同，则测试已成功。 Sybase 和之间对象的对应关系 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据当前 SSMA 项目的架构映射设置来确定。  
  
成功测试的必需要求是所有 Sybase 对象都转换并加载到目标数据库。 此外，还应迁移表数据，以便同步两个平台上的表的内容。  
  
## <a name="run-test-case"></a>运行测试用例  
运行准备的测试用例：  
  
1.  单击“运行”按钮。  
  
2.  在 " **连接到 Sybase** " 对话框中，输入连接信息，然后单击 " **连接**"。  
  
测试完成后，将创建测试用例报表。 单击 " **报告** " 按钮以查看 [查看测试用例报表 &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md)"。 测试 (测试用例报告) 的结果会自动存储在 [使用测试存储库 &#40;SybaseToSQL&#41;](../../ssma/sybase/using-test-repositories-sybasetosql.md) 以供以后使用。  
  
## <a name="test-case-execution-steps"></a>测试用例执行步骤  
  
### <a name="prerequisites"></a>必备条件  
SSMA 测试人员检查测试开始之前是否满足所有先决条件。 如果未满足某些条件，则会显示错误消息。  
  
### <a name="initialization"></a>初始化  
在此步骤中，SSMA 测试器会在 Sybase 和)  (表、触发器和视图中创建辅助对象 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果表比较模式 **仅更改**，则它们允许跟踪在受影响的表中所做的更改。  
  
假定验证的表名为 USER_TABLE。 对于此类表，将在 Sybase 中创建以下辅助对象。  
  
以下对象在 SSMATESTER2005db 或 SSMATESTER2008db 数据库中的 Sybase 处创建，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ssmatesterdb_syb 数据库中创建。  
  
|名称|类型|说明|  
|--------|--------|---------------|  
|USER_TABLE $ .Trg|触发器|触发审核已验证表中的更改。|  
|USER_TABLE $ Aud|表|其中保存已删除和覆盖的行的表。|  
|USER_TABLE $ AudID|表|其中保存新行和更改行的表。|  
|USER_TABLE|查看|简化表修改的表示形式。|  
|USER_TABLE $ new|查看|简化的插入和覆盖的行的表示形式。|  
|USER_TABLE $ new_id|查看|已插入和已更改行的标识。|  
|USER_TABLE $ old|查看|简化的已删除和覆盖的行的表示形式。|  
  
以下对象在 Sybase 和的已验证表的数据库中创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
|名称|类型|说明|  
|--------|--------|---------------|  
|USER_TABLE $ .Trg|触发器|触发审核已验证表中的更改。|  
  
### <a name="test-object-calls"></a>测试对象调用  
在此步骤中，SSMA 测试人员调用为测试选择的每个对象，比较结果，并显示报表。  
  
### <a name="finalization"></a>定稿  
在终止 SSMA 测试过程中，将清理在 **初始化** 步骤中创建的辅助对象。  
  
## <a name="next-step"></a>下一步  
[查看测试用例报表 &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md)  
  
## <a name="see-also"></a>另请参阅  
[选择并配置要测试 &#40;SybaseToSQL&#41;的对象 ](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md)  
[选择并配置受影响的对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
[测试迁移的数据库对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
