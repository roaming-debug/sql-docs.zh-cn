---
description: sp_serveroption (Transact-SQL)
title: sp_serveroption (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_serveroption_TSQL
- sp_serveroption
dev_langs:
- TSQL
helpviewer_keywords:
- 7343 (Database Engine error)
- sp_serveroption
ms.assetid: 47d04a2b-dbf0-4f15-bd9b-81a2efc48131
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ec6dfda1d4e12b190999b82e16c27b5127d5af6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190782"
---
# <a name="sp_serveroption-transact-sql"></a>sp_serveroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为远程服务器和链接服务器设置服务器选项。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_serveroption [@server = ] 'server'   
      ,[@optname = ] 'option_name'       
      ,[@optvalue = ] 'option_value' ;  
```  
  
## <a name="arguments"></a>参数  
`[ @server = ] 'server'` 要为其设置选项的服务器的名称。 *server* 的数据类型为 **sysname**，无默认值。  
  
`[ @optname = ] 'option_name'` 为指定的服务器设置的选项。 *option_name* 是 **varchar (** 35 **)**，无默认值。 *option_name* 可以是以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**排序规则兼容**|影响分布式查询在链接服务器上的执行。 如果将此选项设置为 **true**，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 假定链接服务器中的所有字符都与本地服务器兼容，无论字符集和排序规则顺序如何)  (或排序顺序。 这使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 得以将字符列上的比较发送给提供程序。 如果没有设置该选项，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将始终在本地进行字符列上的比较。<br /><br /> 只有在确信链接服务器所对应的数据源与本地服务器有相同的字符集和排序顺序时，才应当设置该选项。|  
|**排序规则名称**|如果 " **使用远程排序规则** " 为 **true** ，并且数据源不是数据源，则指定远程数据源使用的排序规则的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 此名称必须是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]支持的排序规则之一。<br /><br /> 如果访问的是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]以外的 OLE DB 数据源，但该数据源的排序规则与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的某个排序规则匹配，则使用该选项。<br /><br /> 链接服务器必须支持该服务器中所有列使用的单个排序规则。 如果链接服务器支持单个数据源内的多个排序规则，或者如果无法确定链接服务器的排序规则是否与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的某个排序规则匹配，则不要设置该选项。|  
|**连接超时**|用于连接到链接服务器的超时 valuein 秒。<br /><br /> 如果为 **0**，则使用 **sp_configure** 默认值。|  
|**数据访问**|启用和禁用链接服务器以进行分布式查询访问。 仅可用于通过 **sp_addlinkedserver** 添加的 **sys.databases。**|  
|dist|分发服务器。|  
|**惰性架构验证**|确定是否检查远程表的架构。<br /><br /> 如果 **为 true**，则在查询开始时跳过远程表的架构检查。|  
|**pub**|发行者。|  
|**查询超时值**|链接服务器上的查询超时值。<br /><br /> 如果为 **0**，则使用 **sp_configure** 默认值。|  
|**rpc**|从给定的服务器启用 RPC。|  
|**rpc 输出**|对给定的服务器启用 RPC。|  
|**sub**|订阅服务器.|  
|**系统**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**使用远程排序规则**|确定是使用远程列的排序规则还是使用本地服务器的排序规则。<br /><br /> 如果 **为 true**，则远程列的排序规则用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源，而在 " **排序规则名称** " 中指定的排序规则用于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源。<br /><br /> 如果 **为 false**，则分布式查询将始终使用本地服务器的默认排序规则，而 **排序规则名称** 和远程列的排序规则将被忽略。 默认值为 false。  (**false** 值与7.0 中使用的排序规则语义兼容 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) |  
|**remote proc transaction promotion**|使用该选项可通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 事务保护服务器到服务器的操作过程。 如果该选项为 TRUE（或 ON），则调用远程存储过程启动分布式事务，并向 MS DTC 登记该事务。 调用远程存储过程的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例是事务创建者，负责控制事务的完成。 当为连接发出后续 COMMIT TRANSACTION 或 ROLLBACK TRANSACTION 语句时，主控实例请求 MS DTC 在所涉及的计算机间管理分布式事务的完成。<br /><br /> 在启动 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务后，可以对已定义为链接服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例调用远程存储过程。 链接服务器全部登记在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分布式事务中，而 MS DTC 确保对每台链接服务器完成该事务。<br /><br /> 如果此选项设置为 FALSE（或 OFF），则对链接服务器调用远程存储过程时将不会把本地事务提升为分布式事务。<br /><br /> 如果进行服务器对服务器过程调用前，事务已是分布式事务，则该选项不起作用。 对链接服务器进行的过程调用将在同一分布式事务下运行。<br /><br /> 如果进行服务器对服务器过程调用前，连接中不存在活动事务，则该选项不起作用。 然后，将对没有活动事务的链接服务器运行此过程。<br /><br /> 该选项的默认值为 TRUE（或 ON）。|  
  
`[ @optvalue = ] 'option_value'`指定是否应 (**TRUE** **或) 禁用或禁用** (**FALSE** 或 **off**) 启用 *option_name* 。 *option_value* 是 **varchar (** 10 **)**，无默认值。  
  
 对于 "**连接超时**" 和 "**查询超时**" 选项， *option_value* 可能是一个非负整数。 对于 " **排序规则名称** " 选项， *option_value* 可以是排序规则名称或 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 如果将 " **排序规则兼容** " 选项设置为 TRUE，则自动 **排序规则名称** 将设置为 NULL。 如果将 **排序规则名称** 设置为非空值，则自动 **排序规则兼容** 会设置为 FALSE。  
  
## <a name="permissions"></a>权限  
 要求对服务器拥有 ALTER ANY LINKED SERVER 权限。  
  
## <a name="examples"></a>示例  
 以下示例配置与另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（即 `SEATTLE3`）相对应的链接服务器，使其排序规则与本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例兼容。  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
## <a name="see-also"></a>另请参阅  
 [分布式查询存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
