---
description: 编辑和删除索引 (Master Data Services)
title: 编辑和删除索引
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: f8fb2a63-f9ae-4b9d-b26f-2024d9af15c5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c6fa616af0fcc16b4898194bcdbb82e9382ba98d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341762"
---
# <a name="edit-and-delete-an-index-master-data-services"></a>编辑和删除索引 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  可以编辑和删除自己创建的有关属性的索引。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   你必须有权访问“系统管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
 **编辑索引**  
  
1.  在主数据管理器中，单击“系统管理” 。  
  
2.  在 " **管理模型** " 页上，从网格中选择一个模型，然后单击 " **实体**"。  
  
3.  在“管理实体”  页上，从网格中选择包含要编辑的索引的实体。  
  
4.  单击“索引” 。  
  
5.  在“管理索引”  页上，选择你想要编辑的索引，然后单击“编辑” 。  
  
6.  在“名称”  框中，键入索引的更新名称。  
  
7.  选中或清除 **IsUnique** 复选框。  
  
8.  通过从列表中添加和删除属性来编辑分配的属性列表。  
  
9. 单击“保存”。  
  
 **删除索引**  
  
1.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体” 。  
  
2.  在“管理实体”  页上，从网格中选择包含要删除的索引的实体。  
  
3.  单击“索引” 。  
  
4.  在“管理索引”  页上，选择你想要删除的索引，然后单击“删除” 。  
  
5.  在确认消息框中，单击“确定” 。  
  
## <a name="see-also"></a>另请参阅  
 [创建索引 &#40;Master Data Services&#41;](../master-data-services/create-an-index-master-data-services.md)   
 [自定义索引 (Master Data Services)](../master-data-services/custom-index-master-data-services.md)  
  
  
