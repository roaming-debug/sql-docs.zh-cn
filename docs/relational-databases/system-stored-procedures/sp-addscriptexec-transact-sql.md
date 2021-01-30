---
description: sp_addscriptexec (Transact-SQL)
title: sp_addscriptexec (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addscriptexec
- sp_addscriptexec_TSQL
helpviewer_keywords:
- sp_addscriptexec
ms.assetid: 1627db41-6a80-45b6-b0b9-c0b7f9a1c886
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6349479d4f3ce3097771516640b0eec258e32f5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207160"
---
# <a name="sp_addscriptexec-transact-sql"></a>sp_addscriptexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将 SQL 脚本（.sql 文件）发布到发布的所有订阅服务器。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addscriptexec [ @publication = ] publication  
    [ , [ @scriptfile = ] 'scriptfile' ]  
    [ , [ @skiperror = ] 'skiperror' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @scriptfile = ] 'scriptfile'` SQL 脚本文件的完整路径。 *scriptfile* 是 **nvarchar (4000)**，无默认值。  
  
`[ @skiperror = ] 'skiperror'` 指示在脚本处理过程中遇到错误时是否应停止分发代理或合并代理。 *SkipError* 的值为 **bit**，默认值为0。  
  
 **0** = 代理将停止。  
  
 **1** = 代理继续执行脚本，并忽略错误。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  在从发布服务器进行发布时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_addscriptexec** 在事务复制和合并复制中使用。  
  
 **sp_addscriptexec** 不用于快照复制。  
  
 若要使用 **sp_addscriptexec**， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须对快照位置具有读取和写入权限，并对存储任何脚本的位置具有读取权限。  
  
 [Sqlcmd 实用工具](../../tools/sqlcmd-utility.md)用于在订阅服务器上执行脚本，该脚本在连接到订阅数据库时分发代理或合并代理使用的安全上下文中执行。 在的早期版本上运行代理时，将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [osql 实用工具](../../tools/osql-utility.md) ，而不是 [sqlcmd](../../tools/sqlcmd-utility.md)。  
  
 **sp_addscriptexec** 在将脚本应用于订阅服务器时很有用，并使用 [sqlcmd](../../tools/sqlcmd-utility.md) 将脚本的内容应用到订阅服务器。 但是，因为订阅服务器的配置可以改变，所以在投递到发布服务器前测试的脚本仍可能导致订阅服务器出错。 *skiperror* 提供分发代理或合并代理忽略错误并继续执行的功能。 在运行 **sp_addscriptexec** 之前，使用 [sqlcmd](../../tools/sqlcmd-utility.md)测试脚本。  
  
> [!NOTE]  
>  跳过的错误将继续记录在代理历史记录中以供参考。  
  
 仅订阅服务器支持使用 **sp_addscriptexec** 将用于发布的脚本文件发布到使用 FTP 进行快照传递 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addscriptexec**。  
  
## <a name="see-also"></a>另请参阅  
 [在同步期间执行脚本 &#40;复制 Transact-sql 编程&#41;](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)   
 [同步数据](../../relational-databases/replication/synchronize-data.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
