---
title: 备份和还原复制的数据库 | Microsoft Docs
description: 查看概述信息，其中还有链接指向有关 SQL Server 中每种复制类型的备份和还原策略的其他信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- backups [SQL Server replication]
- administering replication, restoring
- backing up replicated databases
- backups [SQL Server replication], about backups
- restoring replicated databases [SQL Server replication]
- recovery [SQL Server replication], about recovery
- restoring databases [SQL Server], replicated databases
- backing up databases [SQL Server], replicated databases
- restoring [SQL Server replication], about restoring
- recovery [SQL Server replication]
- replication [SQL Server], administering
- distribution databases [SQL Server replication], backing up
- restoring [SQL Server replication]
- administering replication, backing up
ms.assetid: 04588807-21e7-4bbe-9727-b72f692cffa7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a5b27549630a5ac835e4e9b32f8ebcde5ac6e5d0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469118"
---
# <a name="back-up-and-restore-replicated-databases"></a>备份和还原复制的数据库
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  对于复制的数据库，需要特别注意与备份和还原数据有关的信息。 本主题介绍了适用于各种复制类型的备份和还原策略，并提供了其详细信息的链接。  
  
 复制支持将复制的数据库还原到从中创建备份的同一服务器和数据库。 如果将复制数据库的备份还原到其他服务器或数据库，则无法保留复制设置。 在这种情况下，您必须在还原备份后重新创建所有发布和订阅。  
  
> [!NOTE]  
>  如果正在使用日志传送，则可以将复制数据库还原到备用服务器。 有关详细信息，请参阅[日志传送和复制 (SQL Server)](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)。  
  
 应定期备份复制数据库及其关联系统数据库。 备份下列数据库：  
  
-   发布服务器上的发布数据库  
  
-   分发服务器上的分发数据库  
  
-   各个订阅服务器上的订阅数据库  
  
-   发布服务器、分发服务器和所有订阅服务器上的 **master** 和 **msdb** 系统数据库。 当备份这些数据库中的一个数据库或相关的复制数据库时，应同时备份这些数据库。 例如，应在备份发布数据库的同时备份发布服务器上的 **master** 和 **msdb** 数据库。 如果还原发布数据库，请确保 **master** 和 **msdb** 数据库在复制配置和设置方面与发布数据库保持一致。  
  
 如果执行定期日志备份，则在日志备份中应捕获所有与复制相关的更改。 如果不执行日志备份，则当与复制相关的设置发生更改时，应执行备份。 有关详细信息，请参阅 [Common Actions Requiring an Updated Backup](../../../relational-databases/replication/administration/common-actions-requiring-an-updated-backup.md)。  
  
## <a name="backup-and-restore-strategies"></a>备份和还原策略  
 复制拓扑中每个节点的备份和还原策略都因所用复制类型的不同而有所差异。 有关每种复制的备份和还原策略的信息，请参阅下列主题：  
  
-   [快照复制和事务复制的备份和还原策略](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md)  
  
-   [合并复制的备份和还原策略](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-merge-replication.md)  
  
 作为任何恢复策略的一部分，请始终将当前复制设置的脚本保存在安全的位置。 在服务器出现故障或需要设置测试环境时，可以通过更改服务器名称引用来修改脚本，并用该脚本帮助重新创建复制设置。 除了编写当前复制设置的脚本之外，还应编写启用和禁用复制的脚本。 有关编写复制对象脚本的信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库的备份和还原](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
