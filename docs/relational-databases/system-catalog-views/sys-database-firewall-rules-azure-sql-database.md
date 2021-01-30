---
description: sys.database_firewall_rules (Azure SQL Database)
title: Azure SQL Database (sys.database_firewall_rules) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.database_firewall_rules_TSQL
- database_firewall_rules_TSQL
- sys.database_firewall_rules
- database_firewall_rules
dev_langs:
- TSQL
helpviewer_keywords:
- database_firewall_rules
- sys.database_firewall_rules
ms.assetid: 2e821593-3b9f-43d6-a99b-1ceffe177faf
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: 6e09c2043494b2d01adb3781b7b3c4336a3b9a1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195881"
---
# <a name="sysdatabase_firewall_rules-azure-sql-database"></a>sys.database_firewall_rules (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  返回与关联的数据库级防火墙设置的相关信息 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。 在使用包含的数据库用户时，数据库级防火墙设置特别有用。 有关详细信息，请参阅 [包含的数据库用户 - 使你的数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
 `sys.database_firewall_rules` 视图包含以下各列：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|id|**INTEGER**|数据库级防火墙设置的标识符。|  
|name|**NVARCHAR (128)**|您选择用来描述和区分数据库级防火墙设置的名称。|  
|start_ip_address|**VARCHAR (45)**|数据库级防火墙设置范围内的最低 IP 地址。 等于或大于此值的 IP 地址可能尝试连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 实例。 可能的最低 IP 地址为 `0.0.0.0`。|  
|end_ip_address|**VARCHAR (45)**|防火墙设置范围内的最高 IP 地址。 等于或小于此值的 IP 地址可能尝试连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 实例。 可能的最高 IP 地址为 `255.255.255.255`。<br /><br /> 注意：如果此字段和 **start_ip_address** 字段都等于，则允许 Azure 连接尝试 `0.0.0.0` 。|  
|create_date|**型**|创建数据库级防火墙设置时的 UTC 日期和时间。|  
|modify_date|**型**|上次修改数据库级防火墙设置时的 UTC 日期和时间。|  
  
## <a name="remarks"></a>备注  
 若要返回有关与 Microsoft Azure SQL 数据库关联的服务器级防火墙设置的信息，请使用 [sys.firewall_rules (AZURE SQL 数据库) ](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)。  
  
## <a name="permissions"></a>权限  
 此视图在 **master** 数据库和每个用户数据库中可用。 为具有连接到数据库的权限的所有用户提供对此视图的只读访问权限。  
  
## <a name="see-also"></a>另请参阅
[sp_set_database_firewall_rule（Azure SQL 数据库）](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
[Azure SQL Database &#40;sp_delete_database_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)  
[sp_set_firewall_rule（Azure SQL 数据库）](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
[Azure SQL Database &#40;sp_delete_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database.md)   
[Azure SQL Database &#40;sys.firewall_rules&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)  
[为数据库引擎访问配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)     
[将防火墙配置为进行 FILESTREAM 访问](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)  
[将防火墙配置为允许报表服务器访问](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
