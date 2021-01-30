---
description: sys.dm_os_server_diagnostics_log_configurations
title: sys.dm_os_server_diagnostics_log_configurations |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_server_diagnostics_log_configurations
- sys.dm_os_server_diagnostics_log_configurations_TSQL
- dm_os_server_diagnostics_log_configurations
- dm_os_server_diagnostics_log_configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_server_diagnostics_log_configurations
- sys.dm_os_server_diagnostics_log_configurations
ms.assetid: c09ea433-d283-4f83-af69-d458aad59217
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2df0c1c899990aa123d8e46d3a8b5c6994379d26
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190136"
---
# <a name="sysdm_os_server_diagnostics_log_configurations"></a>sys.dm_os_server_diagnostics_log_configurations
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集诊断日志的当前配置的一行信息。 这些属性设置确定是否已启用诊断日志记录，以及日志文件的位置、数目和大小。  
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|is_enabled|**bit**|指示应启用还是禁用日志记录。<br /><br /> 1 - 启用诊断日志记录<br /><br /> 0 - 禁用诊断日志记录|  
|max_size|**int**|每个诊断日志可以增长到的最大大小（mb）。 默认值为 100 MB。|  
|max_files|**int**|可以存储在计算机上的诊断日志文件的最大数量，超过该数量后这些文件将被新的诊断日志所取代。|  
|path|**nvarchar(260)**|指示诊断日志位置的路径。 默认位置是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例的安装文件夹中的 \<\MSSQL\Log>。|  
  
## <a name="permissions"></a>权限  
 需要对 SQL Server 故障转移群集实例具有 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
 下面的示例使用 sys.dm_os_server_diagnostics_log_configurations 返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移诊断日志的属性设置。  
  
```  
SELECT <list of columns>  
FROM sys.dm_os_server_diagnostics_log_configurations;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|IS_ENABLED|PATH|MAX_SIZE|MAX_FILES|  
|-----------------|----------|---------------|----------------|  
|1|\<C:\Program Files\Microsoft SQL Server\MSSQL13\MSSQL\Log>|10|10|  
  
## <a name="see-also"></a>另请参阅  
 [查看和读取故障转移群集实例诊断日志](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md)  
  
  
