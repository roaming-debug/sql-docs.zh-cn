---
description: SQL Server 示例：模型部署包 (MDS)
title: 模型部署包示例
ms.custom: ''
ms.date: 07/28/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
keywords:
- master data services
- 示例
ms.assetid: 9b31b7b6-319b-4840-b67d-eb383e7762b1
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3dbb973b72d2df5a3a23ea40684ad7da47c19777
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833713"
---
# <a name="sql-server-examples-model-deployment-packages-mds"></a>SQL Server 示例：模型部署包 (MDS)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  安装 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]时，包含使用数据的示例模型包。 这些包文件的默认位置为 \<drive> \Program FILES\MICROSOFT SQL Server\130\Master Data services\samples\packages。  
  
 有关如何部署示例模型包的说明，请参阅 [部署示例模型和数据](../master-data-services/master-data-services-installation-and-configuration.md#deploySample)。 可以使用 [MDSModelDeploy 工具](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)部署示例模型包。  
  
> [!IMPORTANT]
>  **中的示例更新 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]**  
> 
>  示例包支持以下功能。  
> 
>  -   显示多对多关系。  
> 
>      有关详细信息，请参阅 [示例模型中的 M2M 关系](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md#M2MSample)。  
> 
> -   限制基于域的属性的允许值。  
> 
>      有关详细信息，请参阅[创建基于域的属性 (Master Data Services)](../master-data-services/create-a-domain-based-attribute-master-data-services.md)。  
> -   针对实体更改的需要批准。  
> 
>      有关详细信息，请参阅 [&#40;Master Data Services&#41;所需的批准 ](../master-data-services/approval-required-master-data-services.md)。  
> -   在业务规则中使用 Not 和 Else 运算符  
> 
>      有关详细信息，请参阅 [业务规则示例](../master-data-services/business-rule-examples-master-data-services.md)。  
> -   实现自定义索引  
> 
>      有关详细信息，请参阅[自定义索引 (Master Data Services)](../master-data-services/custom-index-master-data-services.md)。  
 

 
 在 Master Data Services 中，包是包含可部署模型结构以及来自模型的数据（可选）的 XML 文件。 使用模型包可以将模型的副本从一个 MDS 环境移到另一个环境，或者在现有的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 环境中创建新模型。  
  
## <a name="see-also"></a>另请参阅  
 [使用 MDSModelDeploy 部署模型部署包](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
  
