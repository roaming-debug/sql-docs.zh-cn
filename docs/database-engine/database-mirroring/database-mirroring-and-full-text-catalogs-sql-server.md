---
title: 数据库镜像和全文目录
description: 了解如何在具有全文目录和故障转移前/后全文索引的数据库上配置数据库镜像。
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- full-text catalogs [SQL Server], database mirroring
- catalogs [SQL Server], database mirroring
ms.assetid: e34072ae-fe8a-462d-bb03-02fa0987f793
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 67676ff897203f4ae1fea1cbd713453f0188cc66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353225"
---
# <a name="database-mirroring-and-full-text-catalogs-sql-server"></a>数据库镜像和全文目录 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  若要对带有全文目录的数据库进行镜像，请使用常规备份创建主体数据库的完整数据库备份，然后还原备份，以便将数据库复制到镜像服务器。 有关详细信息，请参阅 [为镜像准备镜像数据库 (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)的各版本中均未提供见证服务器实例。  
  
## <a name="full-text-catalog-and-indexes-before-failover"></a>故障转移前的全文目录和索引  
 新建镜像数据库中的全文目录与数据库备份时的全文目录相同。 数据库镜像开始后，对 DDL 语句（CREATE FULLTEXT CATALOG、ALTER FULLTEXT CATALOG、DROP FULLTEXT CATALOG）所做的任意目录级更改都会被记录下来，发送到镜像服务器，在镜像数据库中进行重播。 但是，镜像数据库中不会重新生成索引级更改，因为镜像数据库没有登录到主体服务器上。 因此，当主体数据库中的全文目录内容发生变化时，镜像数据库中的全文目录内容便不再同步。  
  
## <a name="full-text-indexes-after-failover"></a>故障转移后的全文索引  
 故障转移后，在下列情况下，可能需要对新主体服务器上的全文索引进行完全爬网，即便不是必需，也会有所帮助：  
  
-   如果全文索引的更改跟踪功能处于关闭状态，则必须使用下面的语句对该索引启动完全爬网：  
  
     ALTER FULLTEXT INDEX ON *table_name* START FULL POPULATION  
  
-   如果全文索引被配置为自动跟踪更改，则将自动同步全文索引。 但是，同步过程多少会降低全文索引的性能。 如果性能太低，则可以通过关闭更改跟踪然后将其重置为自动的方式来启动完全爬网：  
  
    -   关闭更改跟踪：  
  
         ALTER FULLTEXT INDEX ON *table_name* SET CHANGE_TRACKING OFF  
  
    -   开启自动更改跟踪：  
  
         ALTER FULLTEXT INDEX ON *table_name* SET CHANGE_TRACKING AUTO  
  
    > [!NOTE]  
    >  若要查看是否已开启自动更改跟踪，可以使用 [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md) 函数查询表的 **TableFullTextBackgroundUpdateIndexOn** 属性。  
  
 有关详细信息，请参阅 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)。  
  
> [!NOTE]  
>  在故障转移后启动爬网与在还原后启动爬网相似。  
  
## <a name="after-forcing-service"></a>强制服务后  
 对镜像服务器强制运行服务后（可能造成数据丢失），启动完全爬网。 启动完全爬网的方法取决于是否对全文索引启动了更改跟踪。 有关详细信息，请参阅本主题前面的“故障转移后的全文索引”。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [备份和还原全文目录和索引](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
  
