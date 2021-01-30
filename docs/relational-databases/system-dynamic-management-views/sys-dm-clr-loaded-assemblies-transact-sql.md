---
description: sys.dm_clr_loaded_assemblies (Transact-SQL)
title: sys.dm_clr_loaded_assemblies (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_clr_loaded_assemblies
- sys.dm_clr_loaded_assemblies_TSQL
- dm_clr_loaded_assemblies_TSQL
- sys.dm_clr_loaded_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_loaded_assemblies dynamic management view
ms.assetid: 8523d8db-d8a0-4b1f-ae19-6705d633e0a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fea43cf3bdbbedb83b8b848c322986c4f53afebb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188793"
---
# <a name="sysdm_clr_loaded_assemblies-transact-sql"></a>sys.dm_clr_loaded_assemblies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为加载到服务器地址空间的每个托管用户程序集返回一行。 使用此视图可以了解在中执行的 CLR 集成托管数据库对象并对其进行故障排除 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 程序集是用于定义托管数据库对象并在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中部署的托管代码 DLL 文件。 只要用户执行了这些托管数据库对象中的一个，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 CLR 便会加载其中定义了托管数据库对象的程序集（及其引用）。 将加载的程序集保留在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中可提高性能，以便将来可调用程序集中包含的托管数据库对象而无需重新加载该程序集。 只有在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 面临内存不足的压力时才会卸载该程序集。 有关程序集和 CLR 集成的详细信息，请参阅 [Clr 宿主环境](../../relational-databases/clr-integration/clr-integration-architecture-clr-hosted-environment.md)。 有关托管数据库对象的详细信息，请参阅 [通过公共语言运行时生成数据库对象 &#40;CLR&#41; 集成](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)。  

  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|已加载程序集的 ID。 **Assembly_id** 可用于在 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)目录视图中查找有关程序集的详细信息。 请注意， [!INCLUDE[tsql](../../includes/tsql-md.md)] [sys.databases](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md) 目录仅显示当前数据库中的程序集。 " **Sqs.dm_clr_loaded_assemblies** " 视图显示服务器上已加载的所有程序集。|  
|**appdomain_address**|**int**|加载程序集 (**AppDomain**) 的应用程序域的地址。 单个用户拥有的所有程序集始终加载到相同的 **AppDomain** 中。 **Appdomain_address** 可用于在 [sys.dm_clr_appdomains](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md)视图中查找有关 **appdomain** 的详细信息。|  
|**load_time**|**datetime**|程序集的加载时间。 请注意，程序集将保持加载状态，直到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处于内存压力下并卸载 **AppDomain**。 你可以监视 **load_time** ，以了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在内存压力下的频率并卸载 **AppDomain**。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="remarks"></a>备注  
 **Dm_clr_loaded_assemblies. appdomain_address** 视图与 **dm_clr_appdomains** 具有多对一的关系。 Assembly_id 视图与 assembly_id 具有一对多关系。 **dm_clr_loaded_assemblies** . 。  
  
## <a name="examples"></a>示例  
 以下示例显示如何查看当前加载的数据库中所有程序集的详细信息。  
  
```  
 SELECT a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
FROM sys.dm_clr_loaded_assemblies AS l   
INNER JOIN sys.assemblies AS a  
ON l.assembly_id = a.assembly_id;  
```  
  
 下面的示例演示如何查看在其中加载给定程序集的 **AppDomain** 的详细信息。  
  
```  
SELECT appdomain_id, creation_time, db_id, user_id, state  
FROM sys.dm_clr_appdomains AS a  
WHERE appdomain_address =   
(SELECT appdomain_address   
 FROM sys.dm_clr_loaded_assemblies  
 WHERE assembly_id = 555);  
```  
  
## <a name="see-also"></a>另请参阅  
 [与公共语言运行时相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
