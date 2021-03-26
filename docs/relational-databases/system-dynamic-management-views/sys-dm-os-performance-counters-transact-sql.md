---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL)
ms.custom: ''
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62489b131eea77ed67b1207a7606056cea39066d
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551625"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为服务器维护的每个性能计数器返回一行。 有关每个性能计数器的信息，请参阅 [使用 SQL Server 对象](../../relational-databases/performance-monitor/use-sql-server-objects.md)。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 `sys.dm_pdw_nodes_os_performance_counters` 。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar(128)**|该计数器所属的类别。|  
|**counter_name**|**nchar(128)**|计数器的名称。 若要获取有关计数器的详细信息，这是要从 [使用 SQL Server 对象](../../relational-databases/performance-monitor/use-sql-server-objects.md)的计数器列表中选择的主题的名称。 |  
|**instance_name**|**nchar(128)**|计数器特定实例的名称。 通常包含数据库名称。|  
|**cntr_value**|**bigint**|计数器的当前值。<br /><br /> **注意：** 对于每秒计数器，此值为累积值。 速率值必须通过对离散时间间隔的值抽样来进行计算。 任何两个连续抽样值之间的差等于针对所使用时间间隔的速率。|  
|**cntr_type**|**int**|Windows 性能体系结构定义的计数器类型。 有关性能计数器类型的详细信息，请参阅文档中的 [WMI 性能计数器类型](/windows/desktop/WmiSdk/wmi-performance-counter-types) 或 Windows Server 文档。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="remarks"></a>备注  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安装实例无法显示 Windows 操作系统的性能计数器，请使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询来确定性能计数器是否已被禁用。  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
如果返回值为 0 行，表示性能计数器已被禁用。 然后，你应该查看安装日志并搜索错误3409， `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` 这表示未启用性能计数器。 紧邻 3409 错误之前的错误应该指示无法启用性能计数器的根本原因。 有关安装程序日志文件的详细信息，请参阅 [查看和读取 SQL Server 安装日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  

其中 `cntr_type` 列值为65792的性能计数器仅显示最后观测到的值的快照，而不显示平均值。 

性能计数器，其中的 `cntr_type` 列值为272696320或272696576，显示在采样间隔中每秒完成的操作的平均数目。 这种类型的计数器用系统时钟的滴答 (Tick) 来测量时间 例如，若要获取只对和计数器的最后一秒的快照读取 `Buffer Manager:Lazy writes/sec` `Buffer Manager:Checkpoint pages/sec` ，则必须将两个收集点之间的增量进行比较。    

性能计数器，其中 `cntr_type` 列值为537003264，以百分比形式显示子集与其集的比率。 例如， `Buffer Manager:Buffer cache hit ratio` 计数器比较缓存命中总数和缓存查找总数。 因此，若要只获取最后一秒的快照，只需将当前值与基值之间的增量 (分母) 两者之间的差异进行比较。 对应的基值是性能计数器， `Buffer Manager:Buffer cache hit ratio base` 其中 `cntr_type` 列值为1073939712。

如果性能计数器的 `cntr_type` 列值为1073874176，则显示平均处理的项数，作为项处理次数的比率。 例如，计数器将 `Locks:Average Wait Time (ms)` 每秒的锁等待数和每秒锁请求数进行比较，以显示导致等待的每个锁请求的平均等待时间（以毫秒为单位) ） (。 因此，若要只获取最后一秒的快照，只需将当前值与基值之间的增量 (分母) 两者之间的差异进行比较。 对应的基值是性能计数器， `Locks:Average Wait Time Base` 其中 `cntr_type` 列值为1073939712。

`sys.dm_os_performance_counters`数据库引擎重新启动后，DMV 中的数据不会持久保存。 使用 `sqlserver_start_time` [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) 中的列查找上次数据库引擎启动时间。   

## <a name="permission"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
 
## <a name="examples"></a>示例  
 下面的示例返回显示快照计数器值的所有性能计数器。  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>另请参阅  
  [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
 [sys.dm_os_sys_info &#40;Transact-sql&#41;](sys-dm-os-sys-info-transact-sql.md)
