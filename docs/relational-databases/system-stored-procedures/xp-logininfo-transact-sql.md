---
description: xp_logininfo (Transact-SQL)
title: xp_logininfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_logininfo_TSQL
- xp_logininfo
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logininfo
ms.assetid: ee7162b5-e11f-4a0e-a09c-1878814dbbbd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6df69edd25c5c2f451e8a4aa657caf99a4c8103d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124102"
---
# <a name="xp_logininfo-transact-sql"></a>xp_logininfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 Windows 用户和 Windows 组的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_logininfo [ [ @acctname = ] 'account_name' ]   
     [ , [ @option = ] 'all' | 'members' ]   
     [ , [ @privilege = ] variable_name OUTPUT]  
```  
  
## <a name="arguments"></a>参数  
`[ @acctname = ] 'account_name'` 被授予访问权限的 Windows 用户或组的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *account_name* 的默认值为 **sysname**，默认值为 NULL。 如果未指定 *account_name* ，则会报告所有被显式授予登录权限的 windows 组和 windows 用户。 *account_name* 必须是完全限定的。 例如，“ADVWKS4\macraes”或“BUILTIN\Administrators”。  
  
 **"all"**  | **"members"**  
 指定是报告有关帐户的所有权限路径的信息，还是报告有关 Windows 组成员的信息。 **\@ 选项** 为 **varchar (10)**，默认值为 NULL。 除非指定 **all** ，否则只显示第一个权限路径。  
  
`[ @privilege = ] variable_name` 返回指定 Windows 帐户的特权级别的输出参数。 *variable_name* 是 **varchar (10)**，默认值为 "不需要"。 返回的权限级别为 " **用户**"、" **管理员**" 或 " **null**"。  
  
 OUTPUT  
 指定时，将 *variable_name* 放入 output 参数中。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**帐户名**|**sysname**|完全限定的 Windows 帐户名。|  
|type |**char (8)**|Windows 帐户类型。 有效值为 " **用户** " 或 " **组**"。|  
|**特权**|**char(9)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的访问特权。 有效值为 **admin**、 **user** 或 **null**。|  
|**mapped login name**|**sysname**|对于具有用户权限的用户帐户， **映射的登录名** 会显示映射的登录名，该名称在使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 此帐户登录时，将使用之前添加了域名的映射规则来尝试使用。|  
|**permission path**|**sysname**|使帐户得到访问权限的组成员身份。|  
  
## <a name="remarks"></a>备注  
 如果指定了 *account_name* ， **xp_logininfo** 会报告指定的 Windows 用户或组的最高权限级别。 如果 Windows 用户同时拥有系统管理员和域用户访问权限，则会报告为系统管理员。 如果该用户是多个具有相等特权级别的 Windows 组的成员，则只报告第一个被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问权限的组。  
  
 如果 *account_name* 是不与登录名关联的有效 Windows 用户或组 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则返回一个空结果集。 如果 *account_name* 不能识别为有效的 Windows 用户或组，则返回一条错误消息。  
  
 如果指定 *account_name* 和 **all** ，则返回 Windows 用户或组的所有权限路径。 如果 *account_name* 是多个组的成员，而这些组已被授予对的访问权限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则将返回多个行。 在 **用户** 特权行之前、在权限级别行内按照创建相应登录名的顺序返回了 "**管理** 特权" 行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 如果指定 *account_name* 和 **成员** ，则返回组中下一级成员的列表。 如果 *account_name* 是本地组，则该列表可以包含本地用户、域用户和组。 如果 *account_name* 是域帐户，则该列表由域用户组成。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必须连接到域控制器，才能检索组成员身份信息。 如果该服务器无法联系域控制器，则不返回任何信息。  
  
 **xp_logininfo** 仅返回 Active Directory 全局组而不是通用组的信息。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或具有授予 EXECUTE 权限的 **master** 数据库中的 **公共** 固定数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例显示有关 `BUILTIN\Administrators` Windows 组的信息。  
  
```  
EXEC xp_logininfo 'BUILTIN\Administrators';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_denylogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的常规扩展存储过程 ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  
