---
title: 将数据库还原到标记的事务
description: 在对两个或更多相关数据库进行相关更新时，可以在 SQL Server 中使用事务标记将它们恢复到逻辑上一致的点。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- transaction marks [SQL Server]
- marked transactions [SQL Server]
- database restores [SQL Server], inserting transaction marks for
- recovery [SQL Server], related databases
- restoring databases [SQL Server], related database recovery
- database restores [SQL Server], related databases
- marked transactions [SQL Server], creating
- BEGIN TRAN...WITH MARK statement
- two-phase commit
ms.assetid: 50a73574-1a69-448e-83dd-9abcc7cb7e1a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4d07b3b4d85f6ef0e62709f66ea5c738a0dc8110
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129002"
---
# <a name="use-marked-transactions-to-recover-related-databases-consistently"></a>使用标记的事务一致地恢复相关的数据库

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题仅与使用完整恢复模式或大容量日志恢复模式的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库相关。  
  
 在对两个或更多数据库（“相关数据库”  ）进行相关更新时，可以使用事务标记将它们恢复到逻辑上一致的点。 但是，此恢复将丢失在作为恢复点的标记之后提交的所有事务。 只有您在测试相关数据库或不介意丢失近期提交的事务时，标记事务才适用。  
  
 在每个相关数据库中定期标记相关事务将在数据库中建立一系列公用恢复点。 事务标记将记录在事务日志中并包括在日志备份中。 发生灾难时，可以将各数据库还原到相同的事务标记，从而将它们恢复到一致的点。  
  
> [!NOTE]  
>  可以对不同的数据库独立创建各日志备份，而无需同时创建。  
  
 对于以下情况，若要恢复相关数据库，则必须先对各相关数据库中的事务进行标记：  
  
-   一个或多个事务日志已破坏。 需要将数据库集还原到上次进行日志备份时的一致状态。  
  
-   需要将整个数据库集还原到某个更早时间点的相互一致状态。  
  
> [!IMPORTANT]  
>  您仅可以将相关数据库恢复为标记事务，而不是特定时间点。  
  
 有关如何创建标记事务的信息，请参阅本主题后面的“创建标记事务”。  
  
## <a name="typical-scenario-for-using-marked-transactions"></a>使用标记事务的典型方案  
 使用标记事务的典型方案包括以下步骤：  
  
1.  创建每个相关数据库的完整数据库备份或差异数据库备份。  
  
2.  在所有数据库中标记事务块。  
  
3.  备份所有数据库的事务日志。  
  
4.  用 WITH NORECOVERY 还原数据库备份。  
  
5.  用 WITH STOPATMARK 还原日志。  

## <a name="considerations-for-using-marked-transactions"></a>使用标记事务的注意事项  
 在将已命名的标记插入事务日志之前，请注意以下事项：  
  
-   由于事务标记会消耗日志空间，应只对在数据库恢复策略中起重要作用的事务使用标记。  
  
-   标记的事务提交之后，在 [msdb](../../relational-databases/system-tables/logmarkhistory-transact-sql.md) 的 **logmarkhistory** 表中插入一行。  
  
-   如果一个标记的事务跨同一数据库服务器或不同服务器上的多个数据库，这些标记将记录在所有受影响的数据库的日志内。  
  
## <a name="creating-the-marked-transactions"></a>创建标记事务  
 若要创建标记事务，请使用 [BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md) 语句和 WITH MARK [*description*] 子句。 可选内容 *description* 是关于标记的文字说明。 必须为事务指定标记名称。 标记名称可以重复使用。 事务日志记录标记名称、描述、数据库、用户、日期时间信息以及日志序列号 (LSN)。 日期时间信息与标记名称一起使用，以便对标记进行唯一标识。  
  
 **在数据库集中创建标记事务：**  
  
1.  用 BEGIN TRAN 语句命名事务，并使用 WITH MARK 子句。  
  
     你可以在现有事务中嵌套 BEGIN TRAN *new_mark_name* WITH MARK 语句。 即使事务拥有事务名称， *new_mark_name* 的值也是事务的标记名称。  
  
    > [!NOTE]  
    >  如果执行第二个嵌套 BEGIN TRAN...WITH MARK，那么将跳过该语句，但会导致出现警告消息。  
  
2.  对数据库集中的所有数据库进行更新。  
  
     将仅在执行 BEGIN TRAN...WITH MARK 语句的服务器实例上的事务日志中插入特定事务的标记。 事务标记放置在由该服务器实例上的标记事务更新过的各数据库的事务日志中。 如果数据库位于不同的服务器实例上，则必须在各服务器实例上创建相同的标记。  
  
### <a name="examples"></a>示例  
 下面的示例将事务日志还原到名为 `ListPriceUpdate`的标记事务中的标记处。  
  
```sql  
USE AdventureWorks  
GO  
BEGIN TRANSACTION ListPriceUpdate  
   WITH MARK 'UPDATE Product list prices';  
GO  
  
UPDATE Production.Product  
   SET ListPrice = ListPrice * 1.10  
   WHERE ProductNumber LIKE 'BK-%';  
GO  
  
COMMIT TRANSACTION ListPriceUpdate;  
GO  
  
-- Time passes. Regular database   
-- and log backups are taken.  
-- An error occurs in the database.  
USE master  
GO  
  
RESTORE DATABASE AdventureWorks  
FROM AdventureWorksBackups  
WITH FILE = 3, NORECOVERY;  
GO  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups   
   WITH FILE = 4,  
   RECOVERY,   
   STOPATMARK = 'ListPriceUpdate';  
```  
  
## <a name="forcing-a-mark-to-spread-to-other-servers"></a>强制将标记分布到其他服务器  
 事务分散到其他服务器时，事务标记名称不会自动分布到其他服务器。 若要强制标记分散到其他服务器，必须编写包含 BEGIN TRAN *名称* WITH MARK 语句的存储过程。 然后，必须在发起服务器上的事务作用域下，在远程服务器上执行该存储过程。  
  
 例如，假设多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上存在分区数据库。 每个实例上都有一个名为 `coyote`的数据库。 首先，在每个数据库中创建一个存储过程，例如， `sp_SetMark`。  
  
```sql  
CREATE PROCEDURE sp_SetMark  
@name nvarchar (128)  
AS  
BEGIN TRANSACTION @name WITH MARK  
UPDATE coyote.dbo.Marks SET one = 1  
COMMIT TRANSACTION;  
GO  
```  
  
 然后，创建包含在每个数据库中放置标记的事务的存储过程 `sp_MarkAll` 。 `sp_MarkAll` 可以从任意实例中运行。  
  
```sql  
CREATE PROCEDURE sp_MarkAll  
@name nvarchar (128)  
AS  
BEGIN TRANSACTION  
EXEC instance0.coyote.dbo.sp_SetMark @name  
EXEC instance1.coyote.dbo.sp_SetMark @name  
EXEC instance2.coyote.dbo.sp_SetMark @name  
COMMIT TRANSACTION;  
GO  
```  
  
### <a name="two-phase-commit"></a>两阶段提交  
 提交分布式事务分为两个阶段：准备和提交。 提交标记事务时，标记事务中每个数据库的提交日志记录都放在日志的某一点处，以使任何日志中不存在有疑问的事务。 此点保证不存在这样的事务：在一个日志中显示为已提交，在另一个日志中显示为未提交。  
  
 为此，可在提交标记事务的过程中执行下列操作：  
  
1.  标记事务的准备阶段停止所有新的准备和提交。  
  
2.  仅允许继续提交准备好的事务。  
  
3.  标记事务将等待耗尽所有准备好的事务（带超时设置）。  
  
4.  准备并提交标记事务。  
  
5.  取消停止新的准备和提交。  
  
 由跨多个数据库的标记事务产生的停止会降低服务器的事务处理性能。  
  
 建议不要运行并发标记事务。 虽然很少见，但分布式标记事务有可能与其他同时提交的分布式标记事务发生死锁。 出现这种情况时，正在标记的事务将被选作死锁牺牲品并回滚。 出现此错误时，应用程序会重试标记事务。 多个标记事务尝试同时提交时，发生死锁的可能性较大。  
  
## <a name="recovering-to-a-marked-transaction"></a>恢复到标记的事务  
 有关如何将包含标记事务的数据库恢复到特定标记或仅恢复到特定标记之前，请参阅 [包含标记事务的相关数据库恢复](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md)。  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/full-database-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [包含标记的事务的相关数据库的恢复](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md)  
  
  
