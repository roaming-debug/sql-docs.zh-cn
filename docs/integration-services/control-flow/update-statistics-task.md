---
description: “更新统计信息”任务
title: “更新统计信息”任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.updatestatisticstask.f1
helpviewer_keywords:
- updating statistics
- Update Statistics task [Integration Services]
ms.assetid: 0247483b-f092-4511-8fa8-3610108bd1bc
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 09afac2a2eebfc32af84035cd11ee0c43a5663dc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92192744"
---
# <a name="update-statistics-task"></a>“更新统计信息”任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  “更新统计信息”任务为指定的表或索引视图中的一个或多个统计信息组（集合）更新键值分布信息。 有关详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
 通过使用“更新统计信息”任务，包可以为单个数据库或多个数据库更新统计信息。 如果此任务仅更新单个数据库中的统计信息，则可以选择任务要为其更新统计信息的视图或表。 可以配置更新来更新所有统计信息、仅更新列统计信息或仅更新索引统计信息。  
  
 此任务封装 UPDATE STATISTICS 语句，其中包括下列参数和子句：  
  
-   table_name 或 view_name 参数。  
  
-   如果更新应用于所有统计信息，则暗示使用 WITH ALL 子句。  
  
-   如果更新仅应用于列，则包含 WITH COLUMN 子句。  
  
-   如果更新仅应用于索引，则包含 WITH INDEX 子句。  
  
 如果“更新统计信息”任务更新多个数据库中的统计信息，则它将运行多个 UPDATE STATISTICS 语句，每个语句用于一个表或视图。 UPDATE STATISTICS 的所有实例均使用相同的子句，但使用不同的 table_name 或 view_name 值。 有关详细信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md) 和 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)。  
  
> [!IMPORTANT]  
>  此任务创建它所运行的 Transact-SQL 语句花费的时间与它更新的统计信息数成正比。 如果配置此任务来更新具有大量索引的数据库中所有表和视图中的统计信息，或更新多个数据库中的统计信息，则任务将花费相当长的时间来生成 Transact-SQL 语句。  
  
## <a name="configuration-of-the-update-statistics-task"></a>“更新统计信息”任务的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器来设置属性。 此任务位于 **设计器中** “工具箱” **的** “维护计划中的任务” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 部分。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的信息，请单击以下主题：  
  
-   [“更新统计信息”任务（维护计划）](../../relational-databases/maintenance-plans/update-statistics-task-maintenance-plan.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
