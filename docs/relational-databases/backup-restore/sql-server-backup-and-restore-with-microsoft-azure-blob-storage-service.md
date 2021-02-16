---
description: 使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原
title: 使用 Azure blob 存储备份和还原
storage: Learn about SQL Server backup to and restore from Azure Blob storage, including the benefits of using Azure Blob storage to store SQL Server backups.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 6a0c9b6a-cf71-4311-82f2-12c445f63935
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2fb47efd89dfee8777d54b4cf2d1cf0ad2accec9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343649"
---
# <a name="sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service"></a>使用 Microsoft Azure Blob 存储服务进行 SQL Server 备份和还原
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  ![备份到 Azure blob 图形](../../relational-databases/backup-restore/media/backup-to-azure-blob-graphic.png "备份到 Azure blob 图形")  
  
 本主题介绍将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份到 [Microsoft Azure Blob 存储服务](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/)和从中还原。 它还总结了使用 Microsoft Azure Blob 服务存储 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份的好处。  
  
 SQL Server 支持通过以下方式将备份存储到 Microsoft Azure Blob 存储服务：  
  
-   **管理向 Microsoft Azure 进行的备份：** 使用与用于备份到磁盘和磁带相同的方法，现在可通过指定 URL 作为备份目标，备份到 Microsoft Azure 存储。 可使用此功能手动备份或配置自己的备份策略，如同对于本地存储或其他站点外选项所做的一样。 此功能也称为 **SQL Server 备份到 URL**。 有关详细信息，请参阅 [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)。 SQL Server 2012 SP1 CU2 或更高版本中提供此功能。 此功能在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 中得到了增强，以便通过使用块 Blob、共享访问签名和条带化提高性能和改进功能。  
  
    > [!NOTE]  
    >  对于版本低于 SQL Server 2012 SP1 CU2 的 SQL Server，可使用外接程序“SQL Server 备份到 Microsoft Azure 工具”快速轻松地向 Microsoft Azure 存储创建备份。 有关详细信息，请参阅 [下载中心](https://go.microsoft.com/fwlink/?LinkID=324399)。  
  
-   **Azure Blob 存储中的数据库文件的文件快照备份** 通过使用 Azure 快照， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件快照备份可以通过使用 Azure Blob 存储服务为存储的数据库文件提供几乎即时的备份和还原。 此功能可以简化备份和还原策略，而且它还支持时间点还原。 有关详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。 SQL Server 2016 或更高版本中提供此功能。  
  
-   **让 SQL Server 管理向 Microsoft Azure 进行的备份：** 配置 SQL Server 以管理备份策略，并为一个数据库或多个数据库安排备份，或在实例级别设置默认值。 此功能被称为 **[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]** 。 有关详细信息，请参阅 [Microsoft Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)。 SQL Server 2014 或更高版本中提供此功能。  
  
## <a name="benefits-of-using-the-microsoft-azure-blob-service-for-ssnoversion-backups"></a>将 Microsoft Azure Blob 服务用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份的好处  
  
-   灵活、可靠、无限制的站点外存储：在 Microsoft Azure Blob 服务上存储备份是一种方便、灵活、易于访问的站点外备选方法。 为您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份创建站点外存储就像修改您的现有脚本/作业一样简单。 场外存储通常应当远离生产数据库位置，以防止某个灾难可能同时影响场外和生产数据库位置。 通过选择地理复制 Blob 存储区，您在发生可能影响整个区域的灾难时多了一层额外的保护。 此外，备份副本随时随地可用，并可以轻松访问它们来执行还原。  
  
    > [!IMPORTANT]  
    >  在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]中使用块 blob 可以条带化备份集，支持对大小高达 12.8 TB 的文件进行备份。  
  
-   备份存档：Microsoft Azure Blob 存储服务提供一种更好地替代常用磁带方案的方法来存档备份。 选择磁带存储时可能需要将数据实际运输到场外设施，并且需要采取一些介质保护措施。 在 Microsoft Azure Blob 存储区中存储你的备份可以提供一个即时、高度可用、耐久的存档方案。  
  
-   无硬件管理开销：没有有关 Microsoft Azure 服务的硬件管理开销。 Microsoft Azure 服务管理硬件并支持地理复制以提供冗余和防止硬件故障。  
  
-   当前对于在 Microsoft Azure 虚拟机中运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，可以通过创建附加的磁盘来备份到 Microsoft Azure Blob 存储服务。 但是，对于可以附加到 Microsoft Azure 虚拟机的磁盘数有限制。 对特大实例的限制为 16 个磁盘；对较小实例的磁盘限制数更少。 通过允许直接备份到 Microsoft Azure Blob 存储区，你可以绕过 16 个磁盘的限制。  
  
     此外，目前存储在 Microsoft Azure Blob 存储服务中的备份文件直接可用于本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或在 Microsoft Azure 虚拟机中运行的其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，而无需进行数据库附加/分离或下载并附加 VHD。  
  
-   成本优势：仅对使用的服务付费。 作为场外和备份存档方式可能更加划算。 有关详细信息和链接，请参阅 [Microsoft Azure 计费注意事项](#Billing) 一节。  
  
##  <a name="microsoft-azure-billing-considerations"></a><a name="Billing"></a> Microsoft Azure 计费注意事项：  
 了解 Microsoft Azure 存储成本使你能够预测在 Microsoft Azure 中创建和存储备份的成本。  
  
 [Microsoft Azure 价格计算器](https://go.microsoft.com/fwlink/?LinkId=277060) 可以帮助估算你的成本。  
  
 **存储：** 费用基于使用的空间并根据渐变的标准和冗余级别来计算它。 有关详细信息和最新信息，请参阅 **定价详细信息** 文章中的 [“数据管理”](https://go.microsoft.com/fwlink/?LinkId=277059) 一节。  
  
 **数据传输：** 传输到 Microsoft Azure 的入站数据是免费的。 出站传输要支付带宽使用费用，并根据渐变的区域特定标准来计算费用。 有关详细信息，请参阅“定价详细信息”文章中的 [数据传输](https://go.microsoft.com/fwlink/?LinkId=277061) 一节。  
  
## <a name="see-also"></a>另请参阅  

[从 SQL Server 备份到 URL 的最佳做法和故障排除](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)   

[备份和还原系统数据库 (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   

[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)

[SQL Server 备份到 URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)  
  
  
