---
description: sp_delete_firewall_rule（Azure SQL 数据库）
title: sp_delete_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 07/27/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sp_delete_firewall_rule_TSQL
- sp_delete_firewall_rule
- sys.sp_delete_firewall_rule
- sys.sp_delete_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_firewall_rule procedure
ms.assetid: cf93eed1-ba97-4850-9fcc-b9c5a9317908
author: VanMSFT
ms.author: vanto
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 22edbde7dacf45c670c94d77e7cf48833445c346
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472698"
---
# <a name="sp_delete_firewall_rule-azure-sql-database"></a>sp_delete_firewall_rule（Azure SQL 数据库）
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  从 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 服务器中删除服务器级的防火墙设置。 此存储过程仅在服务器级别主体登录名的 master 数据库中可用。  

  
## <a name="syntax"></a>语法  
  
```  
sp_delete_firewall_rule [@name =] 'name' 
[ ; ] 
```  
  
## <a name="arguments"></a>自变量  
 存储过程的参数为：  
  
 [ @name =] "*name*"  
 将删除的服务器级防火墙设置的名称。 *name* 为 **nvarchar (128)** ，无默认值。  
  
## <a name="remarks"></a>备注  
 在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中，对连接和服务器级别的防火墙规则进行身份验证时所需的登录数据会暂时缓存在每个数据库中。 此缓存定期刷新。 若要强制刷新身份验证缓存并确保数据库具有最新版本的登录名表，请执行 [DBCC FLUSHAUTHCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 只有由设置过程创建的服务器级主体登录名才可以删除服务器级防火墙规则。 用户必须连接到 master 数据库才能执行 sp_delete_firewall_rule。  
  
## <a name="example"></a>示例  
 以下示例将删除名为“Example setting 1”的服务器级防火墙设置。 在虚拟 master 数据库中执行语句。  
  
```   
EXEC sp_delete_firewall_rule N'Example setting 1';   
```  
  
## <a name="see-also"></a>另请参阅  
 [Azure SQL Database 防火墙](/azure/azure-sql/database/firewall-configure)   
 [如何： (Azure SQL 数据库配置防火墙设置) ](/azure/azure-sql/database/firewall-configure)   
 [Azure SQL Database &#40;sp_set_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [Azure SQL Database &#40;sys.firewall_rules&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)  
  
