---
description: sp_helplinkedsrvlogin (Transact-SQL)
title: sp_helplinkedsrvlogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplinkedsrvlogin_TSQL
- sp_helplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplinkedsrvlogin
ms.assetid: a2b1eba0-bf71-47e7-a4c7-9f55feec82a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 274d1d9a39feac333d5b7d572ec475bb89444c2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190021"
---
# <a name="sp_helplinkedsrvlogin-transact-sql"></a>sp_helplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供有关某些登录名映射的信息，这些登录名是针对特定的链接服务器定义的，而这些链接服务器是用于分布式查询和远程存储过程的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helplinkedsrvlogin [ [ @rmtsrvname = ] 'rmtsrvname' ]   
     [ , [ @locallogin = ] 'locallogin' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @rmtsrvname = ] 'rmtsrvname'` 应用登录映射的链接服务器的名称。 *rmtsrvname* 的值为 **sysname**，默认值为 NULL。 如果为 NULL，则返回针对所有链接服务器定义的所有登录映射，这些链接服务器在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地计算机中定义。  
  
`[ @locallogin = ] 'locallogin'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]本地服务器上的登录名，它具有到链接服务器的映射 *rmtsrvname*。 *locallogin* 的值为 **sysname**，默认值为 NULL。 NULL 指定返回 *rmtsrvname* 上定义的所有登录映射。 如果不为 NULL，则必须已经存在 *locallogin* 到 *rmtsrvname* 的映射。 *locallogin* 可以是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或 Windows 用户。 对于 Windows 用户来说，必须以直接的方式或通过已被授权访问的 Windows 组成员身份授予其访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的权限。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**链接服务器**|**sysname**|链接服务器名称。|  
|**本地登录**|**sysname**|本地登录，映射应用于该本地登录。|  
|**Is Self Mapping**|**smallint**|0 = 连接到 **链接服务器** 时，**本地登录名** 映射到 **远程登录**。<br /><br /> 1 = 连接到 **链接服务器** 时，**本地登录** 名映射到相同的登录名和密码。|  
|**Remote Login**|**sysname**|**IsSelfMapping** 为0时映射到 **LocalLogin** 的 **LinkedServer** 上的登录名。 如果 **IsSelfMapping** 为1，则 **RemoteLogin** 为 NULL。|  
  
## <a name="remarks"></a>备注  
 删除登录映射之前，请使用 **sp_helplinkedsrvlogin** 来确定所涉及的链接服务器。  
  
## <a name="permissions"></a>权限  
 未检查任何权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-all-login-mappings-for-all-linked-servers"></a>A. 显示所有链接服务器的所有登录映射  
 以下示例显示所有链接服务器的所有登录映射，这些链接服务器在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地计算机上定义。  
  
```  
EXEC sp_helplinkedsrvlogin;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Accounts         NULL          1               NULL  
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
Marketing        NULL          1               NULL  
  
(4 row(s) affected)  
```  
  
### <a name="b-displaying-all-login-mappings-for-a-linked-server"></a>B. 显示某个链接服务器的所有登录映射  
 以下示例显示 `Sales` 链接服务器的所有本地定义的登录映射。  
  
```  
EXEC sp_helplinkedsrvlogin 'Sales';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
### <a name="c-displaying-all-login-mappings-for-a-local-login"></a>C. 显示某个本地登录的所有登录映射  
 以下示例显示登录 `Mary` 的所有在本地定义的登录映射。  
  
```  
EXEC sp_helplinkedsrvlogin NULL, 'Mary';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Linked Server    Local Login   Is Self Mapping Remote Login   
---------------- ------------- --------------- --------------   
Sales            NULL          1               NULL  
Sales            Mary          0               sa  
  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
