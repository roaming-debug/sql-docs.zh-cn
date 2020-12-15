---
description: sp_delete_database_firewall_rule（Azure SQL 数据库）
title: Azure SQL Database (sp_delete_database_firewall_rule) |Microsoft Docs
ms.custom: ''
ms.date: 08/04/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sp_delete_database_firewall_rule
- sp_delete_database_firewall_rule_TSQL
- sys.sp_delete_database_firewall_rule
- sys.sp_delete_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_database_firewall_rule procedure
ms.assetid: ed295312-e586-4fc2-9e80-806b490ee7bd
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: ae71838bd9a384e616d0f1fe50d612535f840919
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472688"
---
# <a name="sp_delete_database_firewall_rule-azure-sql-database"></a>sp_delete_database_firewall_rule（Azure SQL 数据库）
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  从删除数据库级防火墙设置 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。 可以为 master 数据库和上的用户数据库配置和删除数据库防火墙规则 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] 。   
  
 
## <a name="syntax"></a>语法  
  
```    
sp_delete_database_firewall_rule [@name =] [N]'name'
[ ; ]  
```  
  
## <a name="arguments"></a>自变量  
 `[@name =] [N]'name'`  
 将删除的数据库级防火墙设置的名称。 *name* 为 **nvarchar (128)** ，无默认值。 Unicode 标识符 `N` 对于是可选的 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] 。 
  
## <a name="permissions"></a>权限  
 只有由设置过程创建的服务器级别主体登录名或分配为管理员的 Azure Active Directory 主体才能删除数据库级防火墙规则。  
  
## <a name="example"></a>示例  
 下面的示例删除名为的数据库级防火墙设置 `Example DB Setting 1` 。
  
```  
-- Remove database-level firewall setting  
EXECUTE sp_delete_database_firewall_rule N'Example DB Setting 1';  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [Azure SQL Database 防火墙](/azure/azure-sql/database/firewall-configure)   
 [如何： (Azure SQL 数据库配置防火墙设置) ](/azure/azure-sql/database/firewall-configure)   
 [Azure SQL Database &#40;sp_set_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [Azure SQL Database &#40;sp_set_database_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)   
 [Azure SQL Database &#40;sys.database_firewall_rules&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
