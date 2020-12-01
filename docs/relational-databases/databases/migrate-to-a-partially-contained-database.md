---
description: Migrate to a Partially Contained Database
title: 迁移到部分包含的数据库 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- contained database, migrating to
ms.assetid: 90faac38-f79e-496d-b589-e8b2fe01c562
author: stevestein
ms.author: sstein
ms.openlocfilehash: 2a942af36a551498e81b3dbaeaa02686dec9c28b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88410993"
---
# <a name="migrate-to-a-partially-contained-database"></a>Migrate to a Partially Contained Database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题讨论要更改为部分包含的数据库模型的准备工作，接着提供了迁移步骤。  
  
 **本主题内容：**  
  
-   [准备迁移数据库](#prepare)  
  
-   [启用包含的数据库](#enable)  
  
-   [将数据库转换为部分包含的数据库](#convert)  
  
-   [将用户迁移为包含的数据库用户](#users)  
  
##  <a name="preparing-to-migrate-a-database"></a><a name="prepare"></a> 准备迁移数据库  
 在考虑将数据库迁移到部分包含的数据库模型时，请复查以下各项。  
  
-   您应该了解部分包含的数据库模型。 有关详细信息，请参阅 [Contained Databases](../../relational-databases/databases/contained-databases.md)。  
  
-   您应该了解部分包含的数据库所独有的风险。 有关详细信息，请参阅 [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md)。  
  
-   包含的数据库不支持复制、变更数据结构或更改跟踪。 确认数据库没有使用这些功能。  
  
-   复查为了部分包含的数据库而修改的数据库功能的列表。 有关详细信息，请参阅[经过修改的功能（包含的数据库）](../../relational-databases/databases/modified-features-contained-database.md)。  
  
-   若要在数据库中查找非包含的对象或功能，请查询 [sys.dm_db_uncontained_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) 。 有关详细信息，请参阅：  
  
-   监视 **database_uncontained_usage** XEvent 以了解何时使用非包含的功能。  
  
##  <a name="enable-contained-databases"></a><a name="enable"></a> 启用包含的数据库  
 必须先在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例上启用包含的数据库，然后才能创建包含的数据库。  
  
### <a name="enabling-contained-databases-using-transact-sql"></a>使用 Transact-SQL 启用包含的数据库  
 下面的示例对 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例启用包含的数据库。  
  
```sql  
sp_configure 'contained database authentication', 1;  
GO  
RECONFIGURE ;  
GO  
```  
  
#### <a name="enabling-contained-databases-using-management-studio"></a>使用 Management Studio 启用包含的数据库  
 下面的示例对 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例启用包含的数据库。  
  
1.  在对象资源管理器中，右键单击服务器名称，然后单击“属性”。  
  
2.  在 **“高级”** 页面上的 **“包含”** 部分中，将 **“启用包含的数据库”** 选项设置为 **“True”**。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

##  <a name="converting-a-database-to-partially-contained"></a><a name="convert"></a> 将数据库转换为部分包含的数据库  
 通过更改 **CONTAINMENT** 选项可以将数据库转换为包含的数据库。  
  
### <a name="converting-a-database-to-partially-contained-using-transact-sql"></a>使用 Transact-SQL 将数据库转换为部分包含的数据库  
 下面的示例将名为 `Accounting` 的数据库转换为部分包含的数据库。  
  
```sql  
USE [master]  
GO  
ALTER DATABASE [Accounting] SET CONTAINMENT = PARTIAL  
GO  
```  
  
### <a name="converting-a-database-to-partially-contained-using-management-studio"></a>使用 Management Studio 将数据库转换为部分包含的数据库  
 下面的示例将数据库转换为部分包含的数据库。  
  
1.  在对象资源管理器中，展开“数据库”，右键单击要转换的数据库，然后单击“属性”。  
  
2.  在 **“选项”** 页面上，将 **“包含类型”** 选项更改为 **“部分”**。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="migrating-users-to-contained-database-users"></a><a name="users"></a> 将用户迁移为包含的数据库用户  
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
 [包含的数据库](../../relational-databases/databases/contained-databases.md)   
 [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)   
 [sys.dm_db_uncontained_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)  
  
  
