---
description: sp_addserver (Transact-SQL)
title: sp_addserver (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addserver
- sp_addserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addserver
- renaming servers
- machine names [SQL Server]
- computer names
ms.assetid: 160a6b29-5e80-44ab-80ec-77d4280f627c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fabe72f253eda808131d876fd180ddb2f7a67ad1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207098"
---
# <a name="sp_addserver-transact-sql"></a>sp_addserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]定义  本地实例的名称。 重命名宿主计算机后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，使用 **sp_addserver** 通知 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 新计算机名称的实例。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 必须在该计算机承载的所有实例上执行此过程。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 无法更改的实例名称。 若要更改命名实例的实例名称，安装具有所需名称的新实例、从旧实例中分离数据库文件、将数据库附加到新实例并删除旧实例。 或者，你可以在客户端计算机上创建客户端别名名称，无需更改服务器计算机上的实例名称即可将连接重定向到其他服务器和实例名称或 **服务器:端口** 组合。

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```

sp_addserver [ @server = ] 'server' ,
     [ @local = ] 'local' 
     [ , [ @duplicate_ok = ] 'duplicate_OK' ]
```

## <a name="arguments"></a>参数
`[ @server = ] 'server'` 服务器的名称。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 服务器名称必须唯一且必须符合  Windows 计算机名称的规则，但不允许包含空格。 *server* 的数据类型为 **sysname**，无默认值。

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果计算机上安装了多个  实例，则实例将如同在一个独立服务器上运行。 通过将 *服务器* 作为 *servername\instancename* 引用来指定命名实例。

`[ @local = ] 'LOCAL'` 指定要添加为本地服务器的服务器。 **\@ local** 是 **varchar (10)**，默认值为 NULL。 将 **\@ local** 指定 **为 local 会** 将 **\@ server** 定义为本地服务器的名称，并使 @ @SERVERNAME 函数返回 *server* 的值。

  安装程序会在安装过程中将此变量设置为计算机名称。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 默认情况下，用户可通过计算机名连接到  的实例而无需额外的配置。

 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 只有重新启动后，本地的定义才会生效。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]每个实例中只能定义一个本地服务器。

`[ @duplicate_ok = ] 'duplicate_OK'` 指定是否允许重复的服务器名称。 **\@ duplicate_OK** 是 **varchar (13)**，默认值为 NULL。 **\@ duplicate_OK** 的值只能 **duplicate_OK** 或 NULL。 如果指定 **duplicate_OK** 并且要添加的服务器名称已存在，则不会引发错误。 如果未使用命名参数，则必须指定 **\@ local** 。

## <a name="return-code-values"></a>返回代码值
 0（成功）或 1（失败）

## <a name="remarks"></a>备注
 若要设置或清除服务器选项，请使用 **sp_serveroption**。

 不能在用户定义的事务内使用 **sp_addserver** 。

 使用 **sp_addserver** 添加远程服务器已停止使用。  改用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)。

## <a name="permissions"></a>权限
  要求具有 setupadmin 固定服务器角色的成员身份。

## <a name="examples"></a>示例
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 下面的示例将承载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机名称的 `ACCOUNTS`条目更改为 。

```
sp_addserver 'ACCOUNTS', 'local';
```

## <a name="see-also"></a>另请参阅
 [重命名承载 SQL Server sp_addlinkedserver 的 Stand-Alone 实例的计算机](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) [&#40;transact-sql&#41;sp_dropserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) [&#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md) [sp_helpserver &#40;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md) Transact-sql&#41;[系统存储过程](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)&#40;transact-sql&#41;[安全存储过程 &#40;transact-sql](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)&#41;


