---
description: sys.syslockinfo (Transact-SQL)
title: sys.syslockinfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syslockinfo_TSQL
- sys.syslockinfo_TSQL
- sys.syslockinfo
- syslockinfo
dev_langs:
- TSQL
helpviewer_keywords:
- syslockinfo system table
- sys.syslockinfo compatibility view
ms.assetid: d8cae434-807a-473e-b94f-f7a0e1b2daf0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d552fdbb75c4aa1cb786025a8d1c866b89135e12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099052"
---
# <a name="syssyslockinfo-transact-sql"></a>sys.syslockinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含有关所有已授权、正在转换和正在等待的锁请求的信息。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
> [!IMPORTANT]  
>  此功能与早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有所不同。 有关详细信息，请参阅 [SQL Server 2016 中数据库引擎功能的重大更改](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**rsc_text**|**nchar(32)**|锁资源的文本化描述。 包含资源名称的一部分。|  
|**rsc_bin**|**binary(16)**|二进制锁资源。 包含锁管理器中所含的实际锁资源。 对于知道用于生成其自己的格式化锁资源的锁资源格式，以及如何在 **syslockinfo** 上执行自联接的工具，将包括此列。|  
|**rsc_valblk**|**binary(16)**|锁值块。 有些资源类型可以在特定的锁资源中包含附加数据，锁管理器不对这类锁资源进行哈希运算以决定具体某个锁资源的所有关系。 例如，页锁不归具体的对象 ID 所有。 但是，对于锁升级和出于其他目的， 页锁的对象 ID 可以包括在锁值块中。|  
|**rsc_dbid**|**smallint**|与资源关联的数据库 ID。|  
|**rsc_indid**|**smallint**|与资源关联的索引 ID（如果适合）。|  
|**rsc_objid**|**int**|与资源关联的对象 ID（如果适合）。|  
|**rsc_type**|**tinyint**|资源类型：<br /><br /> 1 = NULL 资源（未使用）<br /><br /> 2 = 数据库<br /><br /> 3 = 文件<br /><br /> 4 = 索引<br /><br /> 5 = 表<br /><br /> 6 = 页<br /><br /> 7 = 键<br /><br /> 8 = 区<br /><br /> 9 = RID（行 ID）<br /><br /> 10 = 应用程序|  
|**rsc_flag**|**tinyint**|内部资源标志。|  
|**req_mode**|**tinyint**|锁请求模式。 该列是请求者的锁模式，并且代表已授权模式，或代表转换或等待模式。<br /><br /> 0 = NULL。 不授权访问资源。 用作占位符。<br /><br /> 1 = Sch-S（架构稳定性）。 确保在任何会话持有对架构元素（例如表或索引）的架构稳定性锁时，不删除该架构元素。<br /><br /> 2 = Sch-M（架构修改）。 必须由要更改指定资源架构的任何会话持有。 确保没有其他会话正在引用所指示的对象。<br /><br /> 3 = S（共享）。 授予持有锁的会话对资源的共享访问权限。<br /><br /> 4 = U（更新）。 指示对最终可能更新的资源获取的更新锁。 用于防止常见形式的死锁，这类死锁在多个会话锁定资源并且稍后可能更新资源时发生。<br /><br /> 5 = X（排他）。 授予持有锁的会话对资源的独占访问权限。<br /><br /> 6 = IS（意向共享）。 指示有意将 S 锁放置在锁层次结构中的某个从属资源上。<br /><br /> 7 = IU（意向更新）。 指示有意将 U 锁放置在锁层次结构中的某个从属资源上。<br /><br /> 8 = IX（意向排他）。 指示有意将 X 锁放置在锁层次结构中的某个从属资源上。<br /><br /> 9 = SIU（共享意向更新）。 指示对有意在锁层次结构中的从属资源上获取更新锁的资源进行共享访问。<br /><br /> 10 = SIX（共享意向排他）。 指示对有意在锁层次结构中的从属资源上获取排他锁的资源进行共享访问。<br /><br /> 11 = UIX（更新意向排他）。 指示对有意在锁层次结构中的从属资源上获取排他锁的资源持有的更新锁。<br /><br /> 12 = BU。 用于大容量操作。<br /><br /> 13 = RangeS_S（共享键范围和共享资源锁）。 指示可串行范围扫描。<br /><br /> 14 = RangeS_U（共享键范围和更新资源锁）。 指示可串行更新扫描。<br /><br /> 15 = RangeI_N（插入键范围和空资源锁）。 用于在将新键插入索引前测试范围。<br /><br /> 16 = RangeI_S。 通过 RangeI_N 和 S 锁的重叠创建的键范围转换锁。<br /><br /> 17 = RangeI_U。 通过 RangeI_N 和 U 锁的重叠创建的键范围转换锁。<br /><br /> 18 = RangeI_X。 通过 RangeI_N 和 X 锁的重叠创建的键范围转换锁。<br /><br /> 19 = RangeX_S。 通过 RangeI_N 和 RangeS_S 锁的重叠创建的键范围转换锁 住.<br /><br /> 20 = RangeX_U。 通过 RangeI_N 和 RangeS_U 锁的重叠创建的键范围转换锁。<br /><br /> 21 = RangeX_X（排他键范围和排他资源锁）。 这是在更新范围中的键时使用的转换锁。|  
|**req_status**|**tinyint**|锁请求的状态：<br /><br /> 1 = 已授予<br /><br /> 2 = 正在转换<br /><br /> 3 = 正在等待|  
|**req_refcnt**|**smallint**|锁引用计数。 事务每次请求具体某个资源上的锁时，引用计数便会增加。 直到引用计数等于 0 时才能释放锁。|  
|**req_cryrefcnt**|**smallint**|保留以供将来使用。 总是设置为 0。|  
|**req_lifetime**|**int**|锁生存期位图。 在某些查询处理策略的过程中，必须维护资源上的锁，直到查询处理器已完成查询的某个具体阶段为止。 查询处理器和事务管理器用锁生存期位图指示在查询结束运行的某个阶段时可以释放的锁组。 位图内的某些位用于指示即使锁的引用计数等于 0，也必须到事务结束时才释放的锁。|  
|**req_spid**|**int**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 请求锁的会话的内部进程 ID。|  
|**req_ecid**|**int**|执行上下文 ID (ECID)。 用于指示并行操作内拥有具体某个锁的线程。|  
|**req_ownertype**|**smallint**|与锁关联的对象类型：<br /><br /> 1 = 事务<br /><br /> 2 = 游标<br /><br /> 3 = 会话<br /><br /> 4 = ExSession<br /><br /> 注意，3 和 4 代表会话锁的特殊版本，分别跟踪数据库锁和文件组锁。|  
|**req_transactionID**|**bigint**|**Syslockinfo** 和探查器事件中使用的唯一事务 ID|  
|**req_transactionUOW**|**uniqueidentifier**|标识 DTC 事务的工作单元 ID (UOW)。 对于非 MS DTC 事务，UOW 设置为 0。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
