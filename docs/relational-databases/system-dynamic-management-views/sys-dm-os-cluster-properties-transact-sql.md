---
description: sys.dm_os_cluster_properties (Transact-SQL)
title: sys.dm_os_cluster_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cc832c3e5e89e0b78761e680ce53b0f1884e1f92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184947"
---
# <a name="sysdm_os_cluster_properties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为本主题中确定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 群集资源属性的当前设置返回一行。 如果此视图是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例上运行，则不会返回任何数据。  
  
 使用这些属性设置的值将影响故障检测、故障响应时间以及用于监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例的运行状况的日志记录。  
  

|列名|properties|说明|  
|-----------------|--------------|-----------------|  
|VerboseLogging|bigint|SQL Server 故障转移群集的日志记录级别。 可以通过启用详细日志记录在错误日志中提供更多详细信息以排除故障。 以下值之一：<br /><br /> 0 - 关闭日志记录（默认值）<br /><br /> 1 - 仅限错误<br /><br /> 2 - 错误和警告<br /><br /> 有关详细信息，请参阅 [ALTER SERVER CONFIGURATION &#40;transact-sql&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md)。|  
|SqlDumperDumpFlags|bigint|SQLDumper 转储标志确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的转储文件的类型。 默认设置为 0。|  
|SqlDumperDumpPath|nvarchar(260)|SQLDumper 实用工具生成转储文件的位置。|  
|SqlDumperDumpTimeOut|bigint|超时值（毫秒），一旦 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 失败，SQLDumper 实用工具即在该超时值后生成转储。 默认值为 0。|  
|FailureConditionLevel|bigint|设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集应在哪些状况下失败或重新启动。 默认值为 3。 有关详细说明或要更改属性设置，请参阅 [Configure FailureConditionLevel Property settings](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md)。|  
|HealthCheckTimeout|bigint|超时值，即 SQL Server 数据库引擎资源 DLL 在认定 SQL Server 实例不响应之前应等待服务器运行状况信息的时间。 该超时值用毫秒表示。 默认值为60000。 有关详细信息或要更改此属性设置，请参阅 [Configure HealthCheckTimeout Property Settings](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md)。|  
  
## <a name="permissions"></a>权限  
 需要对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例具有 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
 下面的示例使用 sys.dm_os_cluster_properties 返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集资源的属性设置。  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 下面是一个结果集示例。  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|Null|0|3|60000|  
  
  
