---
description: 第 2 课：使用 SSIS 添加循环
title: 第 2 课：使用 SSIS 添加循环 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 01f2ed61-1e5a-4ec6-b6a6-2bd070c64077
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0805b8709e2a0a689538f558672694c96ddff39b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88390663"
---
# <a name="lesson-2-add-looping-with-ssis"></a>第 2 课：使用 SSIS 添加循环

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在[第 1 课中：使用 SSIS 创建项目和基本包](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md)，创建了一个从单个平面文件源中提取数据的包。 然后使用查找转换转换了数据。 最后，程序包将数据加载到 AdventureWorksDW2012 示例数据库中的 FactCurrencyRate 事实数据表的副本中。  
  
提取、转换和加载 (ETL) 过程通常从多个平面文件源中提取数据。 从多个源提取数据需要采用迭代控制流。 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 可以轻松将迭代或循环添加到包。  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 为循环遍历包提供了两种容器类型：Foreach 循环容器和 For 循环容器。 Foreach 循环容器使用枚举器执行循环，而 For 循环容器则通常使用变量表达式。 本课使用 Foreach 循环容器。  
  
Foreach 循环容器使包能够对指定枚举器的每个成员重复执行控制流。 使用 Foreach 循环容器，可以枚举：  
  
-   ADO 记录集行  
  
-   ADO .Net 架构信息  
  
-   文件和目录结构  
  
-   系统、包和用户变量  
  
-   变量中的可枚举对象  
  
-   集合中的项  
  
-   XML Path 语言 (XPath) 表达式中的节点  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 管理对象 (SMO)  
  
在本课程中，将修改第 1 课的示例 ETL 包以使用 Foreach 循环容器，并为该包设置用户定义的包变量。 然后，该变量用于循环访问示例文件夹中的匹配文件。   
  
在本课中，将不修改数据流，而只修改控制流。  
  
> [!NOTE]  
> 如果尚不具备必备条件，请参阅[第 1 课必备条件](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md#prerequisites)。

## <a name="lesson-tasks"></a>课程任务  
本课程包含以下任务：  
  
-   [步骤 1：复制第 1 课包](../integration-services/lesson-2-1-copying-the-lesson-1-package.md)  
  
-   [步骤 2：添加和配置 Foreach 循环容器](../integration-services/lesson-2-2-adding-and-configuring-the-foreach-loop-container.md)  
  
-   [步骤 3：修改平面文件连接管理器](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
-   [步骤 4：测试第 2 课教程包](../integration-services/lesson-2-4-testing-the-lesson-2-tutorial-package.md)  
  
## <a name="start-the-lesson"></a>开始课程  
[步骤 1：复制第 1 课包](../integration-services/lesson-2-1-copying-the-lesson-1-package.md)  
  
## <a name="see-also"></a>另请参阅  
[For 循环容器](../integration-services/control-flow/for-loop-container.md)  
  
  
  
