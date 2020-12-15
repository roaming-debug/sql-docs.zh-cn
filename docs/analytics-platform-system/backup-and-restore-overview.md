---
title: 备份和还原
description: 介绍数据备份和还原如何适用于并行数据仓库 (PDW) 。 备份和还原操作用于灾难恢复。 还可以使用备份和还原将数据库从一个设备复制到另一台设备。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 01/19/2019
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 9dd52db9d34519f2b09cbaba880806c17509c84c
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489707"
---
# <a name="backup-and-restore"></a>备份和还原

介绍数据备份和还原如何适用于并行数据仓库 (PDW) 。 备份和还原操作用于灾难恢复。 还可以使用备份和还原将数据库从一个设备复制到另一台设备。  
    
## <a name="backup-and-restore-basics"></a><a name="BackupRestoreBasics"></a>备份和还原基础知识

PDW *数据库备份* 是设备数据库的副本，以格式存储，以便可以使用它将原始数据库还原到设备。  
  
将使用 [BACKUP database](../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true) t-sql 语句创建 PDW 数据库备份，并将其设置为与 [RESTORE database](../t-sql/statements/restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true) 语句一起使用。它不可用于任何其他目的。 只能将备份还原到具有相同数量或更多计算节点的设备上。  
  
<!-- MISSING LINKS
The [master database](master-database.md) is a SMP SQL Server database. It is backed up with the BACKUP DATABASE statement. To restore master, use the [Restore the Master Database](configuration-manager-restore-master-database.md) page of the Configuration Manager tool.  
-->
  
PDW 使用 SQL Server 备份技术来备份和还原设备数据库。 SQL Server 备份选项预先配置为使用备份压缩。 不能设置压缩、校验和、块大小和缓冲区计数等备份选项。  
  
数据库备份存储在一个或多个备份服务器上，这些服务器位于你自己的客户网络中。  PDW 直接将用户数据库备份从计算节点直接写入到一个备份服务器，并将用户数据库备份直接从备份服务器直接恢复到计算节点。  
  
备份在备份服务器上存储为 Windows 文件系统中的一组文件。 PDW 数据库备份只能还原到 PDW。 但是，可以使用标准的 Windows 文件备份过程将数据库备份从备份服务器存档到另一个位置。 有关备份服务器的详细信息，请参阅 [获取和配置备份服务器](acquire-and-configure-backup-server.md)。  
  
## <a name="database-backup-types"></a><a name="BackupTypes"></a>数据库备份类型

有两种类型的数据需要备份：用户数据库和系统数据库 (例如 master 数据库) 。 PDW 不会备份事务日志。  
  
完整数据库备份是整个 PDW 数据库的备份。 这是默认的备份类型。 用户数据库的完整备份包括数据库用户和数据库角色。 Master 的备份包括登录名。  
  
差异备份包含自上次完整备份后的所有更改。 差异备份所需的时间通常比完整备份所需的时间少，并且可以更频繁地执行。 如果多个差异备份基于相同的完整备份，则每个差异备份都包含上一个差异中的所有更改。  
  
例如，您可以每周创建一次完整备份，并每天创建一次差异备份。 若要还原用户数据库，完整备份和最后一个差异 (（如果存在) 需要还原）。  
  
仅用户数据库支持差异备份。 Master 的备份始终是完整备份。  
  
若要备份整个设备，需要对所有用户数据库和 master 数据库的备份进行备份。  
  
## <a name="database-backup-process"></a><a name="BackupProc"></a>数据库备份过程

下图显示了数据库备份过程中的数据流。  
  
![PDW 备份过程](media/backup-process.png "PDW 备份过程")  
  
备份过程的工作方式如下：  
  
1.  用户向控制节点提交 BACKUP DATABASE tsql 语句。  
  
    -   备份是完整备份或差异备份。  
  
2.  对于用户数据库，"控制" 节点 (MPP 引擎) 创建分布式查询计划以执行并行数据库备份。  
  
3.  备份中涉及的每个节点使用 SQL Server 备份功能将其备份文件复制到备份服务器。  
  
    -   涉及的每个节点都将一个备份文件复制到备份服务器。  
  
    -   用户数据库备份 (full 或微分) 包含存储在每个计算节点上的数据库部分的备份，以及数据库用户和数据库角色的备份。  
  
4.  设备使用未使用的网络并行执行备份。  
  
    -   PDW 并行执行每个完整备份和差异备份。 但是，多个数据库备份不会同时运行。 每个备份请求都必须等待之前提交的备份完成。  
  
    -   Master 数据库的备份仅备份来自控制节点的数据。 此备份类型按顺序执行。  
  
5.  PDW 数据库备份是存储在设备之外的目录中的一组文件。 目录名称指定为网络路径和目录名称。 目录不能是本地路径，也不能在设备上。  
  
6.  备份完成后，可以根据需要使用 Windows 文件系统将备份目录复制到其他位置。  
  
    -   只能将备份还原到具有相等或更多计算节点的 PDW 设备。  
  
    -   在执行还原之前，无法更改备份的名称。 备份目录的名称必须与该备份的原始名称的名称匹配。 备份的原始名称位于备份目录内的 backup.xml 文件中。 若要将数据库还原为其他名称，可以在 restore 命令中指定新名称。 例如：`RESTORE DATABASE MyDB1 FROM DISK = ꞌ\\10.192.10.10\backups\MyDB2ꞌ`。  
  
## <a name="database-restore-modes"></a><a name="RestoreModes"></a>数据库还原模式

完整数据库还原使用数据库备份中的数据重新创建 PDW 数据库。 数据库还原是通过首先还原完整备份来执行的，并可以选择还原一个差异备份。 数据库还原包括数据库用户和数据库角色。  
  
仅标头还原返回数据库的标头信息。 它不会将数据还原到设备。  
  
设备还原是整个设备的还原。 这包括还原所有用户数据库和 master 数据库。  
  
## <a name="restore-process"></a><a name="RestoreProc"></a>还原过程

下图显示了数据库还原过程中的数据流。  
  
![还原过程](media/restore-process.png "还原过程")  
  
## <a name="restoring-to-an-appliance-with-the-same-number-of-compute-nodes"></a>还原到具有相同数量的计算节点的设备上 * *  
  
还原数据时，设备会检测源设备和目标设备上的计算节点数。 如果两个设备的计算节点数相等，则还原过程的工作方式如下：  
  
1.  要还原的数据库备份在非设备备份服务器上的 Windows 文件共享上可用。 为了获得最佳性能，此服务器已连接到设备 "无线网络"。  
  
2.  用户向控件节点提交 [RESTORE DATABASE](../t-sql/statements/restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true) tsql 语句。  
  
    -   还原是完全还原或标头还原。 完全还原将还原完整备份，并根据需要还原差异备份。  
  
3.  控制节点 (MPP 引擎) 创建分布式查询计划以执行并行数据库还原。  
  
    -   SQL ServerPDW 以并行方式执行用户数据库的还原。 但是，不会同时运行多个数据库备份和还原。 MPP 引擎将每个 restore 语句放入队列中;它必须等待之前提交的备份和还原请求完成。  
  
    -   还原 master 数据库只会将数据还原到控制节点;还原是串行执行的。  
  
    -   还原标头信息是一个快速操作，并且不会将任何数据还原到计算节点或控制节点。 相反，控件节点以查询输出的形式返回结果。  
  
4.  备份文件会并行复制到正确的计算节点，这通常是通过设备的 "无线网络"。  
  
5.  每个计算节点都还原其用户数据库的部分。 如果任何还原未成功完成，则所有数据库都将被删除，并且还原无法成功完成。  
  
## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>还原到计算节点数更大的设备  
  
将备份还原到计算节点数更大的设备会与计算节点数成比例地增加分配的数据库大小。  
  
例如，在从双节点设备中还原 60 GB 数据库时，每个节点 (30 GB) 到6节点设备，SQL Server PDW 将在6节点的设备上创建一个 180 gb 的数据库 (6 个节点，每个节点) 30 GB。 SQL Server PDW 最初将数据库还原到2个节点以匹配源配置，然后将数据重新分配给所有6个节点。  
  
重新分发后，每个计算节点将包含的实际数据量更少，并且比小型源设备上的每个计算节点的可用空间更多。 使用附加空间可将更多数据添加到数据库。 如果还原的数据库大小大于所需的大小，则可以使用 [ALTER database](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) 来缩减数据库文件的大小。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|备份和还原任务|说明|  
|---------------------------|---------------|  
|准备服务器作为备份服务器。|[获取和配置备份服务器](acquire-and-configure-backup-server.md)|  
|备份数据库。|[BACKUP DATABASE](../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)|  
|还原数据库。|[还原数据库](../t-sql/statements/restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)|    

<!-- MISSING LINKS

|Create a disaster recovery plan.|[Create a Disaster Recovery Plan](create-disaster-recovery-plan.md)|
|Restore the master database.|To restore the master database, use the [Restore the master database](configuration-manager-restore-master-database.md) page in the Configuration Manager tool.| 
|Copy a database from one appliance to another appliance.|[Copy a PDW database to another appliance](copy-pdw-database-to-another-appliance.md).|  
|Monitor backups and restores.|[Monitor backups and restores](monitor-backup-and-restore.md)|  

-->
