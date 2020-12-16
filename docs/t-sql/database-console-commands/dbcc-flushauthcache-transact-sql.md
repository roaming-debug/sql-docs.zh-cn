---
description: DBCC FLUSHAUTHCACHE (Transact SQL)
title: DBCC FLUSHAUTHCACHE (Transact SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC FLUSHAUTHCACHE
- FLUSHAUTHCACHE
- DBCC_FLUSHAUTHCACHE_TSQL
- FLUSHAUTHCACHE_TSQL
helpviewer_keywords:
- DBCC FLUSHAUTHCACHE
ms.assetid: 681ef31d-ceb9-4da5-86bf-bf1240df950f
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.openlocfilehash: 0927bb62d78df87d007874e21dccadab9b8ddf4d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468248"
---
# <a name="dbcc-flushauthcache-transact-sql"></a>DBCC FLUSHAUTHCACHE (Transact SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

为 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中当前用户数据库清空包含有关登录名和防火墙规则信息的数据库身份验证缓存。 此语句不适用于逻辑 master 数据库，因为 master 数据库包含登录名和防火墙规则信息的物理存储。 执行该语句的用户和当前连接的其他用户保持连接状态。 （[!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 暂不支持 DBCC FLUSHAUTHCACHE。）
 
![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC FLUSHAUTHCACHE [ ; ]  
```

## <a name="arguments"></a>参数  
无。
  
## <a name="remarks"></a>备注  
身份验证缓存创建 master 中存储的登录名和服务器防火墙规则的副本，并将它们放在用户数据库的内存中。  由于包含的数据库用户的相关信息已存储在用户数据库中，因此包含的数据库用户不是身份验证缓存的一部分。
与 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 持续保持活动连接需要至少每隔 10 小时进行重新授权（由 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 执行）。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用最初提交的密码尝试重新授权，且无需用户输入。 为了提升性能，在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中重置密码时，连接不会重新进行身份验证，即使连接因连接池而重置，也不例外。 此行为与本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的行为不同。 如果自最初授权连接时已更改密码，必须终止连接，并使用新密码建立新连接。 具有 KILL DATABASE CONNECTION 权限的用户可使用 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]KILL (Transact-SQL)[ 命令，显式终止与 ](../../t-sql/language-elements/kill-transact-sql.md) 的连接。
  
## <a name="permissions"></a>权限  
需要 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 管理员帐户。
  
## <a name="example"></a>示例  
以下语句会清除当前数据库的身份验证缓存。
  
```sql
DBCC FLUSHAUTHCACHE;  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
