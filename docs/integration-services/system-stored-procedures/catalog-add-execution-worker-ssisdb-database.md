---
description: catalog.add_execution_worker（SSISDB 数据库）
title: catalog.add_execution_worker（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: d587cedd-6402-4d5c-9526-7cd25627a037
author: chugugrace
ms.author: chugu
monikerRange: '>= sql-server-2017'
ms.openlocfilehash: 905913f8728f82a14de1ed2aadf0e2e496946dd4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460590"
---
# <a name="catalogadd_execution_worker-ssisdb-database"></a>catalog.add_execution_worker（SSISDB 数据库）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE[sqlserver2017](../../includes/applies-to-version/sqlserver2017.md)]

将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Worker 添加到 Scale Out 中的一个执行实例。

## <a name="syntax"></a>语法

```sql
catalog.add_execution_worker [ @execution_id = ] execution_id, [ @workeragent_id = ] workeragent_id
```

## <a name="arguments"></a>参数
[ @execution_id = ] execution_id  
 执行实例的唯一标识符。 execution_id 为 bigint。  
 
[@workeragent_id = ] workeragent_id  
Scale Out Worker 的辅助角色代理 ID。 workeragent_id 为 uniqueIdentifier。

## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 无  

## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对执行实例的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
 
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
 
- 用户不具备适当的权限。

- 执行标识符无效。

- 辅助角色代理 ID 无效。

- 执行不在 Scale Out 中进行。

## <a name="see-also"></a>另请参阅
[在 Scale Out 中执行包](~/integration-services/scale-out/run-packages-in-integration-services-ssis-scale-out.md)。

