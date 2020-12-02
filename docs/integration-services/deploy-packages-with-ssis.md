---
description: 使用 SSIS 部署包
title: 使用 SSIS 部署包 | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: quickstart
helpviewer_keywords:
- deployment tutorial [Integration Services]
- deploying packages [Integration Services]
- SSIS, tutorials
- Integration Services, tutorials
- deploying packages [Integration Services], installing
- SQL Server Integration Services, tutorials
- walkthroughs [Integration Services]
- deployment utility [Integration Services]
- deploying packages [Integration Services], configurations
ms.assetid: de18468c-cff3-48f4-99ec-6863610e5886
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d82aae4ee0195adca300d16bf9f2a2217c40a38c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123341"
---
# <a name="deploy-packages-with-ssis"></a>使用 SSIS 部署包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供将包轻松部署到其他计算机的工具。 部署工具还管理任何依赖项，如包需要的配置和文件。 在本教程中，您将了解如何使用这些工具在目标计算机上安装包及其依赖项。    
    
首先，您将执行任务为部署做好准备。 您将在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中创建一个新的 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 项目，再将现有的包和数据文件添加到该项目。 不是从头开始创建任何新包，而是仅使用专为本教程创建的已完成包。 将不修改本教程中包的功能；但是，在将包添加到项目后，您可能会发现在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中打开包并查看每个包的内容是很有用的。 通过检查包，您将了解有关包依赖项（如日志文件）的信息和有关包的其他有趣功能的信息。    
    
在为部署做准备时，您还将更新包以使用配置。 配置使得包属性和包对象在运行时是可更新的。 在本教程中，您将使用配置来更新日志文件和文本文件的连接字符串以及包所用的 XML 和 XSD 文件的位置。 有关详细信息，请参阅 [包配置](./packages/legacy-package-deployment-ssis.md) 和 [创建包配置](./packages/legacy-package-deployment-ssis.md)。    
    
在验证包是否在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中成功运行后，将创建用来安装包的部署捆绑。 部署捆绑将包括包文件和您添加到 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目的其他项、 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 自动包括的包依赖项以及您生成的部署实用工具。 有关详细信息，请参阅 [Create a Deployment Utility](./packages/legacy-package-deployment-ssis.md)。    
    
然后，将部署捆绑复制到目标计算机，并运行包安装向导以安装包和包依赖项。 包将安装在 msdb SQL Server 数据库中，支持文件和辅助文件将安装在文件系统中。 由于已部署的包使用配置，因此将配置更新为使用新的值，这样包就可以在新环境中成功运行。    
    
最后，通过使用执行包实用工具在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中运行包。    
    
本教程的目的是模拟实际部署中您可能遇到的问题的复杂性。 但是，如果不可能将包部署到其他计算机，仍可以学习本教程，方法是将包安装在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的本地实例上的 msdb 数据库中，再从同一实例上的 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 运行包。    

**学完本教程的估计时间：** 2 小时

## <a name="what-you-learn"></a>学习内容    
熟悉 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中的新工具、控件和功能的最好方法，就是使用它们。 本教程将引导您完成创建 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目再将包和其他所需文件添加到项目的步骤。 完成项目后，将创建部署捆绑，将该捆绑复制到目标计算机，然后在目标计算机上安装包。    
    
## <a name="prerequisites"></a>先决条件    
本教程适用于已经熟悉基本的文件系统操作，但对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中的新功能认识有限的用户。 为了更好地理解将在本教程中使用的基本 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 概念，可能会发现先完成以下 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 教程很有用： [SSIS 如何创建 ETL 包](../integration-services/ssis-how-to-create-an-etl-package.md)。    
    
### <a name="on-the-source-computer"></a>在源计算机上

将在其上创建部署捆绑的计算机必须安装下列组件：

- SQL Server。 （从[SQL Server 下载](https://www.microsoft.com/sql-server/sql-server-downloads)下载 SQL Server 的免费评估版或开发人员版。）

- 示例数据、已完成的包、配置和自述文件。 要将示例数据和课程包下载为 Zip 文件，请参阅 [SQL Server Integration Services 教程文件](https://www.microsoft.com/download/details.aspx?id=56827)。 为了防止意外更改发生，zip 文件中的大部分文件都是只读文件。 若要将输出写入到文件或更改输出，必须在文件属性中禁用只读属性。

-   AdventureWorks2014 示例数据库。 要下载 AdventureWorks2014 数据库，请从 [AdventureWorks sample databases](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)（AdventureWorks 示例数据库）下载 `AdventureWorks2014.bak`，并还原备份。  

-   必须具有在 AdventureWorks 数据库中创建和删除表的权限。
    
-   [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md) 。    
    
### <a name="on-the-destination-computer"></a>在目标计算机上

向其部署包的计算机 **必须安装下列组件：**    
    
- SQL Server。 （从[SQL Server 下载](https://www.microsoft.com/sql-server/sql-server-downloads)下载 SQL Server 的免费评估版或开发人员版。）

- 示例数据、已完成的包、配置和自述文件。 要将示例数据和课程包下载为 Zip 文件，请参阅 [SQL Server Integration Services 教程文件](https://www.microsoft.com/download/details.aspx?id=56827)。 为了防止意外更改发生，zip 文件中的大部分文件都是只读文件。 若要将输出写入到文件或更改输出，必须在文件属性中禁用只读属性。

-   AdventureWorks2014 示例数据库。 要下载 AdventureWorks2014 数据库，请从 [AdventureWorks sample databases](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)（AdventureWorks 示例数据库）下载 `AdventureWorks2014.bak`，并还原备份。  
    
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。    
    
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. 若要安装 SSIS，请参阅[安装 Integration Services](install-windows/install-integration-services.md)。
    
-   必须具有在 AdventureWorks 数据库中创建和删除表以及在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中运行 SSIS 包的权限。    
    
-   必须具有对 `msdb` [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 系统数据库中 `sysssispackages` 表的读写权限。    
    
如果您计划将包部署到在其上创建部署捆绑的计算机，则该计算机必须同时满足源计算机和目标计算机的要求。    
        
## <a name="lessons-in-this-tutorial"></a>本教程中的课程    
[第 1 课：准备创建部署捆绑](../integration-services/lesson-1-preparing-to-create-the-deployment-bundle.md)    
在本课中，将通过创建新的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目再将包和其他所需文件添加到该项目，为部署 ETL 解决方案做好准备。    
    
[第 2 课：在 SSIS 中创建部署捆绑](../integration-services/lesson-2-create-the-deployment-bundle-in-ssis.md)    
在本课中，将生成部署实用工具，并验证部署捆绑是否包括所需的文件。    
    
[第 3 课：安装 SSIS 包](../integration-services/lesson-3-install-ssis-packages.md)    
在本课中，将部署捆绑复制到目标计算机，安装包，再运行包。    
