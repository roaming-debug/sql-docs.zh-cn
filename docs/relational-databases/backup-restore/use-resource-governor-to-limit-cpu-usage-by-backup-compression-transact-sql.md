---
title: 限制 CPU 负载：使用 Resource Governor 进行备份压缩
description: 可以通过将 SQL Server 用户的会话映射到限制对压缩备份的 CPU 使用的 Resource Governor 工作负载组，来对这些会话进行分类。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup compression [SQL Server], Resource Governor
- backup compression [SQL Server], CPU usage
- compression [SQL Server], backup compression
- backups [SQL Server], compression
- Resource Governor, backup compression
ms.assetid: 01796551-578d-4425-9b9e-d87210f7ba72
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6285ec4a9f06bb44d6502af44aa964b666bb58c1
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125473"
---
# <a name="use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql"></a>使用资源调控器限制备份压缩的 CPU 使用量 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  默认情况下，使用压缩进行备份会显著增加 CPU 的使用，并且压缩进程所消耗的额外 CPU 会对并发操作产生不利影响。 因此，你可能需要在会话中创建低优先级的压缩备份，当发生 CPU 争用时，此备份的 CPU 使用率受[Resource Governor](../../relational-databases/resource-governor/resource-governor.md) 限制。 本主题介绍了这样一种方案：通过将特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户的会话映射到限制 CPU 使用的资源调控器工作负荷组，来对这些会话进行分类。  
  
> [!IMPORTANT]  
>  在具体的资源调控器方案中，会话分类可能基于用户名、应用程序名称或者可以区分连接的其他任何因素。 有关详细信息，请参阅 [Resource Governor Classifier Function](../../relational-databases/resource-governor/resource-governor-classifier-function.md) 和 [Resource Governor Workload Group](../../relational-databases/resource-governor/resource-governor-workload-group.md)。  
  
##  <a name="this-topic-contains-the-following-set-of-scenarios-which-are-presented-in-sequence"></a><a name="Top"></a> 本主题包含下面一组方案，将按顺序对它们进行介绍：  
  
1.  [为低优先级操作设置登录名和用户](#setup_login_and_user)  
  
2.  [配置资源调控器以限制 CPU 使用](#configure_RG)  
  
3.  [验证当前会话的分类 (Transact-SQL)](#verifying)  
  
4.  [使用具有有限 CPU 的会话压缩备份](#creating_compressed_backup)  
  
##  <a name="setting-up-a-login-and-user-for-low-priority-operations"></a><a name="setup_login_and_user"></a> 为低优先级操作设置登录名和用户  
 本主题中的方案需要低优先级的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和用户。 将使用用户名对运行于此登录中的会话进行分类，并将它们路由到限制 CPU 使用的资源调控器工作负荷组。  
  
 下面的过程介绍了出于此目的设置登录名和用户的步骤，之后给出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 示例“示例 A：设置登录名和用户 (Transact-SQL)。”  
  
### <a name="to-set-up-a-login-and-database-user-for-classifying-sessions"></a>设置用于对会话进行分类的登录名和数据库用户  
  
1.  创建一个用于创建低优先级压缩备份的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
     **创建登录名**  
  
    -   [创建登录名](../../relational-databases/security/authentication-access/create-a-login.md)  
  
    -   [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)  
  
2.  还可以选择对此登录名授予 VIEW SERVER STATE。  
  
    -   [GRANT 系统对象权限 (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)  
  
     有关详细信息，请参阅 [GRANT 数据库主体权限 (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)。  
  
3.  为此登录名创建一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户。  
  
     **创建用户**  
  
    -   [创建数据库用户](../../relational-databases/security/authentication-access/create-a-database-user.md)  
  
    -   [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)  
  
4.  要使此登录名和用户的会话可以备份给定数据库，应将此用户添加到该数据库的 db_backupoperator 数据库角色。 对该用户将备份的每个数据库执行此步骤。 还可以选择将该用户添加到其他固定数据库角色。  
  
     **将用户添加到固定数据库角色**  
  
    -   [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
  
     有关详细信息，请参阅 [GRANT 数据库主体权限 (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)。  
  
### <a name="example-a-setting-up-a-login-and-user-transact-sql"></a>示例 A：设置登录名和用户 (Transact-SQL)  
 只有当您选择为低优先级备份创建新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和用户时，下面的示例才对您有意义。 或者，如果存在适当的登录名和用户，也可以使用现有的登录名和用户。  
  
> [!IMPORTANT]  
>  以下示例使用示例登录名和用户名 *domain_name*`\MAX_CPU`。 将此登录名和用户名替换为您计划在创建低优先级压缩备份时使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和用户名。  
  
 此示例为 *domain_name*`\MAX_CPU` Windows 帐户创建一个登录名，然后对该登录名授予 VIEW SERVER STATE 权限。 使用此权限，您可以验证登录名的会话的资源调控器分类。 本示例为 *domain_name*`\MAX_CPU` 创建一个用户，并将它添加到 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库的 db_backupoperator 固定数据库角色。 此用户名将供资源调控器分类器函数使用。  
  
```sql  
-- Create a SQL Server login for low-priority operations  
USE master;  
CREATE LOGIN [domain_name\MAX_CPU] FROM WINDOWS;  
GRANT VIEW SERVER STATE TO [domain_name\MAX_CPU];  
GO  
-- Create a SQL Server user in AdventureWorks2012 for this login  
USE AdventureWorks2012;  
CREATE USER [domain_name\MAX_CPU] FOR LOGIN [domain_name\MAX_CPU];  
EXEC sp_addrolemember 'db_backupoperator', 'domain_name\MAX_CPU';  
GO  
```  
  
 [[返回页首]](#Top)  
  
##  <a name="configuring-resource-governor-to-limit-cpu-usage"></a><a name="configure_RG"></a> 配置资源调控器以限制 CPU 使用  
  
> [!NOTE]  
>  确保资源调控器已启用。 有关详细信息，请参阅 [启用 Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)。  
  
 在本资源调控器方案中，配置过程包括以下基本步骤：  
  
1.  创建并配置一个资源调控器资源池，发生 CPU 争用时，该资源池将限制分配给资源池中的请求的最大平均 CPU 带宽。  
  
2.  创建并配置一个使用该池的资源调控器工作负荷组。  
  
3.  创建一个 *分类器函数*，它是一个用户定义函数 (UDF)，其返回值供 Resource Governor 用来对会话进行分类，以便将它们路由到适当的工作负荷组。  
  
4.  将分类器函数注册到资源调控器。  
  
5.  将更改应用于资源调控器内存中配置。  
  
> [!NOTE]  
>  有关 Resource Governor 资源池、工作负荷组以及分类的信息，请参阅 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)。  
  
 这些步骤的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在“配置用于限制 CPU 使用的资源调控器”这一步中介绍，该步骤后面给出了此步骤的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 示例。  
  
 **配置资源调控器 (SQL Server Management Studio)**  
  
-   [使用模板配置资源调控器](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)  
  
-   [创建资源池](../../relational-databases/resource-governor/create-a-resource-pool.md)  
  
-   [创建工作负荷组](../../relational-databases/resource-governor/create-a-workload-group.md)  
  
### <a name="to-configure-resource-governor-for-limiting-cpu-usage-transact-sql"></a>配置用于限制 CPU 使用的资源调控器 (Transact-SQL)  
  
1.  发出 [CREATE RESOURCE POOL](../../t-sql/statements/create-resource-pool-transact-sql.md) 语句以创建资源池。 此过程的示例使用以下语法：  
  
    ```sql  
    CREATE RESOURCE POOL <pool_name> WITH ( MAX_CPU_PERCENT = <value> );
    ```  
  
    *Value* 是 1 到 100 之间的一个整数，它指示最大平均 CPU 带宽的百分比。 实际适合的值取决于具体的环境。 为便于说明，本主题中的示例使用 20% (MAX_CPU_PERCENT = 20)。  
  
2.  发出一个 [CREATE WORKLOAD GROUP](../../t-sql/statements/create-workload-group-transact-sql.md) 语句，为你要调控其 CPU 使用的低优先级操作创建一个工作负荷组。 此过程的示例使用以下语法：  
  
    ```sql  
    CREATE WORKLOAD GROUP <group_name> USING <pool_name>;
    ```
  
3.  发出一个 [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md) 语句，以创建一个分类器函数，此函数将上一步创建的工作负荷组映射到低优先级登录名的用户。 此过程的示例使用以下语法：  
  
    ```sql 
    CREATE FUNCTION <schema_name>.<function_name>() RETURNS sysname  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
        DECLARE @workload_group_name AS <sysname>  
        IF (SUSER_NAME() = '<user_of_low_priority_login>')  
        SET @workload_group_name = '<workload_group_name>'  
        RETURN @workload_group_name  
    END;
    ```
  
    有关此 `CREATE FUNCTION` 语句各组成部分的信息，请参阅：  
  
    -   [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
  
    -   [SUSER_SNAME (Transact-SQL)](../../t-sql/functions/suser-sname-transact-sql.md)  
  
        > [!IMPORTANT]  
        >  SUSER_NAME 仅仅是可用在分类器函数中的几个系统函数中的一个。 有关详细信息，请参阅 [创建和测试分类器用户定义函数](../../relational-databases/resource-governor/create-and-test-a-classifier-user-defined-function.md)。  
  
    -   [SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)。  
      
4.  发出一个 [ALTER RESOURCE GOVERNOR](../../t-sql/statements/alter-resource-governor-transact-sql.md) 语句，将该分类器函数注册到资源调控器中。 此过程的示例使用以下语法：  
  
    ```sql  
    ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION = <schema_name>.<function_name>);
    ```  
  
5.  再发出一个 ALTER RESOURCE GOVERNOR 语句，将更改应用于资源调控器内存中配置，如下所示：  
  
    ```sql  
    ALTER RESOURCE GOVERNOR RECONFIGURE;  
    ```  
  
### <a name="example-b-configuring-resource-governor-transact-sql"></a>示例 B：配置资源调控器 (Transact-SQL)  
 下例在一个事务内执行下列步骤：  
  
1.  创建 `pMAX_CPU_PERCENT_20` 资源池。  
  
2.  创建 `gMAX_CPU_PERCENT_20` 工作负荷组。  
  
3.  创建 `rgclassifier_MAX_CPU()` 分类器函数，此函数使用在前一示例中创建的用户名。  
  
4.  将该分类器函数注册到资源调控器。  
  
 提交事务后，本示例将应用 ALTER WORKLOAD GROUP 或 ALTER RESOURCE POOL 语句中请求的配置更改。  
  
> [!IMPORTANT]  
>  以下示例使用示例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户的用户名，该用户是在“示例 A：设置登录名和用户 (Transact-SQL)” domain_name`\MAX_CPU` 中创建的  。 将此用户名替换为您计划在创建低优先级压缩备份时使用的登录名的用户名。  
  
```sql  
-- Configure Resource Governor.  
USE master;  
-- Create a resource pool that sets the MAX_CPU_PERCENT to 20%.   
CREATE RESOURCE POOL pMAX_CPU_PERCENT_20  
   WITH  
      (MAX_CPU_PERCENT = 20);  
GO  

-- Create a workload group to use this pool.   
CREATE WORKLOAD GROUP gMAX_CPU_PERCENT_20  
USING pMAX_CPU_PERCENT_20;  
GO  

-- Create a classification function.  
-- Note that any request that does not get classified goes into   
-- the 'Default' group.  
CREATE FUNCTION dbo.rgclassifier_MAX_CPU() RETURNS sysname   
WITH SCHEMABINDING  
AS  
BEGIN  
    DECLARE @workload_group_name AS sysname  
      IF (SUSER_NAME() = 'domain_name\MAX_CPU')  
          SET @workload_group_name = 'gMAX_CPU_PERCENT_20'  
    RETURN @workload_group_name  
END;  
GO  
  
-- Register the classifier function with Resource Governor.  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION= dbo.rgclassifier_MAX_CPU);  
COMMIT TRAN;  
GO  

-- Start Resource Governor  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO    
```  
  
 [[返回页首]](#Top)  
  
##  <a name="verifying-the-classification-of-the-current-session-transact-sql"></a><a name="verifying"></a> 验证当前会话的分类 (Transact-SQL)  
 还可以选择使用在分类器函数中指定的用户身份登录，通过在对象资源管理器中发出以下 [SELECT](../../t-sql/queries/select-transact-sql.md) 语句来验证会话分类：  
  
```sql  
USE master;  
SELECT sess.session_id, sess.login_name, sess.group_id, grps.name   
FROM sys.dm_exec_sessions AS sess   
JOIN sys.dm_resource_governor_workload_groups AS grps   
    ON sess.group_id = grps.group_id  
WHERE session_id > 50;  
GO  
```  
  
 在“结果”窗格中，“名称”列应列出你在分类器函数中指定的工作负荷组名称的一个或多个会话  。  
  
> [!NOTE]  
>  有关此 SELECT 语句调用的动态管理视图的信息，请参阅 [sys.dm_exec_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) 和 [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)。  
  
 [[返回页首]](#Top)  
  
##  <a name="compressing-backups-using-a-session-with-limited-cpu"></a><a name="creating_compressed_backup"></a> 使用具有有限 CPU 的会话压缩备份  
 要在限定了最大 CPU 的会话中创建压缩备份，应当以分类器函数中指定的用户身份登录。 在备份命令中，指定 WITH COMPRESSION ([!INCLUDE[tsql](../../includes/tsql-md.md)]) 或选择“压缩备份”([!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)])  。 若要创建压缩数据库备份，请参阅[创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)。  
  
### <a name="example-c-creating-a-compressed-backup-transact-sql"></a>示例 C：创建压缩备份 (Transact-SQL)  
 下面的 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 示例在一个采用新格式的备份文件 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 中创建 `Z:\SQLServerBackups\AdvWorksData.bak`数据库的压缩完整备份。  
  
```sql  
--Run backup statement in the gBackup session.  
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'   
WITH   
   FORMAT,   
   MEDIADESCRIPTION='AdventureWorks2012 Compressed Data Backups'  
   DESCRIPTION='First database backup on AdventureWorks2012 Compressed Data Backups media set'  
   COMPRESSION;  
GO  
```  
  
 [[返回页首]](#Top)  
  
## <a name="see-also"></a>另请参阅  
 [创建和测试分类器用户定义函数](../../relational-databases/resource-governor/create-and-test-a-classifier-user-defined-function.md)   
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)  
  
  
