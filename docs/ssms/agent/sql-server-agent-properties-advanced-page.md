---
description: SQL Server 代理属性（“高级”页）
title: SQL Server 代理属性（“高级”页）
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.agent.advanced.f1
ms.assetid: a4d798ee-4c18-40d4-b6af-63d17503738c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: df8a5fd3a3d5645a5ac38331e73f476c0cdd58eb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402554"
---
# <a name="sql-server-agent-properties-advanced-page"></a>SQL Server 代理属性（“高级”页）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可以查看和修改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务的高级属性。  
  
## <a name="options"></a>选项  
**SQL Server 事件转发**  
此类别中的选项可用来激活和配置事件转发功能。  
  
**将事件转发到其他服务器**  
将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理事件转发到其他服务器。  
  
**Server**  
选择要将事件转发到的服务器的名称。  
  
**未处理的事件**  
仅将未处理的事件转发到指定的服务器。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理仅转发警报未对其响应的事件。  
  
**所有事件**  
转发所有事件。 当本地实例中的某个警报响应该事件时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将转发该事件并处理此警报。  
  
**如果事件的严重性不低于**  
仅转发严重级别不低于指定级别的事件。  
  
**空闲 CPU 条件**  
此类别中的选项定义一些条件，在这些条件下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理运行那些“空闲 CPU”计划安排的作业。  
  
**定义空闲 CPU 条件**  
定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理认为 CPU 处于空闲状态的条件。  
  
**CPU 平均使用率低于**  
CPU 平均使用率，在低于该使用率时，CPU 被认为处于空闲状态。  
  
**并且保持低于此级别**  
CPU 平均占用时间量必须低于指定的级别， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理才可按照“空闲 CPU”计划运行作业。  
  
## <a name="see-also"></a>另请参阅  
[创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[管理事件](../../ssms/agent/manage-events.md)  
