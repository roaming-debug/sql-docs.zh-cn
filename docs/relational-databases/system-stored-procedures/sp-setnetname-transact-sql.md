---
description: sp_setnetname (Transact-SQL)
title: sp_setnetname (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_setnetname
- sp_setnetname_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setnetname
ms.assetid: f416ba81-3835-4588-b0a3-2fe75589490e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f84120579d3eae3b2755eaab89eff137af6190db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193500"
---
# <a name="sp_setnetname-transact-sql"></a>sp_setnetname (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将 **sys. server** 中的网络名称设置为的远程实例的实际网络计算机名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 该过程可用于启用对计算机（其网络名中包含无效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符）的远程存储过程调用执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_setnetname  
@server = 'server',   
     @netname = 'network_name'  
```  
  
## <a name="arguments"></a>参数  
 **@server = "** *server* **"**  
 在用户编码的远程存储过程调用语法中引用的远程服务器名。 **Sys.** server 中必须已有一行才能使用此 *服务器*。 *server* 的数据类型为 **sysname**，无默认值。  
  
 **@netname = '** *network_name* **'**  
 对其执行远程存储过程调用的计算机网络名。 *network_name* **sysname**，无默认值。  
  
 该名称必须与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 计算机名相匹配，并且该名称可以包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符中不允许使用的字符。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 如果计算机名包含无效的标识符，对 Windows 计算机的一些远程存储过程调用则可能遇到问题。  
  
 因为链接服务器和远程服务器驻留在相同的命名空间内，所以它们的名称不能相同。 但是，你可以通过分配不同的名称并使用 **sp_setnetname** 将其中一个服务器的网络名称设置为基础服务器的网络名称，从而为指定的服务器定义链接服务器和远程服务器。  
  
```  
--Assume sqlserv2 is actual name of SQL Server   
--database server  
EXEC sp_addlinkedserver 'sqlserv2';  
GO  
EXEC sp_addserver 'rpcserv2';  
GO  
EXEC sp_setnetname 'rpcserv2', 'sqlserv2';  
```  
  
> [!NOTE]  
>  不支持使用 **sp_setnetname** 将链接服务器指向本地服务器。 以这种方式引用的服务器不能参与分布式事务。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 和 **setupadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例显示在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上发出远程存储过程调用所使用的典型管理序列。  
  
```  
USE master;  
GO  
EXEC sp_addserver 'Win_1';  
EXEC sp_setnetname 'Win_1','Win-1';  
EXEC Win_1.master.dbo.sp_who;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
