---
description: sp_dbcmptlevel (Transact-SQL)
title: sp_dbcmptlevel (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_dbcmptlevel
- sp_dbcmptlevel_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbcmptlevel
ms.assetid: 508c686d-2bd4-41ba-8602-48ebca266659
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fbf4875ec03cc961e696ae5f39bd0e29abccb4e9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98168304"
---
# <a name="sp_dbcmptlevel-transact-sql"></a>sp_dbcmptlevel (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将某些数据库行为设置为与指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本兼容。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 请改用 [ALTER Database 兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dbcmptlevel [ [ @dbname = ] name ]   
    [ , [ @new_cmptlevel = ] version ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] name` 要更改其兼容级别的数据库的名称。 数据库名称必须符合标识符的规则。 *名称* 为 **sysname**，默认值为 NULL。  
  
`[ @new_cmptlevel = ] version`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]要使数据库兼容的的版本。 *版本* 为 **tinyint**，默认值为 NULL。 该值必须为下列值之一：  
  
 **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 如果未指定参数，或未指定 *name* 参数， **sp_dbcmptlevel** 将返回错误。  
  
 如果未指定 *名称* ， *则* 将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 返回一条消息，显示指定数据库的当前兼容级别。  
  
## <a name="remarks"></a>备注  
 有关兼容性级别的说明，请参阅 [ALTER DATABASE 兼容级别 &#40;transact-sql&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
## <a name="permissions"></a>权限  
 只有数据库所有者、 **sysadmin** 固定服务器角色的成员以及 **db_owner** 固定数据库角色的成员 (如果要更改当前数据库) 可以执行此过程。  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [保留关键字 (Transact-SQL)](../../t-sql/language-elements/reserved-keywords-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
