---
description: 复制已分区表和索引
title: 复制已分区表和索引 | Microsoft Docs
ms.custom: ''
ms.date: 09/10/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- partitioned indexes [SQL Server], replicating
- partitioned tables [SQL Server], replicating
- replication [SQL Server], partitioned tables
- publishing [SQL Server replication], partitioned tables
- transactional replication, partitioned tables
ms.assetid: c9fa81b1-6c81-4c11-927b-fab16301a8f5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 75c0f38e67fd4d02791c5d2b953f4354a5945dc2
ms.sourcegitcommit: e5664d20ed507a6f1b5e8ae7429a172a427b066c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697143"
---
# <a name="replicate-partitioned-tables-and-indexes"></a>复制已分区表和索引
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  由于使用分区可以快速而有效地管理和访问数据子集，并同时保持数据集合的完整性，因而使大型表或索引更易于管理。 有关详细信息，请参阅 [Partitioned Tables and Indexes](../../../relational-databases/partitions/partitioned-tables-and-indexes.md)。 复制支持分区，提供了一组属性来指定如何处理已分区表和索引。  
  
## <a name="article-properties-for-transactional-and-merge-replication"></a>事务复制和合并复制的项目属性  
 下表列出了用于对数据进行分区的对象。  
  
|对象|使用以下内容创建|  
|------------|----------------------|  
|已分区表或已分区索引|CREATE TABLE 或 CREATE INDEX|  
|分区函数|CREATE PARTITION FUNCTION|  
|分区方案|CREATE PARTITION SCHEME|  
  
 分区属性是项目架构选项，用于决定是否应将分区对象复制到订阅服务器。 可以按下列方式设置这些架构选项：  
  
-   在新建发布向导的 **“项目属性”** 页或者“发布属性”对话框中。 若要复制上一个表中列出的对象，请为属性 **“复制表分区方案”** 和 **“复制索引分区方案”** 指定 **true** 值。 有关如何访问“项目属性”页的信息，请参阅[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
-   通过使用以下其中一个存储过程的 *schema_option* 参数：  
  
    -   用于事务复制的[sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 或 [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)  
  
    -   用于合并复制的[sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 或 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)  
  
     若要复制上一个表中列出的对象，请指定相应架构选项值。 有关如何指定架构选项的信息，请参阅 [Specify Schema Options](../../../relational-databases/replication/publish/specify-schema-options.md)。  
  
 在初始同步阶段，复制会将这些对象复制到订阅服务器。 如果分区方案使用的文件组不是 PRIMARY 文件组，则在进行初步同步之前，这些文件组必须位于订阅服务器上。  
  
 在初始化订阅服务器后，数据更改传播到订阅服务器上，并应用于相应的分区。 不过，不支持对分区方案的更改。 事务复制和合并复制不支持复制下列命令：ALTER INDEX 的 ALTER PARTITION FUNCTION、ALTER PARTITION SCHEME 或 REBUILD WITH PARTITION 语句。 与之关联的更改将不会自动复制到订阅服务器。 由用户负责在订阅服务器上手动进行类似的更改。  
  
## <a name="replication-support-for-partition-switching"></a>复制支持分区切换  
 表分区的主要优点之一是能快速而有效地在分区之间移动数据的子集。 数据是使用 SWITCH PARTITION 命令移动的。 默认情况下，当某个表启用为进行复制时，由于下列原因而阻止 SWITCH PARTITION 操作：  
  
-   如果将数据移入或移出发布服务器上存在（但订阅服务器上不存在）的表，则发布服务器和订阅服务器之间可能会变得不一致。 在将数据移入或移出临时表时通常会发生此问题。  
  
-   如果订阅服务器和发布服务器具有不同的已分区表定义，当分发代理尝试在订阅服务器上应用更改时将失败。  
  
尽管存在这些潜在问题，仍然可以为事务复制启用分区切换。 在启用分区切换前，请确发布服务器和订阅服务器上存在分区切换涉及的所有表，并确保表定义和分区定义相同。  
  
当分区在发布服务器和订阅服务器上具有完全相同的分区方案时，你可以启用 allow_partition_switch 和 replication_partition_switch，这只会将分区切换语句复制到订阅服务器。 您也可以启用 *allow_partition_switch* 而不复制 DDL。 这在以下情况下非常有用：您希望从分区中淘汰已经过的月份，但为另一年保留复制的分区，以便在订阅服务器上进行备份。  
  
如果在 SQL Server 2008 R2 上通过当前版本启用分区切换，稍后可能还需要执行拆分和合并操作。 在复制或启用了 CDC 的表上执行拆分或合并操作之前，请确保所涉及的分区没有任何待定的复制命令。 你还应确保在拆分和合并操作期间，该分区上没有执行任何 DML 操作。 如果存在日志读取器或 CDC 捕获作业尚未处理的事务，或者执行拆分或合并操作期间在复制或启用了 CDC·的表的分区上执行 DML 操作（涉及同一个分区），将导致日志读取器代理或 CDC 捕获作业出现处理错误（错误 608 - 找不到分区 ID 的目录条目）。 为了更正错误，可能需要重新初始化订阅或禁用该表或数据库上的 CDC。 

### <a name="unsupported-scenarios"></a>不支持的方案

使用带有分区切换功能的复制时，不支持以下方案： 

对等复制   
分区切换不支持对等复制。 

使用带有分区切换的变量   

对于 `ALTER TABLE ... SWITCH TO ... PARTITION ...` 语句，不支持在使用事务复制或变更数据捕获 (CDC) 发布的表上使用带有分区切换的变量。

例如，以下分区切换代码将不适用于启用了 CDC 的数据库，也不适用于参与事务发布的 TableA： 

```sql
DECLARE @SomeVariable INT = $PARTITION.pf_test(10);
ALTER TABLE dbo.TableA
SWITCH TO dbo.TableB 
PARTITION @SomeVariable;
```

请改用分区函数直接切换分区，如以下示例所示： 

```sql
ALTER TABLE NonPartitionedTable 
SWITCH TO PartitionedTable PARTITION $PARTITION.pf_test(10);
```


### <a name="enabling-partition-switching"></a>启用分区切换  
 使用事务发布的下列属性，用户可以控制已复制环境中分区切换的行为。  
  
-   当 `@allow_partition_switch` 设置为 `true` 时，可以对发布数据库执行 SWITCH PARTITION。  
  
-   `@replicate_partition_switch` 确定 SWITCH PARTITION DDL 语句是否应复制到订阅服务器。 仅当 `@allow_partition_switch` 设置为 `true` 时才有效。  
  
 创建发布时可以使用 [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 设置这些属性，或在创建发布后使用 [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) 设置这些属性。 如上所述，合并复制不支持分区切换。 若要对已启用合并复制的表执行 SWITCH PARTITION，请从发布中删除该表。  
  
## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
