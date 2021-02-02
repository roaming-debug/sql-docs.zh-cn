---
description: sys.servers (Transact-SQL)
title: sys. server (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- servers_TSQL
- sys.servers_TSQL
- servers
- sys.servers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.servers catalog view
ms.assetid: 4e774ed9-4e83-4726-9f1d-8efde8f9feff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a37a592866c8fcfdc12e5b84657bc7fa7166ee81
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237887"
---
# <a name="sysservers-transact-sql"></a>sys.servers (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  为每个已注册的链接服务器或远程服务器包含一行，并为具有 **server_id** = 0 的本地服务器提供一行。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|链接服务器的本地 ID。|  
|name|**sysname**|当 **server_id** = 0 时，返回的值是服务器名称。<br /><br /> 如果 **server_id** > 0，则返回的值为链接服务器的本地名称。|  
|**product**|**sysname**|链接服务器的产品名。 值 "SQL Server" 指示的另一个实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**程序**|**sysname**|用于连接到链接服务器的 OLE DB 访问接口名称。<br /><br />从开始 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ，值 "sqlncli.msi" 将映射到默认情况下 [SQL SERVER (MSOLEDBSQL) 的 Microsoft OLE DB 驱动程序 ](../../connect/oledb/oledb-driver-for-sql-server.md) 。 在早期版本中，值 "SQLNCLI.MSI" 映射到 [SQL Server Native Client OLE DB 提供程序 (SQLNCLI11) ](../../relational-databases/native-client/sql-server-native-client.md)。|  
|**data_source**|**nvarchar(4000)**|OLE DB 数据源连接属性。|  
|**location**|**nvarchar(4000)**|OLE DB 位置连接属性。 如果没有，则为 NULL。|  
|**provider_string**|**nvarchar(4000)**|OLE DB 访问接口字符串连接属性。<br /><br /> 如果调用方具有权限，则为 NULL `ALTER ANY LINKED SERVER` 。|  
|**catalog**|**sysname**|OLE DB 目录连接属性。 如果没有，则为 NULL。|  
|**connect_timeout**|**int**|以秒为单位的连接超时，0 表示没有超时。|  
|**query_timeout**|**int**|以秒为单位的查询超时，0 表示没有超时。|  
|**is_linked**|**bit**|0 = 是使用 **sp_addserver** 添加的一种老式服务器，具有不同的 RPC 和分布式事务行为。<br /><br /> 1 = 标准链接服务器。|  
|**is_remote_login_enabled**|**bit**|设置 RPC 选项来启用该服务器的传入远程登录。|  
|**is_rpc_out_enabled**|**bit**|启用（从该服务器的）传出 RPC。|  
|**is_data_access_enabled**|**bit**|为分布式查询启用服务器。|  
|**is_collation_compatible**|**bit**|如果没有可用的排序规则信息，则假定远程数据的排序规则与本地数据兼容。|  
|**uses_remote_collation**|**bit**|如果为 1，则使用远程服务器报告的排序规则；否则，使用下一列指定的排序规则。|  
|**collation_name**|**sysname**|要使用的排序规则的名称，或者，如果只使用本地排序规则，则为 NULL。|  
|**lazy_schema_validation**|**bit**|如果为 1，则在启动查询时不检查架构验证。|  
|**is_system**|**bit**|该服务器只能由内部系统进行访问。|  
|**is_publisher**|**bit**|服务器为复制发布服务器。|  
|**is_subscriber**|**bit**|服务器为复制订阅服务器。|  
|**is_distributor**|**bit**|服务器为复制分发服务器。|  
|**is_nonsql_subscriber**|**bit**|服务器为非 SQL Server 复制订阅服务器。|  
|**is_remote_proc_transaction_promotion_enabled**|**bit**|如果是 1，则调用远程存储过程将启动分布式事务，并用 MS DTC 登记该事务。 有关详细信息，请参阅 [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)的数据。|  
|modify_date|**datetime**|上次更改服务器信息的日期。|  
|**is_rda_server**|**bit**|**适用于：** 从开始 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 。<br /><br />服务器为远程数据存档 (启用 stretch) 启用。 有关详细信息，请参阅在 [服务器上启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer)。|
  
## <a name="permissions"></a>权限  
 **Provider_string** 中的值始终为 NULL，除非调用方具有 ALTER ANY 链接服务器权限。  
  
  (**server_id** = 0) 查看本地服务器不需要权限。  
  
 创建链接服务器或远程服务器时，会 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建一个到 **公共** 服务器角色的默认登录映射。 默认登录映射意味着所有登录名都可以查看所有链接服务器和远程服务器。 若要限制这些服务器的可见性，请通过执行 [sp_droplinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md) 并为 *LOCALLOGIN* 参数指定 NULL 来删除默认的登录映射。  
  
 如果删除了默认登录映射，则只有已作为链接登录名或远程登录名显式添加的用户才能查看其拥有登录名的链接服务器或远程服务器。  在默认的登录映射之后，查看所有链接服务器和远程服务器需要以下权限：  
  
- `ALTER ANY LINKED SERVER` 或 `ALTER ANY LOGIN ON SERVER`  
- **Setupadmin** 或 **sysadmin** 固定服务器角色的成员身份  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [链接服务器目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addremotelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)  
  
