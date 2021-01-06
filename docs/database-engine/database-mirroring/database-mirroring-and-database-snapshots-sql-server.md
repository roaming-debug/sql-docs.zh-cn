---
title: 数据库镜像和数据库快照
description: 了解将数据库快照与数据库镜像一起使用的互操作性，以减轻 SQL Server 中报表的负载。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- snapshots [SQL Server database snapshots], database mirroring
- database snapshots [SQL Server], database mirroring
ms.assetid: 0bf1be90-7ce4-484c-aaa7-f8a782f57c5f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: d4dc48ce78438ef6b2a50576cde51cbe2bb12085
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642126"
---
# <a name="database-mirroring-and-database-snapshots-sql-server"></a>数据库镜像和数据库快照 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  可以利用为了实现可用性目标而维护的镜像数据库来减轻报表的负载。 若要将镜像数据库用于报表，可以在镜像数据库中创建数据库快照，并将客户端连接请求定向到最新的快照。 由于数据库快照只在创建快照时存在，因此，它是一个静态的、只读的并与其源数据库保持事务一致的快照。 若要在镜像数据库中创建数据库快照，数据库必须处于同步镜像状态。  
  
 与镜像数据库本身不同，客户端可以访问数据库快照。 只要镜像服务器与主体服务器进行通信，就可以将报表客户端连接定向到快照。 注意，由于数据库快照是静态的，因此没有新数据可用。 为了让用户能够使用相对较新的数据，必须定期创建新的数据库快照，并通过应用程序将传入客户端连接定向到最新的快照。  
  
 新的数据库快照几乎是空的，但是它会随着越来越多的数据页的首次更新而增长。 由于数据库中的每个快照都以这种方式增长，因此，每个数据库快照与常规数据库使用同样多的资源。 根据镜像服务器和主体服务器的配置，在镜像数据库中保留过多的数据库快照可能会降低主体数据库的性能。 因此，我们建议在镜像数据库中仅保留少量相对较新的快照。 一般情况下，在创建替换快照之后，应重新将传入查询定向到新的快照，并在完成所有当前的查询之后删除较早的快照。  
  
> [!NOTE]  
>  有关数据库快照的详细信息，请参阅 [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)。  
  
 如果出现角色切换，则数据库及其快照将重新启动并暂时断开与用户的连接。 然后，数据库快照保留在创建时所在的服务器实例中，并成为新的主体数据库。 用户可以在故障转移后继续使用快照。 但是，这样会给新的主体服务器带来额外的负荷。 如果在您的环境中性能是一个关注点，则我们建议您应在新镜像数据库成为可用后，在其中创建快照，并将客户端重新定向到新快照，同时从以前的镜像数据库中删除所有数据库快照。  
  
> [!NOTE]  
>  对于能很好地向外扩展的专用报告解决方案，可以考虑复制。 有关详细信息，请参阅 [SQL Server Replication](../../relational-databases/replication/sql-server-replication.md)。  
  
## <a name="example"></a>示例  
 下面的示例将对镜像数据库创建快照。  
  
 假定数据库镜像会话的数据库为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]。 此示例在 `AdventureWorks` 数据库（位于驱动器 `F` 中）的镜像副本中创建了三个数据库快照。 分别将这些快照命名为 `AdventureWorks_0600`、 `AdventureWorks_1200`和 `AdventureWorks_1800` ，以标识它们的近似创建次数。  
  
1.  在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]的镜像中创建第一个数据库快照。  
  
    ```  
    CREATE DATABASE AdventureWorks_0600  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_0600.SNP')  
       AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
2.  在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]的镜像中创建第二个数据库快照。 仍在使用 `AdventureWorks_0600` 的用户可以继续使用它。  
  
    ```  
    CREATE DATABASE AdventureWorks_1200  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_1200.SNP')  
       AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
     此时，可以用编程的方式将新客户端连接定向至最新的快照。  
  
3.  在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]的镜像中创建第三个快照。 仍在使用 `AdventureWorks_0600` 或 `AdventureWorks_1200` 的用户可以继续使用它们。  
  
    ```  
    CREATE DATABASE AdventureWorks_1800  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_1800.SNP')  
        AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
     此时，可以用编程的方式将新客户端连接定向至最新的快照。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建数据库快照 (Transact-SQL)](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
-   [查看数据库快照 (SQL Server)](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [删除数据库快照 (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
  
## <a name="see-also"></a>另请参阅  
 [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)   
 [将客户端连接到数据库镜像会话 (SQL Server)](../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)  
  
  
