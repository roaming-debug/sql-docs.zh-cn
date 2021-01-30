---
description: sp_migrate_user_to_contained (Transact-SQL)
title: sp_migrate_user_to_contained (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_migrate_user_to_contained
- sp_migrate_user_to_contained_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_migrate_user_to_contained
ms.assetid: b3a49ff6-46ad-4ee7-b6fe-7e54213dc33e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81450361252fcdc2d02a9d9e118839db3c5af64c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195484"
---
# <a name="sp_migrate_user_to_contained-transact-sql"></a>sp_migrate_user_to_contained (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的数据库用户转换为具有密码的包含数据库用户。 在包含的数据库中，请使用此过程删除安装了该数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的依赖项。 **sp_migrate_user_to_contained** 将用户与原始 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名分开，以便为包含的数据库单独管理密码和默认语言等设置。 在将包含的数据库移动到的其他实例之前，可以使用 sp_migrate_user_to_contained [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ，以消除当前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例登录名的依赖项。  
  
> [!NOTE]
> 使用 **sp_migrate_user_to_contained** 时要小心，因为您将无法撤消该效果。 此过程仅在包含的数据库中使用。 有关详细信息，请参阅 [Contained Databases](../../relational-databases/databases/contained-databases.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_migrate_user_to_contained [ @username = ] N'user' ,   
    [ @rename = ] { N'copy_login_name' | N'keep_name' } ,   
    [ @disablelogin = ] { N'disable_login' | N'do_not_disable_login' }   
```  
  
## <a name="arguments"></a>参数  
 [**@username =** ] **N '**_用户_*_'_*  
 当前包含的数据库中的用户名称，该用户将映射到经过身份验证的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 该值为 **sysname**，默认值为 **NULL**。  
  
 [**@rename =** ] **N '**_copy_login_name_*_'_*  | **N '**_keep_name_*_'_*  
 如果基于登录名的数据库用户与登录名的用户名不同，请使用 *keep_name* 在迁移过程中保留数据库用户名。 使用 *copy_login_name* 创建具有登录名的新包含数据库用户，而不是用户。 如果基于登录名的数据库用户具有与登录名相同的用户名，这两个选项将创建包含数据库用户而不更改名称。  
  
 [**@disablelogin =** ] **N '**_disable_login_*_'_*  | **N '**_do_not_disable_login_*_'_*  
 *disable_login* 禁用 master 数据库中的登录名。 若要在启用登录名时进行连接，连接必须提供作为连接字符串的一部分的 **初始目录** 的包含数据库名称。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_migrate_user_to_contained** 将创建包含密码的包含数据库用户，而不考虑登录名的属性或权限。 例如，如果已禁用该登录名或拒绝该用户对数据库的 **CONNECT** 权限，则该过程可能会成功。  
  
 **sp_migrate_user_to_contained** 具有以下限制。  
  
-   数据库中不能已存在此用户名。  
  
-   无法转换内置用户，例如，dbo 和 guest。  
  
-   用户不能在已签名的存储过程的 **EXECUTE AS** 子句中指定。  
  
-   用户不能拥有包含 **EXECUTE AS OWNER** 子句的存储过程。  
  
-   不能在系统数据库中使用 **sp_migrate_user_to_contained** 。  
  
## <a name="security"></a>安全性  
 在迁移用户时，切勿从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中禁用或删除所有管理员登录名。 如果删除了所有登录名，请参阅 [在系统管理员被锁定时连接到 SQL Server](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)。  
  
 如果 **BUILTIN\Administrators** 登录名存在，管理员可以通过使用 "以 **管理员身份运行** " 选项启动其应用程序进行连接。  
  
### <a name="permissions"></a>权限  
 要求具有 **CONTROL SERVER** 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-migrating-a-single-user"></a>A. 迁移单个用户  
 以下示例将名为 `Barry` 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名迁移到具有密码的包含数据库用户。 该示例不更改用户名，并将登录名保留为 "已启用"。  
  
```sql  
sp_migrate_user_to_contained   
@username = N'Barry',  
@rename = N'keep_name',  
@disablelogin = N'do_not_disable_login' ;  
  
```  
  
### <a name="b-migrating-all-database-users-with-logins-to-contained-database-users-without-logins"></a>B. 将所有具有登录名的数据库用户迁移到没有登录名的包含数据库用户  
 以下示例将所有基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的用户迁移到具有密码的包含数据库用户。 该示例不包括未启用的登录名。 必须在包含的数据库中执行该示例。  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [包含的数据库](../../relational-databases/databases/contained-databases.md)  
  
  
