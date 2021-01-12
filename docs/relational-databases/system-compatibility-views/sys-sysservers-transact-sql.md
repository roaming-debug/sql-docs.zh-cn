---
description: sys.sysservers (Transact-SQL)
title: " (Transact-sql) sys.sys服务器 |Microsoft Docs"
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysservers
- sysservers_TSQL
- sysservers
- sys.sysservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysservers system table
- sys.sysservers compatibility view
ms.assetid: d02f186f-c00f-44a6-b38d-dc78a3d2145b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1cf2a9649745eeeb5fbf10667b9849c0c1de5dec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095318"
---
# <a name="syssysservers-transact-sql"></a>sys.sysservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可作为 OLE DB 数据源访问的每个服务器都包含一行。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**srvid**|**smallint**|远程服务器的 ID（只限本地使用）。|  
|**srvstatus**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**srvname**|**sysname**|服务器的名称。|  
|**srvproduct**|**sysname**|远程服务器的产品名称。|  
|**providername**|**nvarchar(128)**|用于访问此服务器的 OLE DB 访问接口名称。|  
|**datasource**|**nvarchar(4000)**|OLE DB 数据源值。|  
|**location**|**nvarchar(4000)**|OLE DB 位置值。|  
|**providerstring**|**nvarchar(4000)**|OLE DB 访问接口字符串值。|  
|**schemadate**|**datetime**|此行上次更新的日期。|  
|**topologyx**|**int**|未使用。|  
|**topologyy**|**int**|未使用。|  
|**catalog**|**sysname**|与 OLE DB 访问接口连接时所用的目录。|  
|**srvcollation**|**sysname**|服务器的排序规则。|  
|**connecttimeout**|**int**|服务器连接的超时设置。|  
|**querytimeout**|**int**|对服务器进行查询的超时设置。|  
|**srvnetname**|**char(30)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**isremote**|**bit**|1 = 服务器为远程服务器。<br /><br /> 0 = 服务器为链接服务器。|  
|**rpc**|**bit**|1 = **sp_serveroption \@ rpc** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ rpc** 设置为 **false** 或 **off**。|  
|**pub**|**bit**|1 = **sp_serveroption \@ pub** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ pub** 设置为 **false** 或 **off**。|  
|**sub**|**bit**|1 = **sp_serveroption \@ 子项** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ sub** 设置为 **false** 或 **off**。|  
|dist|**bit**|1 = **sp_serveroption \@ dist** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ dist** 设置为 **false** 或 **off**。|  
|**dpub**|**bit**|1 = **sp_serveroption \@** 设置为 **true** 或 **on 的** dpub。<br /><br /> 0 = **sp_serveroption \@ dpub** 设置为 **false** 或 **off**。|  
|**rpcout**|**bit**|1 = **sp_serveroption \@ rpc out** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ rpc out** 设置为 **false** 或 **off**。|  
|**dataaccess**|**bit**|1 = **sp_serveroption 的 \@ 数据访问** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ 数据访问** 设置为 **false** 或 **off**。|  
|**collationcompatible**|**bit**|1 = **sp_serveroption \@ 排序规则兼容性** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ 排序规则兼容性** 设置为 **false** 或 **off**。|  
|**系统**|**bit**|1 = **sp_serveroption \@ 系统** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ 系统** 设置为 **false** 或 **off**。|  
|**useremotecollation**|**bit**|1 = **sp_serveroption \@ 远程排序规则** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ 远程排序规则** 设置为 **false** 或 **off**。|  
|**lazyschemavalidation**|**bit**|1 = **sp_serveroption \@ 延迟架构验证** 设置为 **true** 或 **on**。<br /><br /> 0 = **sp_serveroption \@ 延迟架构验证** 设置为 **false** 或 **off**。|  
|**归类**|**sysname**|按 **sp_serveroption \@ 排序规则名称** 设置的服务器排序规则。|  
|**nonsqlsub**|bit|0 = 服务器是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例<br /><br /> 1 = 服务器不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
