---
title: FILESTREAM 兼容性 | Microsoft Docs
description: FILESTREAM 将数据存储在文件系统中。 了解在使用具有各种 SQL Server 功能的 FILESTREAM 时要注意的指导原则、限制和提示。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], other SQL Server features and
- FILESTREAM [SQL Server], limitations
ms.assetid: d2c145dc-d49a-4f5b-91e6-89a2b0adb4f3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 53ea4630f147ce576ad071d1ac593e00452944a3
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250613"
---
# <a name="filestream-compatibility-with-other-sql-server-features"></a>FILESTREAM 与其他 SQL Server 功能的兼容性

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  由于 FILESTREAM 数据存在于文件系统中，因此本主题提供了将 FILESTREAM 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的以下功能一起使用时的一些注意事项、指南和限制：  
  
-   [SQL Server Integration Services (SSIS)](#ssis)  
  
-   [分布式查询和链接服务器](#distqueries)  
  
-   [加密](#encryption)  
  
-   [数据库快照](#DatabaseSnapshot)  
  
-   [复制](#Replication)  
  
-   [日志传送](#LogShipping)  
  
-   [数据库镜像](#DatabaseMirroring)  
  
-   [全文索引](#FullText)  
  
-   [故障转移群集](#FailoverClustering)  
  
-   [SQL Server Express](#SQLServerExpress)  
  
-   [包含的数据库](#contained)  
  
##  <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)  
 SQL Server Integration Services (SSIS) 使用 DT_IMAGE SSIS 数据类型像处理任何其他 BLOB 数据一样处理数据流中的 FILESTREAM 数据。  
  
 您可以使用“导入列”转换将文件从文件系统加载到 FILESTREAM 列， 还可以使用“导出列”转换将文件从 FILESTREAM 列提取到文件系统中的另一个位置。  
  
##  <a name="distributed-queries-and-linked-servers"></a><a name="distqueries"></a> 分布式查询和链接服务器  
 你可以通过将 FILESTREAM 数据作为 **varbinary(max)** 数据处理，使用分布式查询和链接服务器来处理 FILESTREAM 数据。 不能在使用由四部分构成的名称的分布式查询中使用 FILESTREAM **PathName()** 函数，甚至在该名称表示本地服务器时也不能使用。 但是，你可以在使用 **OPENQUERY()** 的传递查询的内部查询中使用 **PathName()** 。  
  
##  <a name="encryption"></a><a name="encryption"></a> 加密  
 即使启用了透明数据加密，也不会加密 FILESTREAM 数据。  
  
##  <a name="database-snapshots"></a><a name="DatabaseSnapshot"></a> 数据库快照  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持对 FILESTREAM 文件组使用 [数据库快照](../../relational-databases/databases/database-snapshots-sql-server.md) 功能。 如果 CREATE DATABASE ON 子句中包含 FILESTREAM 文件组，则相应语句将会失败并会引发错误。  
  
 当您使用 FILESTREAM 时，您可以创建标准（非 FILESTREAM）文件组的数据库快照。 这些数据库快照的 FILESTREAM 文件组被标记为脱机。  
  
 对数据库快照中的 FILESTREAM 表执行的 SELECT 语句不能包含 FILESTREAM 列；否则将返回以下错误消息：  
  
 `Could not continue scan with NOLOCK due to data movement.`  
  
##  <a name="replication"></a><a name="Replication"></a> Replication  
 可以将发布服务器上启用了 FILESTREAM 属性的 **varbinary(max)** 列复制到订阅服务器，复制时可以带 FILESTREAM 属性，也可以不带。 若要指定复制列的方式，请使用“项目属性 - \<Article>”对话框，或者使用 [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 或 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 的 @schema_option 参数。 复制到不具有 FILESTREAM 属性的 **varbinary(max)** 列的数据不能超过该数据类型的 2 GB 大小限制；否则，将产生运行时错误。 我们建议，如果您不是将数据复制到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]，请复制 FILESTREAM 属性。 不论指定的架构选项为何，均不支持将包含 FILESTREAM 列的表复制到 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 订阅服务器。  
  
> [!NOTE]  
>  从 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 复制到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 订阅服务器的大数据值最多不得超过 256 MB。 有关详细信息，请参阅 [最大容量规范](../../sql-server/maximum-capacity-specifications-for-sql-server.md)。  
  
### <a name="considerations-for-transactional-replication"></a>事务复制的注意事项  
 如果您使用为事务复制发布的表中的 FILESTREAM 列，请注意以下事项：  
  
-   如果表中的列具有 FILESTREAM 属性，则不能将 *database snapshot* 或 *database snapshot character* 的值用于 [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 的 @sync_method 属性。  
  
-   max text repl size 选项用于指定可以插入到为复制发布的列中的最大数据量。 此选项可用来控制复制的 FILESTREAM 数据的大小。  
  
-   如果您指定了用于复制 FILESTREAM 属性的架构选项，但您筛选出了 FILESTREAM 所需的 **uniqueidentifier** 列或指定不复制对该列的 UNIQUE 约束，则复制操作不复制 FILESTREAM 属性。 该列将只作为 **varbinary(max)** 列复制。  
  
### <a name="considerations-for-merge-replication"></a>合并复制的注意事项  
 如果您使用为合并复制发布的表中的 FILESTREAM 列，请注意以下事项：  
  
-   合并复制和 FILESTREAM 都需要一个数据类型为 **uniqueidentifier** 的列来标识表中的每一行。 如果相应的表中没有这样的列，合并复制会自动添加一列。 合并复制要求该列具有 ROWGUIDCOL 属性集和默认值 NEWID() 或 NEWSEQUENTIALID()。 除这些要求外，FILESTREAM 还要求为该列定义一个 UNIQUE 约束。 这些要求将产生以下结果：  
  
    -   如果您向已经为合并复制发布的表添加 FILESTREAM 列，请确保 **uniqueidentifier** 列具有 UNIQUE 约束。 如果该列不具有 UNIQUE 约束，请向发布数据库中的该表添加一个命名约束。 默认情况下，合并复制将发布此项架构更改，此更改将应用于每个订阅数据库。  
  
         如果您已按照说明手动添加了 UNIQUE 约束，并且要删除合并复制，则您必须先删除 UNIQUE 约束；否则，复制删除操作将失败。  
  
    -   默认情况下，合并复制使用 NEWSEQUENTIALID()，因为与 NEWID() 相比，它可以提供更高的性能。 如果你要向将为合并复制发布的表添加 **uniqueidentifier** 列，请指定 NEWSEQUENTIALID() 作为默认值。  
  
-   合并复制包括为复制大型对象类型而进行的优化。 这种优化由 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 的 @stream_blob_columns 参数控制。 如果设置了用于复制 FILESTREAM 属性的架构选项，则 @stream_blob_columns 参数值将设置为 **true**。 通过使用 [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)，可以替代这种优化。 使用此存储过程可以将 @stream_blob_columns 设置为 **false**。 如果你要将 FILESTREAM 列添加到已经为合并复制发布的表中，建议你使用 sp_changemergearticle 将该选项设置为 **rue** 。  
  
-   如果 FILESTREAM 列中的数据大小超过 2 GB，则在创建项目后为 FILESTREAM 启用架构选项会导致复制失败，且复制过程中会发生冲突。 如果您预计这种情况会发生，建议您删除表项目，然后重新创建它，并在创建时间启用相应的 FILESTREAM 架构选项。  
  
-   合并复制可以使用 [Web 同步](../../relational-databases/replication/web-synchronization-for-merge-replication.md)通过 HTTPS 连接同步 FILESTREAM 数据。 此数据不能超过针对 Web 同步的 50 MB 大小限制；否则，将产生运行时错误。  
  
##  <a name="log-shipping"></a><a name="LogShipping"></a> 日志传送  
 [日志传送](../../database-engine/log-shipping/about-log-shipping-sql-server.md) 支持 FILESTREAM。 主服务器和辅助服务器运行的都必须是 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]或更高版本，并且必须启用 FILESTREAM。  
  
##  <a name="database-mirroring"></a><a name="DatabaseMirroring"></a> 数据库镜像  
 数据库镜像不支持 FILESTREAM。 不能在主体服务器上创建 FILESTREAM 文件组。 不能为包含 FILESTREAM 文件组的数据库配置数据库镜像。  
  
##  <a name="full-text-indexing"></a><a name="FullText"></a> 全文索引  
 [全文索引](../../relational-databases/search/populate-full-text-indexes.md) 功能处理 FILESTREAM 列的方式与其处理 **varbinary(max)** 列的方式相同。 FILESTREAM 表必须有一列包含每个 FILESTREAM BLOB 的文件扩展名。 有关详细信息，请参阅[使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)、[配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)和[sys.fulltext_document_types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md)。  
  
 全文引擎会对 FILESTREAM BLOB 的内容进行索引。 对诸如图像之类的文件进行索引可能没有用。 更新 FILESTREAM BLOB 时，会重新对其进行索引。  
  
##  <a name="failover-clustering"></a><a name="FailoverClustering"></a> 故障转移群集  
 对于故障转移群集，必须将 FILESTREAM 文件组放在共享磁盘上。 必须在群集中将承载 FILESTREAM 实例的每个节点上启用 FILESTREAM。 有关详细信息，请参阅 [在故障转移群集中设置 FILESTREAM](../../relational-databases/blob/set-up-filestream-on-a-failover-cluster.md)。  
  
##  <a name="sql-server-express"></a><a name="SQLServerExpress"></a> SQL Server Express  
 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 支持 FILESTREAM。 10 GB 的数据库大小限制不包括 FILESTREAM 数据容器。  
  
##  <a name="contained-databases"></a><a name="contained"></a> 包含的数据库  
 FILESTREAM 功能需要数据库以外的一些配置。 因此，使用 FILESTREAM 或 FileTable 的数据库并未被完全包含。  
  
 如果您想要使用包含的数据库的某些功能（例如包含的用户），则可以将数据库包含设置为 PARTIAL。 但在此情况下，您必须知道某些数据库设置未包含在数据库中，并且在数据库移动时不自动移动。  
  
## <a name="see-also"></a>另请参阅  
 [二进制大型对象 (Blob) 数据 (SQL Server)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)  
  
