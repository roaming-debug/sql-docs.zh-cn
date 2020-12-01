---
description: 第 2-2 课 - 验证部署捆绑
title: 步骤 2：验证部署捆绑 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 6c13f5c9-c75e-4e52-94dc-2d2db2c578fe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4704aea54f73a0fa25db60ab9145a0ad3bc9359d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449651"
---
# <a name="lesson-2-2---verifying-the-deployment-bundle"></a>第 2-2 课 - 验证部署捆绑

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在第 1 课中，创建了 Deployment Tutorial 项目，并向该项目添加了包和辅助文件；在上一任务中，您为项目生成了部署实用工具。  
  
在此任务中，您将验证部署捆绑的内容。 部署捆绑是将复制到目标计算机并用来安装包的文件夹。 如果将默认值 bin\Deployment 用作部署实用工具的位置，则在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目中部署捆绑是 Deployment Tutorial 文件夹中的 Bin\Deployment 文件夹。  
  
### <a name="to-verify-the-content-of-deployment-bundle"></a>验证部署捆绑的内容  
  
1.  找到计算机上的 bin\Deployment 文件夹。  
  
2.  验证是否存在下列文件：  
  
    -   DataTransfer.dtsx  
  
    -   datatransferconfig.dtsconfig  
  
    -   Deployment Tutorial.SSISDeploymentManifest  
  
    -   LoadXMLData.dtsx  
  
    -   loadxmldataconfig.dtsconfig  
  
    -   NewCustomers.txt  
  
    -   orders.xml  
  
    -   orders.xsd  
  
    -   Readme.txt  
  
3.  右键单击“Deployment Tutorial.SSISDeploymentManifest”，指向“打开方式”，再单击“Internet Explorer”。 也可以在文本编辑器（如记事本）中打开该文件。 文件的 XML 代码应该如下所示：  
  
    `<?xml version="1.0"?><DTSDeploymentManifest GeneratedBy="Domain\UserName" GeneratedFromProjectName="Deployment Tutorial" GeneratedDate="2006-02-24T13:29:02.6537669-08:00" AllowConfigurationChanges="true"><Package>DataTransfer.dtsx</Package><Package>LoadXMLData.dtsx</Package><ConfigurationFile>datatransferconfig.dtsconfig</ConfigurationFile><ConfigurationFile>loadxmldataconfig.dtsconfig</ConfigurationFile><MiscellaneousFile>Readme.txt</MiscellaneousFile><MiscellaneousFile>orders.xml</MiscellaneousFile><MiscellaneousFile>NewCustomers.txt</MiscellaneousFile><MiscellaneousFile>orders.xsd</MiscellaneousFile></DTSDeploymentManifest>`  
  
4.  验证确保“AllowConfigurationChanges”属性的值为“true”，XML 包括两个包中每个包的 **Package** 元素、四个非包文件中每个文件的 **MiscellaneousFile** 元素以及两个 XML 配置文件中每个文件的 **ConfigurationFile** 元素。  
  
5.  退出 Internet Explorer 或文本编辑器。  
  
## <a name="next-lesson"></a>下一课  
[第 3 课：安装 SSIS 包](../integration-services/lesson-3-install-ssis-packages.md)  
  
  
  
