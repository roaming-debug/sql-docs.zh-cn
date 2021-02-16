---
description: 第 1-4 课 — 添加包配置
title: 步骤 4：添加包配置 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: e04a5321-63d5-4ec5-85b9-cb4eaf6c87f6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ca2d3bb08a1059970b8c72da5541b2ad3d9d1c8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354330"
---
# <a name="lesson-1-4---adding-package-configurations"></a>第 1-4 课 — 添加包配置

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在此任务中，将配置添加到每个包。 在运行时，配置更新包属性和包对象的值。  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供了各种配置类型。 您可以将配置存储在环境变量、注册表项、用户定义变量、 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 表和 XML 文件中。 为了提供更大的灵活性， [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 支持使用间接配置。 这意味着可使用环境变量指定配置的位置，而配置又指定实际值。 Deployment Tutorial 项目中的包使用 XML 配置文件和间接配置的组合。 一个 XML 配置文件可以包括多个属性的配置，并且在适当的时候还可以被多个包引用。 在本教程中，对于每个包都将使用单独的配置文件。  
  
配置文件通常包含敏感信息，如连接字符串。 因此，您应该使用访问控制列表 (ACL) 限制对存储这些文件的位置或文件夹的访问，并仅向被允许运行包的用户或帐户授予访问权限。 有关详细信息，请参阅 [访问包使用的文件](../integration-services/security/security-overview-integration-services.md#files)。  
  
这些包（即 DataTransfer 和 LoadXMLData，在上一任务中已添加到 Deployment Tutorial 项目）需要有配置，才能在部署到目标服务器后成功运行。 若要实现配置，请首先为 XML 配置文件创建间接配置，再创建 XML 配置文件。  
  
您将创建两个配置文件，即 DataTransferConfig.dtsConfig 和 LoadXMLData.dtsConfig。 这些文件包含更新包中属性（指定包所用的数据文件和日志文件的位置）的“名称-值”对。 稍后，作为部署过程中的一个步骤，您将更新配置文件中的值，以反映文件在目标计算机上的新位置。  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 识别出 DataTransferConfig.dtsConfig 和 LoadXMLData.dtsConfig 是 DataTransfer 和 LoadXMLData 包的依赖项，在下一课中创建部署捆绑包时它会自动包括配置文件。  
  
### <a name="to-create-indirect-configuration-for-the-datatransfer-package"></a>为 DataTransfer 包创建间接配置  

检查项目的当前部署模型，并根据需要将其设置为“包部署模型”。 在“项目”菜单上，单击“转换为包部署模型”
  
1.  在解决方案资源管理器中，双击 DataTransfer.dtsx。  
  
2.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，在控制流设计图面背景中的任意位置单击。  
  
3.  在 **SSIS** 菜单上，单击 **“包配置”** 。  
  
4.  在“包配置管理器”对话框中，选择“启用包配置”（如果尚未选择），再单击“添加”。  
  
5.  在包配置向导的欢迎页中，单击“下一步”。  
  
6.  在“选择配置类型”页上，选择“配置类型”列表中选择“XML 配置文件”，然后选择“配置位置存储在一个环境变量中”选项，再键入“DataTransfer”，或者选择列表中的“DataTransfer”环境变量。  
  
    > [!NOTE]  
    > 为了使该环境变量在列表中可用，您最好在添加该变量后重新启动计算机。 如果不希望重新启动计算机，则可以键入该环境变量的名称。  
  
7.  单击“下一步”。   
  
8.  在“完成向导”页上，在“配置名称”框中键入“DataTransfer EV Configuration”，在“预览”窗格中查看配置内容，然后单击“完成”。  
  
9. 关闭“包配置管理器”对话框。  
  
### <a name="to-create-the-xml-configuration-for-the-datatransfer-package"></a>为 DataTransfer 包创建 XML 配置  
  
1.  在解决方案资源管理器中，双击 DataTransfer.dtsx。  
  
2.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，在控制流设计图面背景中的任意位置单击。  
  
3.  在 **SSIS** 菜单上，单击 **“包配置”** 。  
  
4.  在“包配置管理器”对话框中，选中“启用包配置”复选框，然后单击“添加”。  
  
5.  在包配置向导的欢迎页中，单击“下一步”。  
  
6.  在“选择配置类型”页上，选择“配置类型”列表中的“XML 配置文件”后，单击“浏览”。  
  
7.  在“选择配置文件位置”对话框中，导航到 C:\DeploymentTutorial，在“文件名”框中键入“DataTransferConfig”，然后单击“保存”。  
  
8.  在“选择配置类型”页上，单击“下一步”。  
  
9. 在“选择要导出的属性”页上，依次展开“DataTransfer”、“连接管理器”、“部署 Tutorial 日志”和“属性”，然后选中“连接字符串”复选框。  
  
10. 在“连接管理器”内，展开“NewCustomers”后，选中“连接字符串”复选框。  
  
11. 单击“下一步”。   
  
12. 在“完成向导”页上，在“配置名称”框中键入“DataTransfer 配置”，查看配置的内容，然后单击“完成”。  
  
13. 在“包配置管理器”对话框中，验证是否第一个列出“DataTransfer EV 配置”，第二个列出“DataTransfer 配置”，然后单击“关闭”。  
  
### <a name="to-create-indirect-configuration-for-the-loadxmldata-package"></a>为 LoadXMLData 包创建间接配置  
  
1.  在解决方案资源管理器中，双击 LoadXMLData.dtsx。  
  
2.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，在控制流设计图面背景中的任意位置单击。  
  
3.  在 **SSIS** 菜单上，单击 **“包配置”** 。  
  
4.  在“包配置管理器”对话框中，单击“添加”。  
  
5.  在包配置向导的欢迎页中，单击“下一步”。  
  
6.  在“选择配置类型”页上，选择“配置类型”列表中的“XML 配置文件”，然后选择“配置位置存储在一个环境变量中”选项，并键入“LoadXMLData”，或者选择列表中的“LoadXMLData”环境变量。  
  
    > [!NOTE]  
    > 为了使该环境变量在列表中可用，您最好在添加该变量后重新启动计算机。  
  
7.  单击“下一步”。   
  
8.  在“完成向导”页上，在“配置名称”框中键入“LoadXMLData EV 配置”，查看配置的内容，然后单击“完成”。  
  
### <a name="to-create-the-xml-configuration-for-the-loadxmldata-package"></a>为 LoadXMLData 包创建 XML 配置  
  
1.  在解决方案资源管理器中，双击 LoadXMLData.dtsx。  
  
2.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，在控制流设计图面背景中的任意位置单击。  
  
3.  在 **SSIS** 菜单上，单击 **“包配置”** 。  
  
4.  在“包配置管理器”对话框中，选中“启用包配置”复选框，然后单击“添加”。  
  
5.  在包配置向导的欢迎页中，单击“下一步”。  
  
6.  在“选择配置类型”页上，选择“配置类型”列表中的“XML 配置文件”，然后单击“浏览”。  
  
7.  在“选择配置文件位置”对话框中，导航到 C:\DeploymentTutorial，在“文件名”框中键入“LoadXMLDataConfig”，然后单击“保存”。  
  
8.  在“选择配置类型”页上，单击“下一步”。  
  
9. 在“选择要导出的属性”页上，依次展开“LoadXMLData”、“可执行文件”、“加载 XML 数据”和“属性”，然后选中“[XMLSource].[XMLData]”和“[XMLSource].[XMLSchemaDefinition]”复选框。  
  
10. 单击“下一步”。   
  
11. 在“完成向导”页上，在“配置名称”框中键入“LoadXMLData 配置”，查看配置的内容，然后单击“完成”。  
  
12. 在“包配置管理器”对话框中，验证是否第一个列出“LoadXMLData EV 配置”，第二个列出“LoadXMLData 配置”，然后单击“关闭”。  
  
## <a name="next-task-in-lesson"></a>课程中的下一个任务  
[步骤 5：测试更新的包](../integration-services/lesson-1-5-testing-the-updated-packages.md)  
  
## <a name="see-also"></a>另请参阅  
[包配置](./packages/legacy-package-deployment-ssis.md)  
[创建包配置](./packages/legacy-package-deployment-ssis.md)  
[访问包使用的文件](../integration-services/security/security-overview-integration-services.md#files)