---
title: 部署模型
description: 部署模型包以将模型的副本从一个 Master Data Services 环境移到另一个环境，或在环境中创建新模型。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deployment packages [Master Data Services], about deployment packages
- deployment packages [Master Data Services]
ms.assetid: 30085c08-034f-4efe-80fe-408f9091ff5c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 263de1026f5d63c240259852ca4acce1ed8f77d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350191"
---
# <a name="deploying-models-master-data-services"></a>部署模型 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，包是包含可部署模型结构以及来自模型的数据（可选）的 XML 文件。 使用模型包可以将模型的副本从一个 MDS 环境移到另一个环境，或者在现有的 MDS 环境中创建新模型。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]**MDSModelDeploy 工具** 与 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 或更高版本中创建的包向后兼容。  
  
## <a name="tools-for-deploying-models"></a>用于部署模型的工具  
 若要使用模型包，您可以根据需要使用以下三个工具之一。  
  
-   **MDSModelDeploy 工具**：若要创建和部署模型对象和数据，请使用 MDSModelDeploy.exe 工具。 如果你在安装 MDS 时选择了默认路径，则此工具将位于 *drive*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration。  
  
-   **模型部署向导**：若要创建和部署仅包含模型结构的包，请使用 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序中的向导。 您不能使用此向导来部署数据。  
  
-   **模型包编辑器**：若要编辑模型包，请使用启动模型包编辑器向导的 ModelPackageEditor.exe。 您可以使用此向导来编辑由 MDSModelDeploy 工具或模型部署向导创建的包。 如果你在安装 MDS 时选择了默认路径，则此工具将位于 *drive*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration。  
  
> [!IMPORTANT]  
>  可以使用 MDSModelDeploy 工具创建新模型、创建模型的克隆或更新现有模型及其数据。 如果使用 MDSModelDeploy 工具更新现有模型及其数据，并且该包不包含目标模型中存在的实体、属性或成员，则 MDSModelDeploy 不会从模型中删除此实体、属性或成员。  
  
## <a name="what-packages-contain"></a>包所包含的内容  
 模型包是使用 .pkg 扩展名进行保存的 XML 文件。 在您创建部署包时，可以决定是否包括数据。 如果决定包括数据，则必须选择要包括的数据。  
  
 所有模型对象都包括在一个包中。 这些对象是：  
  
-   实体  
  
-   属性  
  
-   特性组  
  
-   层次结构  
  
-   集合  
  
-   业务规则  
  
-   版本标志  
  
-   订阅视图  
  
 不包括文件属性以及用户和组权限。 在您部署模型后，必须手动更新这些内容。  
  
## <a name="sample-packages"></a>示例包  
 在您安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]时将包括示例包文件。 这些包文件位于安装了 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]的 Master Data Services\Samples\Packages 目录中。 在使用 MDSModelDeploy 工具部署这些示例包时，将创建并使用数据填充示例模型。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|通过使用 MDSModelDeploy 工具创建模型对象和/或数据的新的部署包。|[使用 MDSModelDeploy 创建模型部署包](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
|通过使用向导创建只包含模型对象的新的部署包。|[使用向导创建模型部署包](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md)|  
|通过使用 MDSModelDeploy 工具部署包含模型对象和数据的包。|[使用 MDSModelDeploy 部署模型部署包](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
|通过使用向导部署只包含模型对象的包。|[使用向导部署模型部署包](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)|  
|编辑模型部署包，以部署选定的模型部分而不是整个模型。|[编辑模型部署包](../master-data-services/edit-a-model-deployment-package.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [模型部署选项 (Master Data Services)](../master-data-services/model-deployment-options-master-data-services.md)  
  
  
