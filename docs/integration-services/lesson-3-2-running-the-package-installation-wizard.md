---
description: 第 3-2 课 — 运行包安装向导
title: 步骤 2：运行包安装向导 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f30bc07edc2d6d513eb9078e1758caf99a2822cf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88472042"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>第 3-2 课 — 运行包安装向导

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在此任务中，将运行包安装向导，将包从 Deployment Tutorial 项目部署到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的实例。 只能将包安装在 msdb [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库的 sysssispackages 表中，而部署捆绑包括的支持文件将被部署到文件系统。  
  
包安装向导将引导您完成安装和配置包的步骤。 将包安装到目标计算机（向其复制部署捆绑的计算机）上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例。 您还将创建文件夹 C:\DeploymentTutorialInstall，向导将在该文件夹中安装非包文件。  
  
在前面的课程中，已经修改教程中的包，以便使用配置。 使用包安装向导，您将编辑这些配置，使得包能够在安装到的环境中成功运行。  
  
### <a name="to-install-the-packages"></a>安装包  
  
1.  在目标计算机上，找到部署捆绑。  
  
    如果将默认值 bin\Deployment 用作部署实用工具的位置，则部署捆绑是 Deployment Tutorial 项目中的 Deployment 文件夹。  
  
2.  在 Deployment 文件夹中，双击清单文件 Deployment Tutorial.SSISDeploymentManifest。  
  
3.  在包安装向导的“欢迎”页中，单击“下一步”。  
  
4.  在“部署 SSIS 包”页上，选择“SQL Server 部署”选项，选中“安装后验证包”复选框，再单击“下一步”。  
  
5.  在“指定目标 SQL Server”页上，在“服务器名称”框中指定 **(local)**。  
  
6.  如果 SQL Server 的实例支持 Windows 身份验证，请选择“使用 Windows 身份验证”；否则，选择“使用 SQL Server 身份验证”，并提供用户名和密码。  
  
7.  验证是否清除了“依靠服务器存储进行加密”复选框。  
  
8.  单击“下一步”。  
  
9. 在“选择安装文件夹”页上，单击“浏览”。  
  
10. 在“浏览文件夹”对话框中，展开“我的电脑”，再单击“本地磁盘 (C:)”。  
  
11. 单击“新建文件夹”，并将新文件夹的默认名称“新建文件夹”替换为 **DeploymentTutorialInstall**。  
  
    > [!IMPORTANT]  
    > 在配置所使用的环境变量的值中，将引用此名称。 文件夹的名称与引用必须匹配，否则包无法运行。  
  
12. 单击“确定”。   
  
13. 在“选择安装文件夹”页上，验证“文件夹”框中是否包含 **C:\DeploymentTutorialInstall**，再单击“下一步”。  
  
14. 在“确认安装”页上，单击“下一步”。  
  
    向导将安装包。 完成安装后，将打开“配置包”页。  
  
15. 在“配置包”页上，验证“配置文件”框是否列出了 datatransferconfig.dtsconfig 和 loadxmldataconfig.dtsconfig。  
  
16. 在“配置文件”列表中，单击 **datatransferconfig.dtsconfig**，展开“配置”框的“路径”列中的“属性”，再用下列值更新“值”列：  
  
    |属性|值|更新后的值|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. 在“配置文件”列表中，单击 loadxmldataconfig.dtsconfig，展开“配置”框的“路径”列中的“属性”，再用下列值更新“值”列：  
  
    |属性|值|更新后的值|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. 在“包验证”页上，查看所安装的每个包的验证结果，再单击“下一步”。  
  
    由于目标计算机上环境变量的值与开发计算机上环境变量的值不同，因此会在“包验证”页上出现多个警告。 您可能会看到下列四个警告：  
  
    -   配置文件 "C:\DeploymentTutorial\DataTransferConfig.dtsConfig" 无效。 请检查此配置文件名。  
  
    -   包中至少有一个配置条目无法加载。 请检查配置条目和以前的警告，查看配置失败的有关说明。  
  
    -   配置文件 "C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig" 无效。 请检查此配置文件名。  
  
    -   包中至少有一个配置条目无法加载。 请检查配置条目和以前的警告，查看配置失败的有关说明。  
  
    这些警告不影响包的安装。  
  
    如果在“部署 SSIS 包”页上未选择“安装后验证包”选项，则不会打开“包验证”页，而且向导不显示有关验证的安装后信息。  
  
19. 在“完成包安装向导”页上，阅读安装摘要，再单击“完成”。  
  
    > [!NOTE]  
    > 此时，创建一个临时日志文件，供在包验证时使用。 运行包时不使用此文件。  
  
## <a name="next-task-in-lesson"></a>课程中的下一个任务  
[步骤 3：测试已部署的包](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>另请参阅  
[Integration Services 服务（SSIS 服务）](../integration-services/service/integration-services-service-ssis-service.md)  
