---
title: 迁移托管备份设置
description: 本主题介绍在从 SQL Server 2014 升级到 SQL Server 2016 时，目标为 Microsoft Azure 的 SQL Server 托管备份的迁移注意事项。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: ae937ebb-24ff-4a33-be3c-8f85328dfc75
author: cawrites
ms.author: chadam
ms.openlocfilehash: 03a496f72b42b8367d546712e1e7d3c7f7c35fe9
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130368"
---
# <a name="migrate-managed-backup-settings"></a>迁移托管备份设置
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍从 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 升级到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 时 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]的迁移注意事项。  
  
 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的过程和基础行为在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]中已更改。 以下部分描述了功能更改及其意义。  
  
## <a name="overview"></a>概述  
 下表描述了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 与 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]之间的一些主要功能差异。  
  
|区域|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|  
|----------|---------------------------|---------------------------|  
|**命名空间:**|smart_admin|managed_backup|  
|**系统存储过程：**|sp_set_db_backup<br /><br /> sp_set_instance_backup|[managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)<br /><br /> [sp_backup_config_advanced](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)<br /><br /> [sp_backup_config_schedule](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)|  
|**安全性：**|使用 Microsoft Azure 存储帐户和访问密钥的 SQL 凭据。|使用 Microsoft Azure 共享访问签名 (SAS) 令牌的 SQL 凭据。|  
|**基础存储：**|使用页 blob 的 Microsoft Azure 存储。|使用块 blob 的 Microsoft Azure 存储。|  
  
## <a name="benefits"></a>优点  
 使用 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]中的新功能具有多个好处。  
  
-   块 blob 的存储成本较少。  
  
-   借助条带化，你可以存储备份的空间大很多（12 TB 与页 blob 的 1 TB）。  
  
-   条带化还会缩短大型数据库的恢复时间  
  
-   有关对 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 中 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]的其他改进，请参阅 [Microsoft Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)。  
  
## <a name="considerations"></a>注意事项  
 从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]升级之后，请注意以下 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 注意事项：  
  
-   以前在 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 上为 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 配置的任何数据库将继续在 **上使用** smart_admin [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]系统过程和基础行为。  
  
-   在 **上，** 的任何新配置均不支持 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] smart_admin [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]过程。 必须使用新的 **managed_backup** 过程和功能。  
  
## <a name="see-also"></a>另请参阅  
 [目标为 Microsoft Azure 的 SQL Server 托管备份](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
