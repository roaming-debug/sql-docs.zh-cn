---
title: 发布存储过程执行（事务）
description: 了解如何将在发布服务器上执行并影响已发布表的存储过程作为存储过程执行项目包含到事务发布中。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publishing [SQL Server replication], stored procedure execution
- articles [SQL Server replication], stored procedures and
- transactional replication, publishing stored procedure execution
ms.assetid: f4686f6f-c224-4f07-a7cb-92f4dd483158
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0f339c2be8c7424134ba76fabbed1105d447a621
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479588"
---
# <a name="publishing-stored-procedure-execution-in-transactional-replication"></a>在事务复制中发布存储过程执行
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  如果在发布服务器上执行一个或多个存储过程并影响已发布的表，请考虑将这些存储过程作为存储过程执行项目包括在发布中。 初始化订阅时，过程定义（CREATE PROCEDURE 语句）将被复制到订阅服务器上；当在发布服务器上执行过程时，复制将在订阅服务器上执行相应的过程。 在执行大量批处理操作的情况下，这可以显著提高性能，因为这仅复制此过程执行而不需要为每一行复制各种更改。 例如，假设在发布数据库中创建下面的存储过程：  
  
```  
CREATE PROC give_raise AS  
UPDATE EMPLOYEES SET salary = salary * 1.10  
```  
  
 此过程用于为贵公司的 10,000 名雇员每人增加 10% 的薪水。 在发布服务器上执行此存储过程时，它会更新每位雇员的薪水。 若没有复制存储过程执行，此更新将被作为一个大型的、多步骤事务发送到订阅服务器：  
  
```  
BEGIN TRAN  
UPDATE EMPLOYEES SET salary = salary * 1.10 WHERE PK = 'emp 1'  
UPDATE EMPLOYEES SET salary = salary * 1.10 WHERE PK = 'emp 2'  
```  
  
 如此重复进行 10,000 次更新。  
  
 如果复制存储过程执行，则复制将仅发送在订阅服务器上执行存储过程的命令，而不会将所有更新都写入分发数据库并随后通过网络将它们发送到订阅服务器：  
  
```  
EXEC give_raise  
```  
  
> [!IMPORTANT]  
>  存储过程复制并非适用于所有应用程序。 如果对某个项目进行水平筛选，以致发布服务器上存在不同于订阅服务器的行集，那么在这两个服务器上执行同一个存储过程将返回不同的结果。 与此类似，如果某个更新基于另一个未复制表的子查询，那么在发布服务器和订阅服务器上同时执行相同的存储过程将返回不同的结果。  
  
 **发布存储过程的执行**  
  
-   SQL Server Management Studio：[在事务发布中发布存储过程的执行 (SQL Server Management Studio)](../../../relational-databases/replication/publish/publish-execution-of-stored-procedure-in-transactional-publication.md)  
  
-   复制 Transact-SQL 编程：执行 [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 并为参数 `@type` 指定“serializable proc exec”（推荐）或“proc exec”值。 有关如何定义项目的详细信息，请参阅[定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
## <a name="modifying-the-procedure-at-the-subscriber"></a>在订阅服务器上修改过程  
 默认情况下，发布服务器上的存储过程定义会传播到每个订阅服务器。 但是，还可以在订阅服务器上修改存储过程。 这有助于在发布服务器和订阅服务器上执行不同的逻辑。 例如，假设发布服务器上的存储过程 **sp_big_delete** 有两个作用：从复制的表 **big_table1** 中删除 1,000,000 行；更新未复制的表 **big_table2**。 为了减少对网络资源的需求，应通过发布 **sp_big_delete** 将 1 百万行删除作为一个存储过程进行传播。 在订阅服务器上，您可以修改 **sp_big_delete** 只删除 1 百万行且不对 **big_table2** 执行后续更新。  
  
> [!NOTE]  
>  默认情况下，发布服务器上使用 ALTER PROCEDURE 进行的任何更改都会传播到订阅服务器上。 为避免出现这种情况，在执行 ALTER PROCEDURE 之前禁用架构更改的传播。 有关架构更改的信息，请参阅[对发布数据库进行架构更改](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。  
  
## <a name="types-of-stored-procedure-execution-articles"></a>存储过程执行项目的类型  
 发布存储过程的执行有两种不同方式：可序列化过程执行项目和过程执行项目。  
  
-   建议使用可序列化选项，因为仅当过程在可序列化事务的上下文内执行时，才能复制过程执行。 如果存储过程从可序列化事务外部执行，则对已发布的表中数据的更改将作为一系列 DML 语句被复制。 此行为有助于使订阅服务器上的数据与发布服务器上的数据一致。 这对批处理操作（例如大量的清除操作）尤其有用。  
  
-   通过使用过程执行选项，可以将执行复制到所有订阅服务器，无论存储过程中的单个语句是否成功。 此外，由于存储过程引起的数据更改可能在多个事务内发生，因此订阅服务器上的数据可能不能与发布服务器上的数据一致。 若要解决这些问题，要求订阅服务器为只读，并且您使用的是大于未提读书的隔离级别。 如果使用未提交读，则已发布表中的数据更改将复制为一系列 DML 语句。  
  
 下面的示例说明了建议将过程复制设置为可序列化过程项目的原因。  
  
```  
BEGIN TRANSACTION T1  
SELECT @var = max(col1) FROM tableA  
UPDATE tableA SET col2 = <value>   
   WHERE col1 = @var   
  
BEGIN TRANSACTION T2  
INSERT tableA VALUES <values>  
COMMIT TRANSACTION T2  
```  
  
 在前面的示例中，假设事务 T1 中的 SELECT 出现在事务 T2 中的 INSERT 之前。  
  
 如果该过程不在可序列化事务内执行（隔离级别设置为 SERIALIZABLE），则允许事务 T2 在 T1 中 SELECT 语句范围内插入新行，且在 T1 之前提交。 这还意味着在订阅服务器上先应用 T2，后应用 T1。 在订阅服务器上应用 T1 时，SELECT 返回的值可能与发布服务器上的值不同，且可能导致与 UPDATE 不同的结果。  
  
 如果在可序列化事务中执行该过程，将不允许将事务 T2 插入到 T2 中的 SELECT 语句所包括的范围内。 T2 将被阻塞，直到 T1 提交可以确保与订阅服务器上的结果相同。  
  
 在可序列化事务中执行过程时控制锁的时间将更长，并可能导致并发减少。  
  
## <a name="the-xact_abort-setting"></a>XACT_ABORT 设置  
 在复制存储过程执行时，执行此存储过程的会话设置应将 XACT_ABORT 指定为 ON。 如果 XACT_ABORT 设置为 OFF，在发布服务器上执行此过程时将出现错误，并且订阅服务器上也会出现相同的错误，这将导致分发代理失败。 将 XACT_ABORT 指定为 ON 可确保在发布服务器上执行时遇到的任何错误会使整个执行回滚，从而避免分发代理失败。 有关如何设置 XACT_ABORT 的详细信息，请参阅 [SET XACT_ABORT (Transact-SQL)](../../../t-sql/statements/set-xact-abort-transact-sql.md)。  
  
 如果需要将 XACT_ABORT 设置为 OFF，请指定分发代理的 **-SkipErrors** 参数。 这使得代理即使在遇到错误的情况下也会继续在订阅服务器上应用更改。  
  
## <a name="see-also"></a>另请参阅  
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
