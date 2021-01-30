---
description: sp_detach_db (Transact-SQL)
title: sp_detach_db (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/30/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_detach_db
- sp_detach_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_db
- detaching databases [SQL Server]
ms.assetid: abcb1407-ff78-4c76-b02e-509c86574462
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1d4b484cd6d440b14b7639b513fba7e53f615443
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200757"
---
# <a name="sp_detach_db-transact-sql"></a>sp_detach_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从服务器实例中分离当前未使用的数据库，并且可以选择在分离之前为所有表运行 UPDATE STATISTICS。  
  
> [!IMPORTANT]  
>  要分离复制的数据库，该数据库必须是未发布的数据库。 有关详细信息，请参阅本主题后面的“备注”部分。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_detach_db [ @dbname= ] 'database_name'   
    [ , [ @skipchecks= ] 'skipchecks' ]   
    [ , [ @keepfulltextindexfile = ] 'KeepFulltextIndexFile' ]   
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'database_name'` 要分离的数据库的名称。 *database_name* 是 **sysname** 值，默认值为 NULL。  
  
`[ @skipchecks = ] 'skipchecks'` 指定是否要跳过或运行更新统计信息。 *skipchecks* 是一个 **nvarchar (10)** 值，默认值为 NULL。 若要跳过更新统计信息，请指定 **true**。 若要显式运行 UPDATE STATISTICS，请指定 **false**。  
  
 默认情况下，执行 UPDATE STATISTICS 可更新有关表和索引中的数据的信息。 对于要移动到只读介质的数据库，执行 UPDATE STATISTICS 非常有用。  
  
`[ @keepfulltextindexfile = ] 'KeepFulltextIndexFile'` 指定在数据库分离操作过程中不会删除与所分离的数据库关联的全文索引文件。 *KeepFulltextIndexFile* 是一个 **nvarchar (10)** 值，默认值为 **true**。 如果 *KeepFulltextIndexFile* 为 **false**，则将删除与数据库关联的所有全文索引文件和全文索引的元数据，除非该数据库是只读的。 如果为 NULL 或 **true**，则保留与全文相关的元数据。  
  
> [!IMPORTANT]
>  未来版本的中将删除 **\@ keepfulltextindexfile** 参数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 请不要在新的开发工作中使用此参数，并尽快修改当前仍在使用此参数的应用程序。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 分离数据库时，会删除其所有元数据。 如果数据库是任何登录帐户的默认数据库，则 **master** 会成为其默认数据库。  
  
> [!NOTE]  
>  有关如何查看所有登录帐户的默认数据库的信息，请参阅 [&#40;transact-sql&#41;sp_helplogins ](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)。 如果你具有所需的权限，则可以使用 [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) 为登录名分配新的默认数据库。  
  
## <a name="restrictions"></a>限制  
 如果符合下列任一条件，则无法分离数据库：  
  
-   该数据库当前正在使用。 有关详细信息，请参阅本主题后面的“获取独占访问权限”。  
  
-   如果已进行复制，则数据库已发布。  
  
     在分离数据库之前，必须通过运行 [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)禁用发布。  
  
    > [!NOTE]  
    >  如果无法使用 **sp_replicationdboption**，可以通过运行 [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)删除复制。  
  
-   数据库中存在数据库快照。  
  
     必须首先删除所有数据库快照，然后才能分离数据库。 有关详细信息，请参阅 [删除数据库快照 (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)实例。  
  
    > [!NOTE]  
    >  不能分离或附加数据库快照。  
  
-   该数据库正在进行镜像。  
  
     在终止数据库镜像会话之间，无法分离该数据库。 有关详细信息，请参阅 [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)。  
  
-   数据库处于可疑状态。  
  
     您必须先将可疑数据库设为紧急模式，然后才能对其进行分离。 有关如何将数据库置于紧急模式下的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
-   数据库为系统数据库。  
  
## <a name="obtaining-exclusive-access"></a>获取独占访问权限  
 分离数据库需要对数据库有独占访问权限。 如果要分离的数据库正在使用当中，则必须先将该数据库设置为 SINGLE_USER 模式以获取独占访问权限，然后才能对其进行分离。

 在将数据库设置为 SINGLE_USER 之前，应验证 AUTO_UPDATE_STATISTICS_ASYNC 选项是否设置为 OFF。 在此选项设置为 ON 时，用于更新统计信息的后台线程将与数据库建立连接，您将无法以单用户模式访问数据库。 有关详细信息，请参阅 [将数据库设置为单用户模式](../databases/set-a-database-to-single-user-mode.md)。

 例如，下面的 `ALTER DATABASE` 语句在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 所有当前用户从数据库断开连接后，获得对数据库的独占访问权限。  
  
```  
USE master;  
ALTER DATABASE AdventureWorks2012  
SET SINGLE_USER;  
GO  
```  
  
> [!NOTE]  
>  若要立即或在指定的秒数内强制当前用户离开数据库，还请使用 ROLLBACK 选项： ALTER DATABASE *database_name* SET SINGLE_USER WITH ROLLBACK *rollback_option*。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
## <a name="reattaching-a-database"></a>重新附加数据库  
 可以使用 CREATE DATABASE（带有 FOR ATTACH 或 FOR ATTACH_REBUILD_LOG 选项）保留并重新附加分离文件。 这些文件可以移动并附加到其他服务器上。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或数据库 **db_owner** 角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例将 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] *skipchecks* 设置为 true 的数据库进行分离。  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
 以下示例将分离 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库，并保留全文索引文件和全文索引的元数据。 此命令将运行 UPDATE STATISTICS，这是默认行为。  
  
```  
exec sp_detach_db @dbname='AdventureWorks2012'  
    , @keepfulltextindexfile='true';  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [分离数据库](../../relational-databases/databases/detach-a-database.md)  
  
