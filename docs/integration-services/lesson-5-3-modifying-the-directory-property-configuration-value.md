---
description: 第 5-3 课：修改 Directory 属性配置值
title: 步骤 3：修改 Directory 属性配置值 | Microsoft Docs
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: ba2a091f-361c-4331-afe2-53b465164c36
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2a727a5c8d709e982582c26cff8d6cf09eb44ccc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88345693"
---
# <a name="lesson-5-3-modify-the-directory-property-configuration-value"></a>第 5-3 课：修改 Directory 属性配置值

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在此任务中，修改存储在 SSISTutorial.dtsConfig 文件中的配置设置，以设置包级别变量 `User::varFolderName` 的 Value 属性。 该变量更新 Foreach 循环容器的 Directory 属性。 修改后的值指向前一个任务中创建的 New Sample Data 文件夹。 修改配置设置并运行包后，将从配置文件中的变量更新 Directory 属性。 以前，Directory 属性值包含在包中。  
  
## <a name="modify-the-configuration-setting-of-the-directory-property"></a>修改 Directory 属性的配置设置  
  
1.  在记事本或其他文本编辑器中，找到并打开在前一个任务中使用“程序包配置向导”创建的 SSISTutorial.dtsConfig 配置文件。  
  
2.  更改 **ConfiguredValue** 元素的值，使其与上一个任务中创建的 **New Sample Data** 文件夹匹配。 请不要将路径用引号括起来。 如果 New Sample Data 文件夹位于驱动器的根级（例如，C:\\）上，则更新的 XML 应与下面的示例类似：  
  
    ```
    <?xml version="1.0"?>
    <DTSConfiguration>
      <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="DOMAIN\UserName" GeneratedFromPackageName="Lesson 5" GeneratedFromPackageID="{F4475E73-59E3-478F-8EB2-B10AFEE1D3FA}" GeneratedDate="6/10/2018 8:16:50 AM"/>
      </DTSConfigurationHeading>
      <Configuration ConfiguredType="Property" 
          Path="\Package.Variables[User::varFolderName].Properties[Value]" ValueType="String">
        <ConfiguredValue>C:\New Sample Data</ConfiguredValue>
      </Configuration>
    </DTSConfiguration>  
    ```

    在文件中，标题信息（GeneratedBy、GeneratedFromPackageID 和 GeneratedDate）将有所不同。 要注意的元素是 **Configuration** 元素。 变量 `User::varFolderName` 的 Value 属性现在包含 C:\New Sample Data。  
  
3.  保存更改，再关闭文本编辑器。  
  
## <a name="go-to-next-task"></a>转到下一个任务  
[步骤 4：测试第 5 课包](../integration-services/lesson-5-4-testing-the-lesson-5-tutorial-package.md)  
  
  
  
