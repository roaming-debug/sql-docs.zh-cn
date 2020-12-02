---
title: 还原数据库：简单恢复模式 (Transact-SQL)
description: 本文演示如何使用 Transact-SQL 在简单恢复模式下还原完整 SQL Server 数据库备份。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full backups [SQL Server]
- database restores [SQL Server], full backups
- backing up databases [SQL Server], full backups
- database backups [SQL Server], full backups
- restoring databases [SQL Server], full backups
ms.assetid: a928fa36-e285-476f-9a7b-6840a8bb7283
author: cawrites
ms.author: chadam
ms.openlocfilehash: 508e567be5fa44e3bf64465dc583f7196dcdb911
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129135"
---
# <a name="restore-a-database-backup-under-the-simple-recovery-model-transact-sql"></a>在简单恢复模式下还原数据库备份 (Transact-SQL)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何还原完整数据库备份。  
  
> [!IMPORTANT]  
>  还原完整数据库备份的系统管理员必须是当前使用要还原的数据库的唯一人员。  
  
## <a name="prerequisites-and-recommendations"></a>先决条件和建议  
  
-   若要还原已加密的数据库，您必须有权访问用于对数据库进行加密的证书或非对称密钥。 如果没有证书或非对称密钥，数据库将无法还原。 因此，只要需要该备份，就必须保留用于对数据库加密密钥进行加密的证书。 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  
  
-   出于安全性考虑，我们建议您不要从未知或不信任的源附加或还原数据库。 此类数据库可能包含恶意代码，这些代码可能会执行非预期的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，或者通过修改架构或物理数据库结构导致错误。 使用来自未知源或不可信源的数据库前，请在非生产服务器上针对数据库运行 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) ，然后检查数据库中的代码，例如存储过程或其他用户定义代码。  
  
## <a name="database-compatibility-level-after-upgrade"></a>升级后的数据库兼容级别  
 升级后， **tempdb**、 **model**、 **msdb** 和 **Resource** 数据库的兼容级别将设置为 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的兼容级别。 **master** 系统数据库保留它在升级之前的兼容级别，除非该级别小于 100。 如果 **master** 的兼容级别在升级前小于 100，升级后兼容级别将设置为 100。  
  
 如果升级前用户数据库的兼容级别为 100 或更高，升级后将保持相应级别。 如果升级前兼容级别为 90，则在升级后的数据库中，兼容级别将设置为 100，该级别为 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]支持的最低兼容级别。  
  
> [!NOTE]  
>  新的用户数据库将继承 **model** 数据库的兼容级别。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-restore-a-full-database-backup"></a>还原完整数据库备份  
  
1.  执行 RESTORE DATABASE 语句可以还原完整数据库备份，同时指定：  
  
    -   要还原的数据库的名称。  
  
    -   从中还原完整数据库备份的备份设备。  
  
    -   NORECOVERY 子句，前提是在还原完整数据库备份之后，还要应用事务日志备份或差异数据库备份。  
  
    > [!IMPORTANT]  
    >  若要还原已加密的数据库，您必须有权访问用于对数据库进行加密的证书或非对称密钥。 如果没有证书或非对称密钥，数据库将无法还原。 因此，只要需要该备份，就必须保留用于对数据库加密密钥进行加密的证书。 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  
  
2.  可以选择指定：  
  
    -   FILE 子句，以此标识备份设备上要还原的备份集。  
  
> [!NOTE]  
>  如果将早期版本的数据库还原到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]，将自动升级该数据库。 通常，该数据库将立即可用。 但是，如果 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库具有全文检索，则升级过程将导入、重置或重新生成它们，具体取决于  **upgrade_option** 服务器属性的设置。 如果将升级选项设置为“导入”(**upgrade_option** = 2) 或“重新生成”(**upgrade_option** = 0)，在升级过程中将无法使用全文检索。 导入可能需要数小时，而重新生成所需的时间最多时可能十倍于此，具体取决于要编制索引的数据量。 另请注意，如果将升级选项设置为“导入”，并且全文目录不可用，则会重新生成关联的全文索引。 若要更改 **upgrade_option** 服务器属性的设置，请使用 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>说明  
 以下示例从磁带中还原 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 的完整数据库备份。  
  
### <a name="example"></a>示例  
  
```  
USE master;  
GO  
RESTORE DATABASE AdventureWorks2012  
   FROM TAPE = '\\.\Tape0';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [完整数据库还原（完整恢复模式）](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)   
 [完整数据库还原（简单恢复模式）](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [备份历史记录和标头信息 (SQL Server)](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [重新生成系统数据库](../../relational-databases/databases/rebuild-system-databases.md)  
  
  
