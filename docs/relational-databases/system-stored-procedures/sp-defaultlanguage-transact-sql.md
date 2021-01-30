---
description: sp_defaultlanguage (Transact-SQL)
title: sp_defaultlanguage (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_defaultlanguage
- sp_defaultlanguage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_defaultlanguage
ms.assetid: 908d01cc-e704-45d9-9e85-d2df6da3e6f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9c382a442e6b87220f2650e65adc03089f2014b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195560"
---
# <a name="sp_defaultlanguage-transact-sql"></a>sp_defaultlanguage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录的默认语言。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_defaultlanguage [ @loginame = ] 'login'   
     [ , [ @language = ] 'language' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @loginame = ] 'login'` 登录名。 *login* 的 **sysname** 为，无默认值。 *登录名* 可以是现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，也可以是 Windows 用户或组。  
  
`[ @language = ] 'language'` 登录名的默认语言。 *language* 的值为 **sysname**，默认值为 NULL。 *语言* 必须是服务器上的有效语言。 如果未指定 *语言* ，则 *语言* 设置为服务器默认语言;默认语言由 **sp_configure** 配置变量 **默认语言** 定义。 更改服务器默认语言不会更改现有登录的默认语言。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_defaultlanguage** 调用支持其他选项的 ALTER LOGIN。 有关更改其他登录默认值的信息，请参阅 [ALTER login &#40;transact-sql&#41;](../../t-sql/statements/alter-login-transact-sql.md)。  
  
 使用 SET LANGUAGE 语句更改当前会话的语言。 使用 @ @LANGUAGE 函数显示当前语言设置。  
  
 如果从服务器中删除登录的默认语言，则登录将获取服务器的默认语言。 不能在用户定义的事务中执行 **sp_defaultlanguage** 。  
  
 **sys.sys语言** 目录视图中显示了服务器上安装的语言的相关信息。  
  
## <a name="permissions"></a>权限  
 需要 ALTER ANY LOGIN 权限。  
  
## <a name="examples"></a>示例  
 以下示例使用 `ALTER LOGIN` 将登录 `Fathima` 的默认语言更改为阿拉伯语。 这是首选方法。  
  
```  
ALTER LOGIN Fathima WITH DEFAULT_LANGUAGE = Arabic;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [@@LANGUAGE (Transact-SQL)](../../t-sql/functions/language-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [ &#40;Transact-sql&#41;的sys.sys语言 ](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
