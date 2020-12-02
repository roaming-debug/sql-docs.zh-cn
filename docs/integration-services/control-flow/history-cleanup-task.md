---
description: “清除历史记录”任务
title: “清除历史记录”任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.historycleanuptask.f1
helpviewer_keywords:
- history tables [SQL Server]
- History Cleanup task [Integration Services]
ms.assetid: 5defc5b9-dfd3-4859-a7fe-ac8c2b5480f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2455138c200097adc1547d3d57fbe4014b2be022
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194239"
---
# <a name="history-cleanup-task"></a>“清除历史记录”任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  “清除历史记录”任务删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb 数据库中下列历史记录表中的条目：  
  
-   backupfile  
  
-   backupfilegroup  
  
-   backupmediafamily  
  
-   backupmediaset  
  
-   backupset  
  
-   restorefile  
  
-   restorefilegroup  
  
-   restorehistory  
  
 通过使用“清除历史记录”任务，包可以删除与备份和还原活动、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业和数据库维护计划相关的历史数据。  
  
 此任务封装 sp_delete_backuphistory 系统存储过程并将指定日期作为参数传递给该过程。 有关详细信息，请参阅 [sp_delete_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md)。  
  
## <a name="configuration-of-the-history-cleanup-task"></a>“清除历史记录”任务的配置  
 此任务包含的一个属性用于指定要保留在历史记录表中的数据的最早日期。 您可以用从当天算起的天数、周数、月数或年数来指示日期，此任务可自动将该间隔转换为一个日期。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器来设置属性。 此任务位于 **设计器中** “工具箱” **的** “维护计划中的任务” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 部分。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题：  
  
-   [“清除历史记录”任务（维护计划）](../../relational-databases/maintenance-plans/history-cleanup-task-maintenance-plan.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
