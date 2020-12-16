---
title: 数据库状态 | Microsoft Docs
description: 了解各种数据库状态，例如 ONLINE、OFFLINE 或 SUSPECT。 了解如何验证数据库的当前状态。
ms.custom: ''
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.DATABASESTATES.F1
helpviewer_keywords:
- emergency database state [SQL Server]
- verifying database states
- viewing database states
- displaying database states
- database states [SQL Server]
- current database state
- offline database state [SQL Server]
- suspect database state
- recovery pending database state [SQL Server]
- states [SQL Server], databases
- online database state
- states [SQL Server]
- restoring database state [SQL Server]
ms.assetid: b7f1f111-ca73-4a89-b567-a98d64d6ecb3
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9039669df4b04463b7fa13305f70d2678a1e4d1f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481448"
---
# <a name="database-states"></a>数据库状态
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  数据库总是处于一个特定的状态中， 例如，这些状态包括 ONLINE、OFFLINE 或 SUSPECT。 若要确认数据库的当前状态，请选择 **sys.databases** 目录视图中的 [state_desc](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 列或 **DATABASEPROPERTYEX** 函数中的 [Status](../../t-sql/functions/databasepropertyex-transact-sql.md) 属性。  
  
## <a name="database-state-definitions"></a>数据库状态定义  
 下表定义了数据库的状态。  
  
|状态|定义|  
|-----------|----------------|  
|ONLINE|可以对数据库进行访问。 即使可能尚未完成恢复的撤消阶段，主文件组仍处于在线状态。|  
|OFFLINE|数据库无法使用。 数据库由于显式的用户操作而处于离线状态，并保持离线状态直至执行了其他的用户操作。 例如，可能会让数据库离线以便将文件移至新的磁盘。 然后，在完成移动操作后，使数据库恢复到在线状态。|  
|RESTORING|正在还原主文件组的一个或多个文件，或正在脱机还原一个或多个辅助文件。 数据库不可用。|  
|RECOVERING|正在恢复数据库。 恢复进程是一个暂时性状态，恢复成功后数据库将自动处于在线状态。 如果恢复失败，数据库将处于可疑状态。 数据库不可用。|  
|RECOVERY PENDING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在恢复期间遇到了与资源相关的错误。 数据库未损坏，但是可能缺少文件，或系统资源限制可能导致无法启动数据库。 数据库不可用。 需要用户另外执行操作来解决问题，并让恢复进程完成。|  
|SUSPECT|至少主文件组可疑或可能已损坏。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]启动过程中无法恢复数据库。 数据库不可用。 需要用户另外执行操作来解决问题。|  
|EMERGENCY|用户更改了数据库，并将其状态设置为 EMERGENCY。 数据库处于单用户模式，可以修复或还原。 数据库标记为 READ_ONLY，禁用日志记录，并仅限 **sysadmin** 固定服务器角色的成员访问。 EMERGENCY 主要用于故障排除。 例如，可以将标记为“可疑”的数据库设置为 EMERGENCY 状态。 这样可以允许系统管理员对数据库进行只读访问。 只有 **sysadmin** 固定服务器角色的成员才可以将数据库设置为 EMERGENCY 状态。|  
  
## <a name="related-content"></a>相关内容  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  
 [镜像状态 (SQL Server)](../../database-engine/database-mirroring/mirroring-states-sql-server.md)  
  
 [文件状态](../../relational-databases/databases/file-states.md)  
  
  
