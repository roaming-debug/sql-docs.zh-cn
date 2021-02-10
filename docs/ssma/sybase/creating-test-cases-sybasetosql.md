---
description: 创建测试用例 (SybaseToSQL)
title: " (SybaseToSQL) 创建测试用例 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester component,Test Case Wizard
ms.assetid: b52dfd93-95af-4299-bc8f-83f2a7a6a518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 460ea84af51b872841eb82cd405cc29ebfe59e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056682"
---
# <a name="creating-test-cases-sybasetosql"></a>创建测试用例 (SybaseToSQL)
使用测试用例向导创建测试。 通过此向导，您可以通过选择已测试和已验证的对象并指定测试参数来创建测试用例。  
  
## <a name="starting-the-test-case-wizard"></a>启动测试用例向导  
若要启动测试用例向导，请单击 **"测试器" 菜单上** 的 "**新建测试用例 ...** "。  
  
启动后，向导将根据源 Sybase 服务器上) 的项目类型查找数据库 ssmatester2005db 或 ssmatester2008db (。 它是用于存储辅助对象的测试人员扩展架构。 如果测试用例向导找不到 ssmatester2005db 或 ssmatester2008db，则会显示建议创建测试人员扩展数据库的对话框窗口。  (在首次运行 SSMA 测试器期间通常会发生此情况。 )   
  
如果收到对话框窗口，请单击 **"是"** 在源服务器上创建 Sybase 测试器数据库。 然后，将出现一个新的对话框窗口，应在其中添加一个或多个要在其上查找新的测试人员数据库的设备。 单击 " **添加** " 以添加设备。 在 " **测试人员数据库的分配空间** " 对话框中，选择设备并指定测试人员数据库使用的大小。 此外，还可以为数据库日志设置单独的设备。 请注意，必须具有 Sybase 权限才能创建数据库。  
  
## <a name="overview-of-creating-test-cases-using-the-wizard"></a>使用向导创建测试用例概述  
创建测试用例的过程包括以下五个步骤：  
  
1.  [正在初始化 SybaseToSQL&#41;&#40;测试用例 ](../../ssma/sybase/initializing-test-cases-sybasetosql.md)。  
  
2.  [选择并配置要测试 &#40;SybaseToSQL&#41;的对象 ](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md)。  
  
3.  [选择并配置受影响的对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)。  
  
4.  [自定义调用顺序 &#40;SybaseToSQL&#41;](../../ssma/sybase/customizing-calls-order-sybasetosql.md)。  
  
5.  [完成测试用例准备 &#40;SybaseToSQL&#41;](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[测试迁移的数据库对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
