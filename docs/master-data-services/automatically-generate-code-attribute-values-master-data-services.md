---
description: 自动生成 Code 属性值 (Master Data Services)
title: 自动生成 Code 属性值
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19b354ee-2906-4cc7-ba2f-32b4543bddcf
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 82d501f5d7c5a4a5b79e55571a7351225833680b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341489"
---
# <a name="automatically-generate-code-attribute-values-master-data-services"></a>自动生成 Code 属性值 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中，当希望在每次创建新成员时自动为 Code 值分配一个整数时，自动为实体的 Code 属性设置值。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   实体必须存在。 有关详细信息，请参阅[创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)。  
  
### <a name="to-automatically-generate-code-values"></a>自动生成 Code 值  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在“管理模型”  页上，选择包含要编辑的实体的模型行，然后单击“实体” 。  
  
3.  在“管理实体”  页上，选择要为其生成代码的实体行，然后单击“编辑” 。  
  
4.  选中 **“自动创建代码值”** 复选框。  
  
5.  在 **“起始”** 框中，键入开始递增的数字。 如果成员已存在，则将基于最大的现有值设置 Code。 例如，如果最大的现有 Code 值为 299，则下一个成员的 Code 值将设置为 300。  
  
6.  单击“ **保存**”。  
  
## <a name="see-also"></a>另请参阅  
 [自动创建代码 &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [自动生成 Code 之外的属性值 (Master Data Services)](../master-data-services/automatically-generate-attribute-values-other-than-code-master-data-services.md)  
  
  
