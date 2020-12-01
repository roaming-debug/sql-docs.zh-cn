---
description: 维护计划（管理连接）
title: 维护计划（管理连接）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.connections.f1
ms.assetid: 95ad9375-6584-423e-b9de-0e86782f8017
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ce9ac0469125a6077799e5bab0a69cdc1d06d82b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88420831"
---
# <a name="maintenance-plan-manage-connections"></a>维护计划（管理连接）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用“管理连接”对话框可以指定用于维护计划的连接的属性。 在创建某个维护计划时，将会与创建该计划所在的服务器建立本地连接。 使用此连接，可以创建在此本地连接上执行工作的任务。 如果需要的话，请使用 **“管理连接”** 对话框添加任务。 配置其他连接后，它们将出现在每个任务的连接框中。  
  
## <a name="options"></a>选项  
 **Server**  
 服务器实例。  
  
 **身份验证**  
 指示是用 Windows 身份验证还是用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证建立连接。  

> [!IMPORTANT]  
> 包存储在 msdb 数据库中，其 ProtectionLevel 设置为 ServerStorage，因此，在使用 SQL Server 身份验证时，将不会在 msdb 中加密密码。 只要 msdb 是安全的，就可以使用 SQL Server 身份验证，但建议使用 Windows 身份验证

## <a name="see-also"></a>另请参阅  
 [维护计划](../../relational-databases/maintenance-plans/maintenance-plans.md)  
  
  
