---
description: SSIS 如何创建 ETL 包
title: SSIS 如何创建 ETL 包 | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: quickstart
helpviewer_keywords:
- SSIS, tutorials
- packages [Integration Services], tutorials
- Integration Services, tutorials
- SQL Server Integration Services, tutorials
- logs [Integration Services], tutorials
- walkthroughs [Integration Services]
ms.assetid: d6d5bb1f-4cb1-4605-9cd6-f60b858382c4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f83cb45dc060bc2877cf316e4d19baa073a9e6e0
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88457023"
---
# <a name="ssis-how-to-create-an-etl-package"></a>SSIS 如何创建 ETL 包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在本教程中，你将学习如何使用 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器创建一个简单的 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包。 所创建的包将从平面文件提取数据，重新设置数据的格式，然后将已重新设置格式的数据插入到事实数据表中。 在下列课程中，将扩展包以阐释循环、包配置、日志记录和错误流。  
  
在安装教程所用的示例数据的同时，也会安装将在教程的每一课中创建的完整的包版本。 使用完整的包，您可以按需要跳过前面几课而从后面的课程开始学习教程。 如果本教程是你第一次使用包或新的开发环境，我们建议从第 1 课开始学习。  

## <a name="what-is-sql-server-integration-services-ssis"></a>什么是 SQL Server Integration Services (SSIS)？

[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] (SSIS) 是一个可用于生成高性能数据集成解决方案的平台，其中包括数据仓库的提取、转换和加载 (ETL) 包。 SSIS 包括用于生成和调试包的图形工具和向导；用于执行 FTP 操作等工作流函数、执行 SQL 语句和发送电子邮件的任务；用于提取和加载数据的数据源和目标；用于清理、聚合、合并和复制数据的转换；用于管理包执行和存储的管理数据库 `SSISDB`；以及用于对 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 对象模型进行编程的应用程序编程接口 (API)。  

## <a name="what-you-learn"></a>学习内容  
熟悉 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中的新工具、控件和功能的最好方法，就是使用它们。 本教程将指导使用 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器创建一个简单的 ETL 包，其中包含循环、配置、错误流逻辑和日志记录。  
  
## <a name="prerequisites"></a>先决条件  
本教程适用于熟悉基本数据库操作，但对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中的新功能认识有限的用户。  

若要运行本教程，必须安装下列组件：  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]。 若要安装 SQL Server 和 SSIS，请参阅[安装 Integration Services](install-windows/install-integration-services.md)。

-   AdventureWorksDW2012 示例数据库。 若要下载 AdventureWorksDW2012 数据库，请从 [AdventureWorks sample databases](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)（AdventureWorks 示例数据库）下载 `AdventureWorksDW2012.bak`，并还原备份。  

-   示例数据文件。 示例数据与 [!INCLUDE[ssIS](../includes/ssis-md.md)] 课程包一起提供。 要将示例数据和课程包下载为 Zip 文件，请参阅 [SQL Server Integration Services 教程文件](https://www.microsoft.com/download/details.aspx?id=56827)。

    - 为了防止意外更改发生，zip 文件中的大部分文件都是只读文件。 若要将输出写入到文件或更改输出，必须在文件属性中禁用只读属性。
    - 示例包假定数据文件位于文件夹 `C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Creating a Simple ETL Package` 中。 如果将下载内容解压缩到其他位置，必须在示例包中的多个位置更新文件路径。

## <a name="lessons-in-this-tutorial"></a>本教程中的课程  
[第 1 课：使用 SSIS 创建项目和基本包](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md)  
在本课中，将创建一个简单的 ETL 包，从单个平面文件中提取数据，再使用查找转换转换数据，最后将所得结果加载到目标事实数据表中。  
  
[第 2 课：使用 SSIS 添加循环](../integration-services/lesson-2-adding-looping-with-ssis.md)  
在本课中，将扩展第 1 课中创建的包，以便利用新增的循环功能，将多个平面文件提取到单个数据流进程中。  
  
[第 3 课：使用 SSIS 添加日志记录](../integration-services/lesson-3-add-logging-with-ssis.md)  
在本课中，将扩展第 2 课中创建的包，以便利用新增的日志记录功能。  
  
[第 4 课：使用 SSIS 添加错误流重定向](../integration-services/lesson-4-add-error-flow-redirection-with-ssis.md)  
在本课中，将扩展第 3 课中创建的包，以便利用新增的错误输出配置。  
  
[第 5 课：添加包部署模型的 SSIS 包配置](../integration-services/lesson-5-add-ssis-package-configurations-for-the-package-deployment-model.md)  
在本课中，将扩展第 4 课中创建的包，以便利用新增的包配置选项。  
  
[第 6 课：在 SSIS 中对项目部署模型使用参数](../integration-services/lesson-6-using-parameters-with-the-project-deployment-model-in-ssis.md)  
在本课中，将扩展第 5 课中创建的包，以便将新参数用于项目部署模型。  
  
  
  
