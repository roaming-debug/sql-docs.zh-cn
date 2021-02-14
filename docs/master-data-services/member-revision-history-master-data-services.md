---
description: 成员修订历史记录 (Master Data Services)
title: 成员修订历史记录
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 113069c5-12e6-48ec-b443-b42e14f77308
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 148d2d815680ef0e8c03cf2dc4d68f43864872f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341410"
---
# <a name="member-revision-history-master-data-services"></a>成员修订历史记录 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  如果实体事务日志类型为成员，则每当成员发生变化时，系统都会记录成员修订历史记录。  
  
 有关事务日志类型的信息，请参阅[更改实体事务日志类型 (Master Data Services)](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)。  
  
 当发生下列变化时，系统会记录成员修订历史记录。  
  
-   创建、删除、重新激活或清除成员。  
  
-   更改属性值。  
  
-   在层次结构或集合中移动成员  
  
## <a name="view-and-manage-revision-history-by-entity"></a>由实体查看和管理修订历史记录  
 在资源管理器功能区中，可以查看实体中所有成员的修订版本。 如果具有更新权限，可以将成员回退到之前的修订版本。  
  
 **查看和管理修订历史记录**  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，选择模型和版本，然后单击“资源管理器” 。  
  
2.  从“实体”  菜单中选择实体。  
  
3.  单击“查看历史记录”  来查看实体的所有历史数据。  
  
4.  单击“筛选器”  来筛选数据。  
  
5.  单击列标题对数据进行排序。  
  
6.  如果具有更新权限，请单击“还原成员”  回退到所选版本。  
  
## <a name="view-and-manage-revision-history-by-member"></a>由成员查看和管理修订历史记录  
 如果具有成员的读取权限，就可以在资源管理器功能区中查看成员的修订版本。 如果具有更新权限，就可以将成员回退到之前的修订版本或为修订版本添加注释。  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，选择模型和版本，然后单击“资源管理器” 。  
  
2.  从“实体”  菜单中选择实体。  
  
3.  选择成员。  
  
4.  在右窗格中，单击“查看历史记录”  。  
  
## <a name="log-retention-setting"></a>日志保留设置  
 可以通过设置 **数据库的系统设置中的“日志保留天数”**[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 属性配置历史记录数据保留期，也可以通过在创建或编辑模型时设置“日志保留天数”  来配置。  
  
## <a name="related-task"></a>相关任务  
  
|任务说明|主题|  
|----------------------|-----------|  
|回退成员修订历史记录|[回退成员修订历史记录 (Master Data Services)](../master-data-services/rollback-member-revision-history-master-data-services.md)|  
  
## <a name="see-also"></a>另请参阅  
 [创建模型 &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)   
 [系统设置 &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md)  
  
  
