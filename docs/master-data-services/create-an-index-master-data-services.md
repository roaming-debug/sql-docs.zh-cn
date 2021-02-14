---
description: 创建索引 (Master Data Services)
title: 创建索引
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d694a105-69b1-4ff6-99d3-1f408b916b81
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: cad316eb2c1bd4925e72839ede2e29bccb825118
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351891"
---
# <a name="create-an-index-master-data-services"></a>创建索引 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  为经常查询的属性列表创建自定义索引，以提高查询性能。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   你必须有权访问“系统管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
 **创建索引**  
  
1.  在主数据管理器中，单击“系统管理” 。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体” 。  
  
3.  在“管理实体”  页上，从“网格”  中选择要为其创建索引的实体所在的行。  
  
4.  单击“索引” 。  
  
5.  在“名称”  框中，键入索引的名称。  
  
6.  如果你想创建唯一索引，请选择“是唯一的”  。 有关索引类型的详细信息，请参阅 [自定义索引 (Master Data Services)](../master-data-services/custom-index-master-data-services.md)。  
  
7.  依次单击“可用属性”  框中的属性和“添加”  箭头。 若要添加全部属性，请单击“全部添加”  箭头。  
  
8.  单击“保存”。  
  
 对于你创建的每个索引，系统都会在网格中添加一行（其中包含四列）。 下表对这些列进行了说明。  
  
|列名|描述|  
|-----------------|-----------------|  
|状态|索引状态。<br /><br /> 单击 " **保存**" 时，将显示 ![更新状态](../master-data-services/media/mds-statusicon-updating.png "用于更新状态的图标") 图像的图标，表明索引正在更新。<br /><br /> 如果在创建或编辑索引时出现错误，则会显示 ![错误状态图标图标](../master-data-services/media/mds-statusicon-error.png "错误状态图标") 。<br /><br /> 否则，状态为 "正常"，并显示 !["确定状态](../master-data-services/media/mds-statusicon-ok.png ""正常" 状态图标") " 图像的图标。|  
|name|索引名称。|  
|是唯一的|指定索引是否是唯一的。|  
|对应属性|显示在其上定义索引的属性的显示名称。|  
  
 单击索引后可看到以下信息：  
  
-   创建者：创建索引的用户的用户名。  
  
-   创建时间：创建索引的日期和时间。  
  
-   更新者：上次更新索引的用户的用户名。  
  
-   更新时间：上次更新索引的日期和时间。  
  
## <a name="next-steps"></a>后续步骤  
 [编辑和删除索引 (Master Data Services)](../master-data-services/edit-and-delete-an-index-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [自定义索引 (Master Data Services)](../master-data-services/custom-index-master-data-services.md)  
  
  
