---
description: 运行测试用例 (OracleToSQL)
title: " (OracleToSQL) 运行测试用例 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: fc208cdb-7373-4f6b-8f6c-cdff9d3dcd02
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: bc811575dd845be43aef7a9398d8b4fb3458db1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067712"
---
# <a name="running-test-cases-oracletosql"></a>运行测试用例 (OracleToSQL)
当 SSMA 测试人员运行测试用例时，它将执行选择用于测试的对象，并创建有关验证结果的报表。 如果两个平台上的结果相同，则测试已成功。 Oracle 和之间对象的对应关系 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据当前 SSMA 项目的架构映射设置来确定。  
  
成功测试的必需要求是所有 Oracle 对象都转换并加载到目标数据库。 此外，还应迁移表数据，以便同步两个平台上的表的内容。  
  
## <a name="run-test-case"></a>运行测试用例  
运行准备的测试用例：  
  
1.  单击“运行”按钮。  
  
2.  在 " **连接到 Oracle** " 对话框中，输入连接信息，然后单击 " **连接**"。  
  
测试完成后，将创建测试用例报表。 单击 " **报表** " 按钮以查看 [测试用例报表](viewing-test-case-reports-oracletosql.md)。 测试 (测试用例报告) 的结果会自动存储在 [测试结果存储库](using-test-repositories-oracletosql.md) 中供以后使用。  
  
## <a name="test-case-execution-steps"></a>测试用例执行步骤  
  
### <a name="prerequisites"></a>必备条件  
SSMA 测试人员检查测试开始之前是否满足所有先决条件。 如果未满足某些条件，则会显示错误消息。  
  
### <a name="initialization"></a>初始化  
在此步骤中，SSMA 测试人员) 在 Oracle 服务器的 SSMATESTER_ORACLE 架构中创建辅助对象 (表、触发器和视图。 它们允许在选择进行验证的受影响对象中进行跟踪更改。  
  
假定验证的表名为 USER_TABLE。 对于此类表，将在 Oracle 中创建以下辅助对象。  
  
|名称|类型|说明|  
|-|-|-|  
|USER_TABLE $ .Trg|触发器|触发审核已验证表中的更改。|  
|USER_TABLE $ AUD|表|其中保存已删除和覆盖的行的表。|  
|USER_TABLE $ AUDID|表|其中保存新行和更改行的表。|  
|USER_TABLE|view|简化表修改的表示形式。|  
|USER_TABLE $ NEW|view|简化的插入和覆盖的行的表示形式。|  
|USER_TABLE $ NEW_ID|view|已插入和已更改行的标识。|  
|USER_TABLE $ OLD|view|简化的已删除和覆盖的行的表示形式。|  
  
以下对象是在的已验证表的架构中创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
|名称|类型|说明|  
|-|-|-|  
|USER_TABLE $ .Trg|触发器|触发审核已验证表中的更改。|  
  
在 ssmatesterdb 数据库的中创建了以下对象 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
|名称|类型|说明|  
|-|-|-|  
|USER_TABLE $ Aud|表|其中保存已删除和覆盖的行的表。|  
|USER_TABLE $ AudID|表|其中保存新行和更改行的表。|  
|USER_TABLE|view|简化表修改的表示形式。|  
|USER_TABLE $ new|view|简化的插入和覆盖的行的表示形式。|  
|USER_TABLE $ new_id|view|已插入和已更改行的标识。|  
|USER_TABLE $ old|view|简化的已删除和覆盖的行的表示形式。|  
  
### <a name="test-object-calls"></a>测试对象调用  
在此步骤中，SSMA 测试人员调用为测试选择的每个对象，比较结果，并显示报表。  
  
### <a name="finalization"></a>定稿  
在终止 SSMA 测试过程中，将清理在 **初始化** 步骤中创建的辅助对象。  
  
## <a name="next-step"></a>下一步  
[查看测试用例报表 &#40;OracleToSQL&#41;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md)  
  
## <a name="see-also"></a>另请参阅  
[选择并配置要测试 &#40;OracleToSQL&#41;的对象 ](../../ssma/oracle/selecting-and-configuring-objects-to-test-oracletosql.md)  
[选择并配置受影响的对象 &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
