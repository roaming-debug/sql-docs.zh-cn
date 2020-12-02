---
description: 新建代理配置文件
title: 新建代理配置文件 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.profiles.newperfprofile.f1
helpviewer_keywords:
- New Agent Profile dialog box
ms.assetid: ebf59330-a421-45a5-9020-0484a96852bc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: b8bfc5c942c10d141ee57793fa6e7a68fcd30513
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470183"
---
# <a name="new-agent-profile"></a>新建代理配置文件
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  使用 **“新建代理配置文件”** 对话框可以创建新的配置文件。 新的配置文件始终基于现有配置文件，但可以进行修改以满足应用程序的要求。 创建配置文件之后，可以在 **“代理配置文件”** 对话框中将其应用于现有的和将来的代理作业。 可在 \<**AgentProfileName> 属性** 对话框中编辑代理参数值。  
  
## <a name="options"></a>选项  
 **名称**  
 输入配置文件的名称。  
  
 **说明**  
 输入配置文件的说明。  
  
 **Parameter**  
 配置文件中包含的代理参数。 新配置文件所基于的配置文件不必为每个参数指定值。 若要查看对给定代理有效的所有参数，请清除 **“仅显示此配置文件中使用的参数”** 复选框。 有关每个参数的说明，请参阅：  
  
-   [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
-   [复制日志读取器代理](../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
-   [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
-   [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)  
  
-   [复制队列读取器代理](../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
 **默认值**  
 每个代理参数的默认值。  
  
 **值**  
 为新配置文件所基于的配置文件中的参数指定的值。 对要更改的所有参数值，编辑此字段。  
  
 **“仅显示此配置文件中使用的参数”**  
 清除此复选框将会显示给定代理的所有有效参数。  
  
## <a name="see-also"></a>另请参阅  
 [使用复制代理配置文件](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [复制代理配置文件](../../relational-databases/replication/agents/replication-agent-profiles.md)  
  
  
