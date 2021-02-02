---
title: 维护计划 | Microsoft Docs
description: 了解维护计划，这些计划会创建工作流来确保 SQL Server 数据库得到优化、定期备份且保持一致。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.AG.MAINTPLAN.LEGACY.F1
helpviewer_keywords:
- maintenance plans [SQL Server], about database maintenance plans
- maintenance plans [SQL Server], database compatibility level displayed in designer
- maintenance plans [SQL Server]
ms.assetid: 5982ca65-74fe-44e3-aef9-00a65a0db169
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6eb1586c38283d695e96d4640d37c261314a339f
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048911"
---
# <a name="maintenance-plans"></a>维护计划
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  维护计划可创建所需的任务工作流，以确保优化数据库、定期进行备份并确保数据库一致。 维护计划向导还可创建核心维护计划，但手动创建计划具有更大的灵活性。  
  
## <a name="benefits-of-maintenance-plans"></a>维护计划的优点  
 在 [!INCLUDE[ssDECurrent](../../includes/ssdecurrent-md.md)]中，维护计划将创建由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 代理作业运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包。 可以按预订的时间间隔手动或自动运行维护计划。  
  
 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 维护计划可提供以下功能：  
  
-   使用各种典型维护任务创建工作流的功能。 此外，还可以创建自己的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。  
  
-   概念性层次结构。 使用每个计划可创建或编辑任务工作流。 每个计划的任务可分组到子计划中，可以安排这些子计划在不同时间运行。  
  
-   支持可以用于主服务器/目标服务器环境中的多服务器计划。  
  
-   支持在远程服务器上记录计划历史记录。  
  
-   支持 Windows 身份验证和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="maintenance-plan-functionality"></a>维护计划功能  
 可以创建维护计划来执行以下任务：  
  
-   用新填充因子重新生成索引来重新组织数据和索引页上的数据。 用新填充因子重新生成索引会确保数据库页中包含的数据量和可用空间的平均分布。 还使得以后能够更快地增长。 有关详细信息，请参阅 [为索引指定填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)。  
  
-   通过删除空数据库页压缩数据文件。  
  
-   更新索引统计信息，确保查询优化器含有关于表中数据值分布的最新信息。 这使得查询优化器能够更好地确定访问数据的最佳方法，因为可以获得数据库中存储数据的详细信息。 虽然 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会定期自动更新索引统计信息，但是此选项可以对统计信息立即进行强制更新。  
  
-   对数据库内的数据和数据页执行内部一致性检查，确保系统或软件故障没有损坏数据。  
  
-   备份数据库和事务日志文件。 数据库和日志备份可以保留一段指定时间。 这样，您就可以为备份创建一份历史记录，以便在需要将数据库还原到早于上一次数据库备份的时间的时候使用。 还可以执行差异备份。  
  
-   运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。 这可以用来创建可执行各种操作的作业以及运行这些作业的维护计划。  
  
 维护任务生成的结果可以作为报表写入文本文件，或写入 **msdb** 维护计划表（**sysmaintplan_log** 和 **sysmaintplan_logdetail**）。 若要在日志文件查看器中查看结果，请右键单击“维护计划”，再单击“查看历史记录”。  
  
## <a name="related-tasks"></a>Related Tasks  
 参考以下主题以开始使用维护计划。  
  
|说明|主题|  
|-|-|  
|配置“代理 XP”服务器配置选项以启用 SQL Server 代理扩展存储过程。|[“代理 XP”服务器配置选项](../../database-engine/configure-windows/agent-xps-server-configuration-option.md)|
|说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 创建维护计划。|[创建维护计划](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)|  
|说明如何使用维护计划设计图面创建维护计划。|[创建维护计划（维护计划设计图面）](../../relational-databases/maintenance-plans/create-a-maintenance-plan-maintenance-plan-design-surface.md)|  
|对象资源管理器中可用的文档维护计划功能。|[维护计划节点（对象资源管理器）](../../relational-databases/maintenance-plans/maintenance-plans-node-object-explorer.md)|  
  
  
