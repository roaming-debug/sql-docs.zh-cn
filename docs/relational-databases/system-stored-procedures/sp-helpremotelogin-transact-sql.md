---
description: sp_helpremotelogin (Transact-SQL)
title: sp_helpremotelogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpremotelogin_TSQL
- sp_helpremotelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpremotelogin
ms.assetid: 93f50869-2627-4642-899f-8f626f8833f4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ff291542fee3d10fe94e9ccd628e05c8f9e77f7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210817"
---
# <a name="sp_helpremotelogin-transact-sql"></a>sp_helpremotelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  报告在本地服务器上定义的某个或所有远程服务器的远程登录名的相关信息。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] 请改用链接服务器和链接服务器存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpremotelogin [ [ @remoteserver = ] 'remoteserver' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>参数  
 [ @remoteserver **=** ] **\ "**_remoteserver_*_"_*  
 远程服务器，将返回其远程登录名信息。 *remoteserver* 的值为 **sysname**，默认值为 NULL。 如果未指定 *remoteserver* ，则返回在本地服务器上定义的所有远程服务器的相关信息。  
  
 [ @remotename **=** ] **'**_remote_name_*_'_*  
 远程服务器上的特定远程登录名。 *remote_name* 的默认值为 **sysname**，默认值为 NULL。 如果未指定 *remote_name* ，则返回有关为 *remoteserver* 定义的所有远程用户的信息。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|server|**sysname**|本地服务器上定义的远程服务器的名称。|  
|local_user_name|**sysname**|本地服务器上的登录名，来自服务器的远程登录名映射到该登录名。|  
|remote_user_name|**sysname**|远程服务器上的登录，该登录映射到 local_user_name。|  
|选项|**sysname**|Trusted = 从远程服务器连接到本地服务器时，远程登录名不需要提供密码。<br /><br /> Untrusted（或空白）= 从远程服务器连接到本地服务器时，提示远程登录名提供密码。|  
  
## <a name="remarks"></a>备注  
 使用 sp_helpserver 列出在本地服务器上定义的远程服务器的名称。  
  
## <a name="permissions"></a>权限  
 未检查任何权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-reporting-help-on-a-single-server"></a>A. 报告关于单个服务器的帮助  
 以下示例显示远程服务器 `Accounts` 上的所有远程用户的相关信息。  
  
```  
EXEC sp_helpremotelogin 'Accounts';  
```  
  
### <a name="b-reporting-help-on-all-remote-users"></a>B. 报告关于所有远程用户的帮助  
 以下示例显示本地服务器已知的所有远程服务器上的所有远程用户的相关信息。  
  
```  
EXEC sp_helpremotelogin;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_addremotelogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)   
 [sp_dropremotelogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_helpserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_remoteoption &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-remoteoption-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
