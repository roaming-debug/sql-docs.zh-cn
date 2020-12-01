---
description: 第 1-1 课：创建新的 Integration Services 项目
title: 步骤 1：创建新的 Integration Services 项目 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f14521b5-941e-443b-8f5e-385f98e37fbf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 575353cd2cf770ed42d439fd31647ccaef3e01bd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449743"
---
# <a name="lesson-1-1-create-a-new-integration-services-project"></a>第 1-1 课：创建新的 Integration Services 项目

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



若要在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中创建包，第一步是创建一个 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。 此示例项目包含用于生成数据转换解决方案的数据源、数据源视图和包的模板。  
  
将本 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 教程中创建的包用于解释受区域设置影响的数据的值。 如果计算机未配置为使用区域选项“英语(美国)”，则需要在包中设置其他属性。 

在第 2 课到第 6 课中使用的包将从你在本课程中创建的包中复制。  
  
> [!NOTE]  
> 如果尚不具备必备条件，请参阅[第 1 课必备条件](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md#prerequisites)。

## <a name="create-a-new-integration-services-project"></a>创建新的 Integration Services 项目  
  
1.  在 Windows“启动”菜单中，搜索并选择“Visual Studio (SSDT)”。  
  
2.  在 Visual Studio 中，依次选择“文件” > “新建” > “项目”以创建一个新的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目  。  
  
3.  在“新建项目”对话框中，展开“已安装”下的“商业智能”节点，并在“模板”窗格中选择“Integration Services 项目”。  
  
4.  在“名称”框中，将默认名称更改为 **SSIS Tutorial**。 要使用已存在的文件夹，请清除“创建解决方案目录”复选框。  
  
5.  接受默认位置，或选择“浏览”，以浏览并找到要使用的文件夹。 在“项目位置”对话框中，依次选择文件夹和“选择文件夹”。  
  
6.  选择“确定” 。  
  
    默认情况下，将创建一个名为 Package.dtsx 的空包，并将该包添加到项目中的“SSIS 包”之下。  
  
7.  在“解决方案资源管理器”中，右键单击“Package.dtsx”，再选择“重命名”，将默认包重命名为 Lesson 1.dtsx。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 2：添加和配置平面文件连接管理器](../integration-services/lesson-1-2-adding-and-configuring-a-flat-file-connection-manager.md)  
  
