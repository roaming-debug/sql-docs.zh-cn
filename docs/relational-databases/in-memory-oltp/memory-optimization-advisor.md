---
title: 内存优化顾问 | Microsoft Docs
description: 在 SQL Server Management Studio 中使用内存优化顾问有助于将基于磁盘的表迁移到内存优化表。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
f1_keywords:
- swb.memoryoptimizationwizard.f1
- sql13.swb.memoryoptimizationwizard.f1
ms.assetid: 181989c2-9636-415a-bd1d-d304fc920b8a
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0626ff511e504a4718a426ae9b1ca85c674ab5f0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473868"
---
# <a name="memory-optimization-advisor"></a>内存优化顾问
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  借助事务性能分析报告（请参阅 [确定是否应将表或存储过程移植到内存中 OLTP](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)），你可以了解数据库中的哪些表在移植后使用内存中 OLTP 会给你带来好处。 找到要移植以使用内存中 OLTP 的表之后，可使用 SQL Server Management Studio 中的内存优化顾问帮助你将基于磁盘的表迁移到内存优化的表中。  
  
 借助内存优化顾问，你可以：  
  
-   确定基于磁盘的表中使用的不受内存优化表支持的任何功能。  
  
-   将表和数据迁移到内存优化表中（如果使用的功能均受支持的话）。  
    
 有关迁移方法的信息，请参阅 [内存中 OLTP - 常见的工作负荷模式和迁移注意事项](/previous-versions/dn673538(v=msdn.10))。  
  
## <a name="walkthrough-using-the-memory-optimization-advisor"></a>使用内存优化顾问进行演练  
 在 **对象资源管理器** 中，右键单击要转换的表，然后选择 **“内存优化顾问”** 。 这将显示 **“表内存优化顾问”** 的欢迎使用页。  
  
### <a name="memory-optimization-checklist"></a>内存优化核对清单  
 当您在 **“表内存优化顾问”** 的欢迎使用页中单击 **“下一步”** 时，将会看到内存优化核对清单。 内存优化表并不支持基于磁盘的表中的所有功能。 内存优化核对清单将报告基于磁盘的表是否使用了与内存优化表不兼容的任何功能。 **“表内存优化顾问”** 不会修改基于磁盘的表，以便它可以迁移到使用内存中 OLTP。 您必须首先进行这些更改，然后才能继续迁移。 对于找到的每个不兼容之处， **“表内存优化顾问”** 都会显示一个链接，指向可帮助您修改基于磁盘的表的信息。  
  
 如果您想要保留这些不兼容之处的列表以便计划您的迁移，则单击 **“生成报表”** 以便生成一个 HTML 列表。  
  
 如果您的表没有不兼容之处并且连接到了具有内存中 OLTP 的某一 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 实例，则单击 **“下一步”** 。  
  
### <a name="memory-optimization-warnings"></a>内存优化警告  
 下一页“内存优化警告”包含问题的列表，这些问题不会阻止表迁移到使用内存中 OLTP，但这些问题可能导致其他对象（例如存储过程或 CLR 函数）的行为失败或者导致意外行为。  
  
 该列表中的前几个警告仅供参考，不一定适用于您的表。 该表的右键单击列中的链接将会给您提供更多的信息。  
  
 该警告表还将显示在您的表中未出现的可能的警告条件。  
  
 可操作警告将在左侧列中具有一个黄色的三角形。 如果存在可操作警告，您应该退出迁移，解决警告问题，然后重新开始迁移过程。 如果您没有解决警告问题，则迁移的表可能会导致失败。  
  
 单击 **“生成报表”** 可生成包含这些警告的 HTML 报表。 单击“下一步”继续。  
  
### <a name="review-optimization-options"></a>检查优化选项  
 下一个屏幕可用于修改用于迁移到内存中 OLTP 的选项：  
  
 内存优化的文件组  
 您的内存优化的文件组的名称。 一个数据库必须首先具有含至少一个文件的内存优化的文件组，然后才能创建内存优化表。  
  
 如果您没有内存优化的文件组，则可以更改默认名称。 内存优化的文件组不能删除。 存在内存优化的文件组可能会禁用某些数据库级别的功能，例如 AUTO CLOSE 和数据库镜像。  
  
 如果某个数据库具有内存优化的文件组，则该字段将用其名称预先填充，并且您将不能更改该字段的值。  
  
 逻辑文件名和文件路径  
 将包含内存优化表的文件的名称。 一个数据库必须首先具有含至少一个文件的内存优化的文件组，然后才能创建内存优化表。  
  
 如果您没有现有的内存优化的文件组，则可以将该文件的默认名称和路径更改为在迁移过程结束时创建。  
  
 如果您具有现有的内存优化的文件组，则这些字段将预先填充并且您将不能更改这些值。  
  
 重命名原始表  
 在迁移过程结束时，将用该表的当前名称创建一个新的内存优化表。 为避免名称冲突，必须重命名当前表。 您可以在此字段中更改该名称。  
  
 估计的当前内存开销 (MB)  
 内存优化顾问将会根据基于磁盘的表的元数据，估计新的内存优化表将使用的内存量。 [内存优化表中的表和行大小](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)中说明了表大小的计算。  
  
 如果没有分配足够的内存，迁移过程将失败。  
  
 还将表数据复制到新的内存优化表  
 如果您还要将当前表中的数据移到新的内存优化表，则选择此选项。 如果未选择此选项，将创建新的内存优化表，但其中不含任何行。  
  
 默认情况下该表将作为持久表迁移  
 内存中 OLTP 支持非持久表，这些表具有与持久的内存优化表相当的优异性能。 但是，在服务器重新启动后非持久表中的数据将会丢失。  
  
 如果选择了此选项，内存优化顾问将创建非持久表，而不是持久表。  
  
> [!WARNING]  
>  只有在您明了与非持久表相关联的数据丢失风险的情况下，才选择此选项。  
  
 单击“下一步”以继续。  
  
### <a name="review-primary-key-conversion"></a>检查主键转换  
 下一个屏幕是 **“检查主键转换”** 。 内存优化顾问将检测到表中是否有一个或多个主键，并且基于主键元数据填充列的列表。 否则，如果您想要迁移到持久的内存优化表，则必须创建主键。  
  
 如果主键不存在并且该表正迁移到非持久表，则该屏幕将不会出现。  
  
 对于文本列（类型为 **char**、 **nchar**、 **varchar** 和 **nvarchar** 的列），必须选择相应的排序规则。 对于内存优化表上的列，内存中 OLTP 仅支持 BIN2 排序规则，并且不支持具有增补字符的排序规则。 有关支持的排序规则以及排序规则中更改的潜在影响的信息，请参阅 [Collations and Code Pages](./introduction-to-memory-optimized-tables.md) 。  
  
 可为主键配置以下参数：  
  
 为该主键选择一个新名称  
 该表的主键名称在数据库内必须唯一。 您可在此处更改主键的名称。  
  
 选择该主键的类型  
 内存中 OLTP 在内存优化表上支持两种类型的索引：  
  
-   NONCLUSTERED HASH 索引。 此索引最适合于具有许多点查找的索引。 您可以在 **“存储桶计数”** 字段中为此索引配置存储桶计数。  
  
-   NONCLUSTERED 索引。 此类型的索引最适合于具有许多范围查询的索引。 您可以在 **“排序列和顺序”** 列表中配置每一列的排序顺序。  
  
 若要了解最适合你的主键的索引的类型，请参阅 [哈希索引](/previous-versions/sql/sql-server-2016/dn133190(v=sql.130))。  
  
 在选择了您的主键后单击 **“下一步”** 。  
  
### <a name="review-index-conversion"></a>检查索引转换  
 下一页是 **“检查索引转换”** 。 内存优化顾问将检测到表中是否有一个或多个索引，并且填充列的列表和数据类型。 您在 **“检查索引转换”** 页中可以配置的参数类似于前一页 **“检查主键转换”** 。  
  
 如果该表仅具有主键并且正迁移到持久表，则该屏幕将不会出现。  
  
 在确定了表中的每个索引后，单击 **“下一步”** 。  
  
### <a name="verify-migration-actions"></a>验证迁移操作  
 下一页是 **“验证迁移操作”** 。 若要编写迁移操作的脚本，请单击 **“脚本”** 以便生成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 然后，您可以修改并且执行该脚本。 单击 **“迁移”** 可开始表迁移。  
  
 在完成该过程后，刷新 **对象资源管理器** 可查看新的内存优化表和旧的基于磁盘的表。 您可以保留旧表或者在方便时删除它。  
  
## <a name="see-also"></a>另请参阅  
 [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
