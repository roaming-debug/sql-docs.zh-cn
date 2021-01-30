---
description: sys.dm_server_registry (Transact-SQL)
title: sys.dm_server_registry (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_server_registry_TSQL
- sys.dm_server_registry
- dm_server_registry
- sys.dm_server_registry_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_registry dynamic management view
ms.assetid: 9b3e0c74-2e99-4996-a383-104d51831e97
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f90ca3117777081e11c1217c38d01f8fdf6a18ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203328"
---
# <a name="sysdm_server_registry-transact-sql"></a>sys.dm_server_registry (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回存储在当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 Windows 注册表中的配置和安装信息。 对于每个注册表项返回一行。 使用此动态视图可以返回诸如以下的信息：主机上可用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的网络配置值等。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|registry_key|**nvarchar(256)**|注册表项名称。 可以为 Null。|  
|value_name|**nvarchar(256)**|项值名称。 这是注册表编辑器的 " **名称** " 列中显示的项。 可以为 Null。|  
|value_data|**sql_variant**|项数据的值。 这是显示在注册表编辑器的 " **数据** " 列中的指定项的值。 可以为 Null。|  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-display-the-sql-server-services"></a>A. 显示 SQL Server 服务  
 下面的示例返回当前 SQL Server 实例的 SQL Server 和 SQL Server 代理服务的注册表项值。  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%ControlSet%';  
```  
  
### <a name="b-display-the-sql-server-agent-registry-key-values"></a>B. 显示 SQL Server 代理注册表项值  
 下面的示例返回当前 SQL Server 实例的 SQL Server 代理注册表项值。  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%SQLAgent%';  
```  
  
### <a name="c-display-the-current-version-of-the-instance-of-sql-server"></a>C. 显示 SQL Server 实例的当前版本  
 以下示例返回当前 SQL Server 实例的版本：  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE value_name = N'CurrentVersion';  
```  
  
### <a name="d-display-the-parameters-passed-to-the-instance-of-sql-server-during-startup"></a>D. 显示在启动过程中传递到 SQL Server 实例的参数  
 以下示例返回在启动过程中传递到 SQL Server 实例的参数。  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%Parameters';  
```  
  
### <a name="e-return-network-configuration-information-for-the-instance-of-sql-server"></a>E. 返回 SQL Server 实例的网络配置信息  
 下面的示例返回当前 SQL Server 实例的网络配置信息值。  
  
```  
SELECT registry_key, value_name, value_data  
FROM sys.dm_server_registry  
WHERE registry_key LIKE N'%SuperSocketNetLib%';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_server_services &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md)  
  
  
