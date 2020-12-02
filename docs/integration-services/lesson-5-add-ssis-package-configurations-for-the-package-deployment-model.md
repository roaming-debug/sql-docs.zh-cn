---
description: 第 5 课：添加包部署模型的 SSIS 包配置
title: 第 5 课：添加包部署模型的 SSIS 包配置 | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 1c10dd54-67cb-4b63-9e4d-aa6ff0452ecb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c50bdc080ca6437cdb2291f901c429526976ab88
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88466601"
---
# <a name="lesson-5-add-ssis-package-configurations-for-the-package-deployment-model"></a>第 5 课：添加包部署模型的 SSIS 包配置

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



包配置允许从开发环境外部设置运行时属性和变量。 配置允许您开发灵活且易于部署和分发的包。 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供了以下配置类型：  
  
-   XML 配置文件  
  
-   环境变量  
  
-   注册表项  
  
-   父包变量  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 表  
  
在本课中，将修改在[第 4 课：使用 SSIS 添加错误流重定向](../integration-services/lesson-4-add-error-flow-redirection-with-ssis.md)中创建的示例 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包，以便使用包部署模型并利用包配置。 还可以复制本教程附带的已完成的第 4 课包。 

使用“程序包配置向导”，可创建一个 XML 配置，以便更新 Foreach 循环容器的 Directory 属性。 使用映射到 Directory 属性的包级别变量。 创建配置文件后，将变量的值从开发环境外部修改为新的示例数据文件夹路径。 再次运行包时，配置文件将填充变量值，而变量将更新 **Directory** 属性。 然后，包将循环访问新数据文件夹中的文件，而不是原始硬编码文件夹中的文件。  
  
> [!NOTE]
> 如果尚不具备必备条件，请参阅[第 1 课必备条件](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md#prerequisites)。
  
## <a name="lesson-tasks"></a>课程任务  
本课程包含以下任务：  
  
-   [步骤 1：复制第 4 课包](../integration-services/lesson-5-1-copying-the-lesson-4-package.md)  
  
-   [步骤 2：启用和配置包配置](../integration-services/lesson-5-2-enabling-and-configuring-package-configurations.md)  
  
-   [步骤 3：修改 directory 属性配置值](../integration-services/lesson-5-3-modifying-the-directory-property-configuration-value.md)  
  
-   [步骤 4：测试第 5 课包](../integration-services/lesson-5-4-testing-the-lesson-5-tutorial-package.md)  
  
## <a name="start-the-lesson"></a>开始课程  
  
-   [步骤 1：复制第 4 课包](../integration-services/lesson-5-1-copying-the-lesson-4-package.md)  
  
  
  
