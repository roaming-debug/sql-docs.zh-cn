---
title: 估计在镜像故障转移期间服务的中断
description: 估计在将数据库镜像从主要角色故障转移到辅助角色时服务的中断。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- parallel redo [SQL Server]
- role switching [SQL Server]
- database mirroring [SQL Server], queues
- failover [SQL Server], database mirroring
- redo [database mirroring]
- database mirroring [SQL Server], failover
ms.assetid: 586a6f25-672b-491b-bc2f-deab2ccda6e2
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4b8f2e5435f6f168a113cf78b1539e97c4935972
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643917"
---
# <a name="estimate-the-interruption-of-service-during-role-switching-database-mirroring"></a>估计在角色切换期间服务的中断（数据库镜像）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在角色切换过程中，数据库镜像功能中断服务的时间取决于角色切换的类型和原因。  
  
-   对于自动故障转移，有两个影响服务中断时间的因素：镜像服务器识别主体服务器实例失败（错误检测）所需的时间，以及数据库故障转移所需的时间（故障转移时间）。  
  
-   对于强制服务操作，尽管已出现失败，但对失败的检测和响应取决于人的响应。 但是，在发出强制服务命令后，估计的潜在服务器中断时间将限定为估计镜像服务器切换角色的时间。  
  
    > [!NOTE]  
    >  若要减少检测特定条件（如某些错误类型）所需的时间，可以针对这些条件定义警报。  
  
-   对于手动故障转移，中断时间仅指在发出故障转移命令后，数据库故障转移所需的时间。  
  
## <a name="error-detection"></a>错误检测  
 系统发现错误的时间取决于错误类型；例如，网络错误几乎可以被立即注意到，而注意到没有响应的服务器需要 10 秒（默认超时）。  
  
 有关可在数据库镜像会话期间导致失败以及在具有自动故障转移功能的高安全性模式下导致超时检测的错误的信息，请参阅 [数据库镜像期间可能出现的故障](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md))。  
  
## <a name="failover-time"></a>故障转移时间  
 故障转移时间主要包括前一个镜像服务器前滚其重做队列中剩余的任意日志所需的时间，以及一小段额外时间（有关镜像服务器如何处理日志记录的详细信息，请参阅[数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。 有关估计故障转移时间的信息，请参阅本主题后面的“估计故障转移重做速度”。  
  
> [!IMPORTANT]  
>  如果在先创建，后更改索引或表的事务中发生故障转移，则故障转移占用的时间可能长于通常所需的时间。  例如，在执行某些操作期间，进行故障转移可能会延长故障转移的时间，这些操作包括：BEGIN TRANSACTION，对表执行的 CREATE INDEX 以及 SELECT INTO。 在通过 COMMIT TRANSACTION 或 ROLLBACK TRANSACTION 语句完成事务之前，该事务中故障转移时间延长的可能性一直存在。  
  
### <a name="the-redo-queue"></a>重做队列  
 前滚数据库涉及应用镜像服务器上的重做队列中当前存在的任何日志记录。 “重做队列”  包括已写入镜像服务器的磁盘上、但尚未在镜像数据库中前滚的日志记录。  
  
 数据库的故障转移时间取决于镜像服务器前滚重做队列中日志的速度，此速度反过来主要由系统硬件和当前的工作负荷决定。 主体数据库可能会非常忙，以至于主体服务器将日志传送到镜像服务器的速度远远大于镜像服务器前滚日志的速度。 在这种情况下，当镜像服务器前滚重做队列中的日志时，故障转移可能占用大量时间。 若要了解重做队列当前的大小，请使用数据库镜像性能对象中的 **Redo Queue** 计数器。 有关详细信息，请参阅 [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md)。  
  
### <a name="estimating-the-failover-redo-rate"></a>估计故障转移重做速度  
 可以使用生产数据库的测试副本测量前滚日志记录所需的时间（“重做速度”）  。  
  
 估计故障转移过程中的前滚时间所用的方法取决于重做阶段中镜像服务器使用的线程数。 线程数取决于以下情况：  
  
-   在 [!INCLUDE[ssStandard](../../includes/ssstandard-md.md)]中，镜像服务器始终使用单个线程来前滚数据库。  
  
-   在 [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)] 中，当作为镜像服务器的计算机使用的 CPU 少于五个时，也只使用单线程。 如果 CPU 的数量为五个或更多时，镜像服务器便会在故障转移过程中在多个线程之间分配其前滚操作（这称为“并行重做”  ）。 并行重做被优化为针对每四个 CPU 使用一个线程。  
  
#### <a name="estimating-the-single-threaded-redo-rate"></a>估计单线程重做速度  
 对于单线程重做，在故障转移过程中，镜像数据库前滚所占用的时间与还原日志备份前滚相同量的日志所占用的时间大致相当。 若要估计故障转移时间，请在您要执行镜像的环境中创建一个测试数据库。 然后从生产数据库进行日志备份。 若要度量该日志备份的重做速度，请记录您将日志备份使用 WITH NORECOVERY 选项还原到测试数据库中所需的时间。  
  
 了解了镜像服务器的重做速度之后，便可使用镜像服务器中当前需要重做的日志量（由 **Redo Queue** 性能计数器测量）除以重做速度，来估计给定时点数据库故障转移的时间。 在正常情况下，如果镜像服务器可以承受主体服务器中的负荷，则 **重做队列** 小于零或接近零，并且故障转移仅会占用几秒钟。  
  
#### <a name="estimating-the-parallel-redo-rate"></a>估计并行重做速度  
 在 [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)]中，并行重做被优化为针对每四个 CPU 使用一个线程。 若要估计并行重做的前滚时间，访问正在运行的测试系统要比访问测试数据库更能得到准确的结果。 当监视镜像服务器中的重做队列时，会增加主体服务器中的负荷。 在正常操作中，重做队列接近于零。 增加主体服务器上的负荷，直到重做队列开始不断增长；系统随后将会达到最大重做速度，而此时的 **Redo Bytes/sec** 性能计数器即表示最大重做速度。 有关详细信息，请参阅 [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md)。  
  
## <a name="estimating-interruption-of-service-during-automatic-failover"></a>估计自动故障转移过程中的服务中断  
 下图阐释了错误检测和故障转移时间如何影响在 **Partner_B** 中完成自动故障转移所需的总时间。 故障转移需要一定的时间前滚数据库（重做阶段），另外还需要一小段时间使数据库联机。 撤消阶段涉及回滚任何未提交的事务，这一阶段发生在新的主体数据库联机之后，并在故障转移后继续。 在撤消阶段，数据库是可用的。  
  
 ![错误检测和故障转移时间](../../database-engine/database-mirroring/media/dbm-failovauto-time.gif "错误检测和故障转移时间")  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像运行模式](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
