---
description: 用于索引的 SORT_IN_TEMPDB 选项
title: 用于索引的 SORT_IN_TEMPDB 选项 | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- SORT_IN_TEMPDB option
- disk space [SQL Server], indexes
- space [SQL Server], indexes
- tempdb database [SQL Server], indexes
- indexes [SQL Server], tempdb database
- index creation [SQL Server], tempdb database
ms.assetid: 754a003f-fe51-4d10-975a-f6b8c04ebd35
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f8f1ab5eb19e5590c5a4e3604469f93c2d524aac
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478178"
---
# <a name="sort_in_tempdb-option-for-indexes"></a>用于索引的 SORT_IN_TEMPDB 选项
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  当创建或重新生成索引时，通过将 SORT_IN_TEMPDB 选项设置为 ON，可以指定 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 使用 **tempdb** 来存储用于生成索引的中间排序结果。 虽然此选项会增加创建索引所用的临时磁盘空间量，但是当 **tempdb** 与用户数据库位于不同的磁盘集上时，该选项可减少创建或重新生成索引所需的时间。 有关 **tempdb** 的详细信息，请参阅 [配置 index create memory 服务器配置选项](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md)。  
  
## <a name="phases-of-index-building"></a>索引生成阶段  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 生成索引时经历下面两个阶段：  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 首先扫描基表的数据页以检索键值，并为每个数据行生成索引叶行。 当内部排序缓冲区被叶索引项填满时，这些项被排序并作为中间排序运行指令写入磁盘。 然后， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 继续扫描数据页，直到排序缓冲区再次被填满。 这种先扫描多个数据页、然后排序并写入排序运行指令的模式继续进行，直到处理完基表中的所有行。  
  
     在聚集索引中，索引的叶行是表的数据行，因此中间排序运行包含所有数据行。 在非聚集索引中，叶行可能包含非键列，但通常比聚集索引小。 如果索引键很大，或者索引中包含多个非键列，则非聚集排序运行指令也可能很大。 有关包含非键列的详细信息，请参阅 [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md)。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将排序的索引叶行进程合并为单个排序流。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的排序合并组件从每个排序运行指令的第一页开始，在所有页中找出最低键，并将该叶行传递到索引创建组件。 然后处理下一个最低键，再处理下一个，依此类推。 当将最后一个叶索引行从排序运行指令页中提取出来时，该进程从此排序进程切换到下一页。 当处理完某个排序运行指令区中的所有页时，将释放该区。 每个叶索引行在传递到索引创建组件时，均包含在缓冲区的叶索引页中。 每个叶页在填充时被写入。 当写入叶页时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 还生成该索引的上级。 每个上级索引页在填充时被写入。  
  
## <a name="sort_in_tempdb-option"></a>SORT_IN_TEMPDB 选项  
 如果 SORT_IN_TEMPDB 设置为 OFF（默认设置），则排序运行指令将存储在目标文件组中。 在创建索引的第一阶段，交替读取基表页和写入排序运行指令会将读/写磁头从磁盘的一个区域移到另一个区域。 扫描数据页时，磁头位于数据页区域。 当填充排序缓冲区且当前排序运行指令必须写入磁盘时，磁头将移到某个可用空间区域，然后在继续扫描表页时移回数据页区域。 在第二阶段，读/写磁头的移动频率较高。 这时，排序进程通常交替读取各个排序运行区域。 排序运行指令和新的索引页都在目标文件组中生成。 这意味着 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 在排序运行指令中分布读取的同时，还必须定期跳至索引区，以便在填充索引页时写入新的索引页。  
  
 如果 SORT_IN_TEMPDB 选项设置为 ON，并且 **tempdb** 与目标文件组位于不同的磁盘集上，那么在第一阶段，对数据页的读取与对 **tempdb** 中排序工作区的写入发生在不同的磁盘上。 这意味着对数据键的磁盘读取在整个磁盘上通常较连续地进行，而对 **tempdb** 磁盘的写入通常也是连续的，就像生成最终索引的写入一样。 即使其他用户正在使用数据库且正在访问不同的磁盘地址，指定 SORT_IN_TEMPDB 选项时的总体读写模式的效率也比没有指定时要高。  
  
 SORT_IN_TEMPDB 选项可能会提高索引区的邻接，尤其当不是并行处理 CREATE INDEX 操作时。 排序工作区在数据库中的释放位置方面有些随机。 如果排序工作区包含在目标文件组中，则释放排序工作区时，可通过请求来获取它们，以使区在生成时具有索引结构。 这在某种程度上会随机化索引区的位置。 如果排序区单独保存在 **tempdb** 中，则释放排序区的顺序对索引区的位置没有影响。 此外，当中间排序运行指令存储在 **tempdb** 中而不是目标文件组中时，目标文件组中的可用空间较多。 这增加了索引区连续的可能性。  
  
 SORT_IN_TEMPDB 选项仅影响当前的语句。 没有任何元数据记录索引是否存储在 **tempdb** 中。 例如，如果使用 SORT_IN_TEMPDB 选项创建一个非聚集索引，然后在不指定该选项的情况下创建一个聚集索引，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 在重新创建该聚集索引时不使用该选项。  
  
> [!NOTE]  
>  如果不需要排序操作或可以在内存中执行排序，则忽略 SORT_IN_TEMPDB 选项。  
  
## <a name="disk-space-requirements"></a>磁盘空间要求  
 如果将 SORT_IN_TEMPDB 选项设置为 ON，则 **tempdb** 中必须有足够的可用空间容纳中间排序运行指令，且目标文件组中必须有足够的可用空间容纳新的索引。 如果可用空间不足，并且由于某些原因数据库无法自动增长以获得更多空间（如磁盘上无空间或自动增长设置为关闭），则 CREATE INDEX 语句将失败。  
  
 如果 SORT_IN_TEMPDB 设置为 OFF，目标文件组中的可用磁盘空间必须大约等于最终索引的大小。 在第一阶段，排序运行指令将生成并要求可用空间量大约等于最终索引的大小。 在第二阶段，处理每个排序运行指令区后将其释放。 这意味着释放排序运行指令区的速度与获取区以容纳最终索引页的速度大概相同，因此，总空间要求不会明显超过最终索引的大小。 这样的一个副作用是如果可用空间量非常接近最终索引的大小，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 通常会在释放排序运行指令区后立即重新使用它们。 因为排序运行指令区的释放方式有些随机，所以在这种情形中将降低索引区的连续性。 SORT_IN_TEMPDB 设置为 OFF 时，如果目标文件组中有足够的可用空间，则可以从邻接的池而不是从刚刚释放的排序段区中分配索引区，这将提高索引区的连续性。  
  
创建非聚集索引时，必须有足够的可用空间：  
  
-   如果 SORT_IN_TEMPDB 设置为 ON，则 **tempdb** 中必须有足够的可用空间来存储排序运行指令，且目标文件组中必须有足够的可用空间来存储最终索引结构。 排序运行指令包含索引的叶行。  
  
-   如果 SORT_IN_TEMPDB 设置为 OFF，则目标文件组中必须有足够的可用空间来存储最终索引结构。 如果具有更多的可用空间，则可以提高索引区的连续性。  
  
对没有非聚集索引的表创建聚集索引时，必须有足够的可用空间：  
  
-   如果 SORT_IN_TEMPDB 设置为 ON，则 **tempdb** 中必须有足够的可用空间来存储排序运行指令。 其中包括表的数据行。 目标文件组中必须有足够的可用空间来存储最终索引结构。 包括表的数据行和索引 B 树。 您可能需要根据不同的因素调整估计值，如键的大小很大或填充因子的值很低。  
  
-   如果 SORT_IN_TEMPDB 设置为 OFF，目标文件组中必须有足够的可用空间来存储最终表。 包括索引结构。 如果具有更多的可用空间，则可以提高表和索引区的连续性。  
  
对具有非聚集索引的表创建聚集索引时，必须有足够的可用空间：  
  
-   如果 SORT_IN_TEMPDB 设置为 ON，则 **tempdb** 中必须有足够的可用空间来存储最大索引（通常是聚集索引）的排序运行指令的集合，且目标文件组中必须有足够的可用空间来存储所有索引的最终结构。 包括包含表的数据行的聚集索引。  
  
-   如果 SORT_IN_TEMPDB 设置为 OFF，目标文件组中必须有足够的可用空间来存储最终表。 包括所有索引的结构。 如果具有更多的可用空间，则可以提高表和索引区的连续性。  
  
## <a name="related-tasks"></a>Related Tasks  
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)  
  
 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)  
  
## <a name="related-content"></a>相关内容  
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)  
  
 [配置 index create memory 服务器配置选项](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md)  
  
 [索引 DDL 操作的磁盘空间要求](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md)  
  
  
