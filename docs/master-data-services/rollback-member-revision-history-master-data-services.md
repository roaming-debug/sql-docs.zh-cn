---
description: 回退成员修订历史记录 (Master Data Services)
title: 回退成员修订历史记录
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d39d3474-20e7-429f-9c8d-fcc4eb0854fc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec99fc4d464037b4051da46e293efb4b2fb6969a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355282"
---
# <a name="rollback-member-revision-history-master-data-services"></a>回退成员修订历史记录 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  每当成员发生变化时，系统都会记录成员修订历史记录。 你可以将成员修订历史记录回退到旧版记录。  
  
## <a name="prerequisites"></a>先决条件  
  
-   你必须有权更新选定成员的至少一个属性。 当你回退修订历史记录时，可以更新的所有属性值都会回退到旧版值。  
  
-   仅当实体的事务日志类型是成员时，才会生成修订历史记录。  
  
 **回退成员修订历史记录**  
  
1.  在“主数据管理器”中，单击“资源管理器”。  
  
2.  选择要回退的实体和成员。  
  
3.  单击“查看历史记录”   
  
4.  选择要回退的修订，然后单击“回退” 。  
  
## <a name="see-also"></a>另请参阅  
 [成员修订历史记录 &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md)   
 [更改实体事务日志类型 &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
  
