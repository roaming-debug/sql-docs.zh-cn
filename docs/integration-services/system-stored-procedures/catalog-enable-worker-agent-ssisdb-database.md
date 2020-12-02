---
description: catalog.enable_worker_agent（SSISDB 数据库）
title: catalog.enable_worker_agent（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: c6e5266b-c32d-49ff-aa69-f09664009fb4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ddacc7a5599344f330779fad11f647b8e153ccfb
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129788"
---
# <a name="catalogenable_worker_agent-ssisdb-database"></a>catalog.enable_worker_agent（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

为使用此 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录的 Scale Out Master 启用 Scale Out Worker。

## <a name="syntax"></a>语法

```sql
catalog.enable_worker_agent [ @WorkerAgentId = ] WorkerAgentId
```
## <a name="arguments"></a>参数
[@WorkerAgentId =] WorkerAgentId Scale Out Worker 的辅助角色代理 ID。 WorkerAgentId 为 uniqueidentifier。

## <a name="example"></a>示例
此示例将在 MachineA 上启用 Scale Out Worker。

```sql
SELECT WorkerAgentId, MachineName FROM [catalog].[worker_agents]
GO
-- Result: --
-- WorkerAgentId                           MachineName --
-- 6583054A-E915-4C2A-80E4-C765E79EF61D    MachineA    --

EXEC [catalog].[enable_worker_agent] '6583054A-E915-4C2A-80E4-C765E79EF61D'
GO 
```

## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  

## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格 

## <a name="errors-and-warnings"></a>错误和警告
如果辅助角色代理 ID 无效，则存储过程返回错误。
