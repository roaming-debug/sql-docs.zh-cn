---
title: 验证
description: 验证数据以确保它的准确性，自动或基于你在 Master Data Services 中创建的业务规则。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 98eb49e7-b190-4a21-8316-08c07cde14ed
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ecd7fcde67c20c416c7eb3eb0d781f714328a64c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336210"
---
# <a name="validation-master-data-services"></a>验证 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，对数据进行验证以确保其准确性。 一些验证自动发生，而另一些验证则基于管理员创建的业务规则。  
  
## <a name="when-data-validation-occurs"></a>何时发生数据验证  
 验证发生在不同的时间，并以不同的方式显示在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Web 应用程序中。  
  
|验证类型|标准确定者|发生条件|在 MasterData 管理器 Web UI 中显示为|在 Excel 外接程序中显示为|是否将数据保存到 MDS 存储库？|  
|---------------------|-----------------------------|--------------------|---------------------------------------------------|-------------------------------------------|------------------------------------------|  
|业务规则验证|MDS 管理员|当用户添加或编辑数据时自动发生。<br /><br /> 当用户应用业务规则时手动发生。<br /><br /> 当管理员在 **Web 应用程序的** “版本管理” [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 功能区域中验证某版本是否符合业务规则时手动发生。|验证错误|ValidationStatus|是|  
|数据类型和内容验证|MDS 管理员，当创建模型对象（例如，属性的长度或数据类型）时|当用户添加或编辑数据时自动发生|输入错误|InputStatus|否|  
|数据类型和内容验证|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|当用户添加或编辑数据时自动发生|输入错误|InputStatus|否|  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建业务规则并发布它们，以便针对它们验证数据。|[创建和发布业务规则 (Master Data Services)](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|根据业务规则验证数据版本。 仅适用于管理员。|[针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)|  
|根据业务规则验证数据的特定子集。 有权访问 **“资源管理器”** 功能区域的所有用户。|[针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|根据业务规则验证数据的特定子集。 有权访问 **“资源管理器”** 功能区域且使用 [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]的所有用户。|[应用业务规则（用于 Excel 的 MDS 外接程序）](../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
  
## <a name="see-also"></a>另请参阅  
 [业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)  
  
  
