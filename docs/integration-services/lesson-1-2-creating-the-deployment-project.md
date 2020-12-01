---
description: 第 1-2 课 - 创建部署项目
title: 步骤 2：创建部署项目 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 59990fe2-7036-4e9c-8efc-6ece9e66eda7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 025e601dfb6f062246d1edcb1799c1daf2b69886
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449661"
---
# <a name="lesson-1-2---creating-the-deployment-project"></a>第 1-2 课 - 创建部署项目

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]中，可部署的单元是 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。 必须先创建一个新的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目，再将所有包和要与包一起部署的任何辅助文件添加到该项目，才能部署包。  
  
### <a name="to-create-the-integration-services-project"></a>创建 Integration Services 项目  
  
1.  单击“开始”，依次指向“所有程序”和“Microsoft SQL Server”，然后依次单击“SQL Server”和“SQL Server Data Tools”。  
  
2.  在“文件”菜单上，指向“新建”，然后单击“项目”创建新的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
3.  在“新建项目”对话框的“模板”窗格中，选择“Integration Services 项目”。  
  
4.  在“名称”框中，将默认名称更改为“Deployment Tutorial”。 或者，清除“创建解决方案的目录”复选框。  
  
5.  接受默认位置，或单击“浏览”找到要使用的文件夹。  
  
6.  在“项目位置”对话框中，单击文件夹，再单击“打开”。  
  
7.  单击“确定”。   
  
8.  默认情况下，将创建一个名为 Package.dtsx 的空包，并将该包添加到项目中。 但是您将不使用此包；相反您将现有的包添加到项目。 由于项目中的所有包都包括在部署中，因此您应该删除 Package.dtsx。 若要删除它，右键单击它，再单击“删除”。  
  
## <a name="next-task-in-lesson"></a>课程中的下一个任务  
[步骤 3：添加包和其他文件](../integration-services/lesson-1-3-adding-packages-and-other-files.md)  
  
## <a name="see-also"></a>另请参阅  
[Integration Services (SSIS) 项目](~/integration-services/integration-services-ssis-projects-and-solutions.md)  
  
  
  

