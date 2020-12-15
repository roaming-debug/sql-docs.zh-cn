---
description: sp_set_database_firewall_rule（Azure SQL 数据库）
title: sp_set_database_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 08/04/2017
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sp_set_database_firewall_rule
- sp_set_database_firewall_rule_TSQL
- sys.sp_set_database_firewall_rule
- sys.sp_set_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_database_firewall_rule
- firewall_rules, setting database rules
ms.assetid: 8f0506b6-a4ac-4e4d-91db-8077c40cb17a
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: edbe51dc6694a94fcf68b012153e065906ce2208
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472638"
---
# <a name="sp_set_database_firewall_rule-azure-sql-database"></a>sp_set_database_firewall_rule（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  创建或更新的数据库级防火墙规则 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。 可以为 **master** 数据库和上的用户数据库配置数据库防火墙规则 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。 使用包含的数据库用户时，数据库防火墙规则特别有用。 有关详细信息，请参阅[包含数据库用户 - 使数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_set_database_firewall_rule [@name = ] [N]'name'  
, [@start_ip_address =] 'start_ip_address'  
, [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>自变量  
`[ @name = ] [N]'name'` 用于描述和区分数据库级防火墙设置的名称。 *name* 为 **nvarchar (128)** ，无默认值。 Unicode 标识符 `N` 对于是可选的 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] 。 
  
`[ @start_ip_address = ] 'start_ip_address'` 数据库级防火墙设置范围内的最低 IP 地址。 等于或大于此值的 IP 地址可能尝试连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 实例。 可能的最低 IP 地址为 `0.0.0.0`。 *start_ip_address* 是 **varchar (50)** ，无默认值。  
  
`[ @end_ip_address = ] 'end_ip_address'` 数据库级防火墙设置范围中的最高 IP 地址。 等于或小于此值的 IP 地址可能尝试连接到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 实例。 可能的最高 IP 地址为 `255.255.255.255`。 *end_ip_address* 是 **varchar (50)** ，无默认值。  
  
 下表说明了中支持的参数和选项 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。  
  
> [!NOTE]  
>  如果此字段和 *start_ip_address* 字段都等于，则允许 Azure 连接尝试 `0.0.0.0` 。  
  
## <a name="remarks"></a>备注  
 数据库的数据库级防火墙设置的名称必须是唯一的。 如果为存储过程提供的数据库级防火墙设置的名称在数据库级防火墙设置表中已经存在，则将更新开始和结束 IP 地址。 否则，将创建新的数据库级防火墙设置。  
  
 如果添加的数据库级防火墙设置的起始和结束 IP 地址等于 `0.0.0.0` ，则可以 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 从任何 Azure 资源中启用对服务器中数据库的访问。 为 *name* 参数提供一个值，以帮助你记住防火墙设置的用途。  
  
## <a name="permissions"></a>权限  
 需要针对数据库的 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 以下代码创建了支持从 Azure 访问数据库的名为 `Allow Azure` 的数据库级防火墙设置。  
  
```  
-- Enable Azure connections.  
EXECUTE sp_set_database_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 以下代码仅为 IP 地址 `Example DB Setting 1` 创建一个称为 `0.0.0.4` 的数据库级防火墙设置。 然后， `sp_set_database firewall_rule` 再次调用此存储过程，将该防火墙设置中的结束 IP 地址更新为 `0.0.0.6` 。 这会创建一个范围，该范围允许 IP 地址 `0.0.0.4` `0.0.0.5` 和 `0.0.0.6` 访问数据库。
  
```  
-- Create database-level firewall setting for only IP 0.0.0.4  
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.4';  
  
-- Update database-level firewall setting to create a range of allowed IP addresses
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.6';  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [Azure SQL Database 防火墙](/azure/azure-sql/database/firewall-configure)   
 [如何： (Azure SQL 数据库配置防火墙设置) ](/azure/azure-sql/database/firewall-configure)   
 [Azure SQL Database &#40;sp_set_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [Azure SQL Database &#40;sp_delete_database_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)   
 [Azure SQL Database &#40;sys.database_firewall_rules&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
