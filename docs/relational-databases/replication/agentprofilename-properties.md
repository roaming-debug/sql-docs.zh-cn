---
description: '&lt;AgentProfileName&gt; 属性'
title: '&lt;AgentProfileName&gt; 属性 | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.profiles.perfprofileprops.f1
helpviewer_keywords:
- Agent Profile Properties dialog box
ms.assetid: 01a992d2-e4ff-417c-93f0-dc43ab2d1624
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: babf773a83101f73b0967ed3cdb94c93af5f6391
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475888"
---
# <a name="ltagentprofilenamegt-properties"></a>&lt;AgentProfileName&gt; 属性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  可以使用 **“代理配置文件属性”** 对话框查看为配置文件中的每个代理参数指定的值，以及修改用户定义的配置文件的值。  
  
## <a name="options"></a>选项  
 **名称**  
 配置文件的名称。  
  
 **说明**  
 关于配置文件的说明。  
  
 **Parameter**  
 配置文件中包含的代理参数。 配置文件无需指定每个参数值。 若要查看对给定代理有效的所有参数，请清除 **“仅显示此配置文件中使用的参数”** 复选框。 有关每个参数的说明，请参阅：  
  
-   [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
-   [复制日志读取器代理](../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
-   [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
-   [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)  
  
-   [复制队列读取器代理](../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
 **默认值**  
 每个代理参数的默认值。  
  
 **值**  
 配置文件中指定的参数值。 对于用户定义的配置文件，此字段是可编辑的。  
  
 **“仅显示此配置文件中使用的参数”**  
 清除此复选框将会显示给定代理的所有有效参数。  
  
## <a name="see-also"></a>另请参阅  
 [使用复制代理配置文件](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [复制代理配置文件](../../relational-databases/replication/agents/replication-agent-profiles.md)  
  
  
