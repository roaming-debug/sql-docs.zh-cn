---
description: 为查找转换创建和部署缓存
title: 为查找转换创建和部署缓存 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- creating cache files for Lookup transformation
- deploying cache files for Lookup transformation
- Lookup transformation cache files
ms.assetid: cedf5cad-2fac-42d0-ad91-9461e117d330
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 426ec69e927a61acfc083f9f4c0462553ea11d97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348294"
---
# <a name="create-and-deploy-a-cache-for-the-lookup-transformation"></a>为查找转换创建和部署缓存

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  可以为查找转换创建和部署缓存文件 (.caw)。 引用数据集存储在缓存文件中。  
  
 查找转换通过将所连接数据源输入列中的数据和引用数据集中的列进行联接来执行查找。  
  
 可以使用缓存连接管理器和“缓存转换”转换来创建缓存文件。 有关详细信息，请参阅 [缓存连接管理器](../../connection-manager/cache-connection-manager.md) 和 [缓存转换](../../../integration-services/data-flow/transformations/cache-transform.md)。  
  
 若要了解查找转换和缓存文件的更多信息，请参阅 [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md)。  
  
### <a name="to-create-a-cache-file"></a>创建缓存文件  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目，再打开该包。  
  
2.  在 **“控制流”** 选项卡上，添加数据流任务。  
  
3.  在 **“数据流”** 选项卡上，向数据流添加“缓存转换”转换，再将该转换连接到数据源。  
  
     根据需要配置数据源。  
  
4.  双击缓存转换，在“缓存转换编辑器”  中的“连接管理器”  页上单击“新建”  ，创建一个新的缓存连接管理器。  
  
5.  在 **“缓存连接管理器编辑器”** 的 **“常规”** 选项卡上，选择以下选项对缓存连接管理器进行配置，以保存缓存：  
  
    1.  选择 **“使用文件缓存”** 。  
  
    2.  在 **“文件名”** 中，键入文件路径。  
  
     运行包时，系统会创建该文件。  
  
    > [!NOTE]  
    >  包的保护级别不适用于缓存文件。 如果缓存文件包含敏感信息，可使用访问控制列表 (ACL) 来限制对存储该文件的位置或文件夹的访问。 应只允许访问某些帐户。 有关详细信息，请参阅 [访问包使用的文件](../../../integration-services/security/security-overview-integration-services.md#files)。  
  
6.  单击 **“列”** 选项卡，然后使用 **“索引位置”** 选项来指定哪些列是索引列。  
  
     对于非索引列，索引位置是 0。 对于索引列，索引位置是连续的正数。  
  
    > [!NOTE]  
    >  当将查找转换配置为使用缓存连接管理器时，则仅引用数据集中的索引列能够映射到输入列。 此外，还必须对所有索引列进行映射。  
  
     有关详细信息，请参阅 [Cache Connection Manager Editor](../../connection-manager/cache-connection-manager.md)。  
  
7.  根据需要配置缓存转换。  
  
     有关详细信息，请参阅[缓存转换编辑器（“连接管理器”页）](./cache-transform.md)和[缓存转换编辑器（“映射”页）](./cache-transform.md)。  
  
8.  运行包。  
  
### <a name="to-deploy-a-cache-file"></a>部署缓存文件  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目，再打开该包。  
  
2.  创建包配置（可选）。 有关详细信息，请参阅 [创建包配置](../../packages/legacy-package-deployment-ssis.md)。  
  
3.  执行以下操作将缓存文件添加到项目：  
  
    1.  在解决方案资源管理器中，选择在步骤 1 中打开的项目。  
  
    2.  在“项目”  菜单上，单击“添加现有项”  。  
  
    3.  选择缓存文件，再单击 **“添加”** 。  
  
     该文件将显示在解决方案资源管理器中的 **“杂项”** 文件夹。  
  
4.  配置项目以创建一个部署实用工具，再生成项目。 有关详细信息，请参阅 [Create a Deployment Utility](../../packages/legacy-package-deployment-ssis.md)。  
  
     将创建一个清单文件 \<*project name*>.SSISDeploymentManifest.xml，其中列出了项目、包及包配置中的杂项文件。  
  
5.  将包部署到文件系统。 有关详细信息，请参阅 [Deploy Packages by Using the Deployment Utility](../../packages/legacy-package-deployment-ssis.md)。  
  
## <a name="see-also"></a>另请参阅  
 [创建部署实用工具](../../packages/legacy-package-deployment-ssis.md)  
  
