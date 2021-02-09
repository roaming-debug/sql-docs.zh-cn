---
title: 弃用的数据库引擎功能
description: 了解 SQL Server 2019 (15.x)、SQL Server 2016 (13.x) 和先前版本中弃用的数据库引擎功能。
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- VIA protocol
- unsupported features [SQL Server]
- SQL Mail
- discontinued functionality [SQL Server]
- RESTORE WITH DBO_ONLY
- BACKUP WITH PASSWORD
- user instances enabled
- BACKUP WITH MEDIAPASSWORD
- AWE
- SQL-DMO
- '*= and =*'
- 80 compatibility levels
- COMPUTE BY
- user instance timeout
- sp_dropalias
- COMPUTE
- SSL
- WITH APPEND
- sys.database_principal_aliases
- sp_dboption
- DATABASEPROPERTY
- FASTFIRSTROW hint
- SET DISABLE_DEF_CNST_CHK
ms.assetid: d686cdf0-d11d-4dba-9ec8-de1a5f189f25
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-linux-2017  || >= sql-server-2016'
ms.openlocfilehash: 093e20773376cddeea566e202d9f18605f1a1c13
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552589"
---
# <a name="discontinued-database-engine-functionality-in-sql-server"></a>SQL Server中弃用的数据库引擎功能
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  本主题介绍 [!INCLUDE[ssDE](../includes/ssde-md.md)] 中不再可用的 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]功能。  

## <a name="discontinued-features-in-sssql19"></a>[!INCLUDE[sssql19](../includes/sssql19-md.md)] 中弃用的功能  

- 以下数据库范围的配置选项已停止使用：

  - `DISABLE_BATCH_MODE_ADAPTIVE_JOIN`
  - `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK`
  - `DISABLE_INTERLEAVED_EXECUTION_TVF`

有关当前配置选项，请参阅[更改数据库范围配置 (Transact-SQL)](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。

>[!NOTE]
>[!INCLUDE[sssql14](../includes/sssql17-md.md)] 中没有弃用的功能。

## <a name="discontinued-features-in-sssql15-md"></a>[!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 中弃用的功能

- [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] 是 64 位应用程序。 32 位安装已停止使用，不过，某些元素仍以 32 位组件运行。  

- 已不再使用兼容级别 90。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  

- ActiveX 子系统已停止使用。 请改用命令行或 PowerShell 脚本。

- 启动参数 -h  和 -g  。 有关详细信息，请参阅 [Database Engine Service Startup Options](/previous-versions/sql/2014/database-engine/configure-windows/database-engine-service-startup-options?view=sql-server-2014&preserve-view=true)。

- 安全套接字层 (SSL) 加密已中断。 请改用传输层安全性 (TLS)。 有关详细信息，请参阅[启用数据库引擎的加密连接](../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。

## <a name="previous-versions"></a>以前的版本

- [SQL Server 2014 中废止的数据库引擎功能](/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014&preserve-view=true)

### <a name="see-also"></a>另请参阅

- [SQL Server 2019 中弃用的数据库引擎功能](deprecated-database-engine-features-in-sql-server-version-15.md)
- [SQL Server 2017 中弃用的数据库引擎功能](deprecated-database-engine-features-in-sql-server-2017.md)
- [SQL Server 2016 中不推荐使用的数据库引擎功能](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)
- [SQL Server 2019 中数据库引擎功能的重大更改](breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [SQL Server 2017 中数据库引擎功能的重大更改](breaking-changes-to-database-engine-features-in-sql-server-2017.md)
- [SQL Server 2016 中数据库引擎功能的重大更改](breaking-changes-to-database-engine-features-in-sql-server-2016.md)
- [SQL Server 复制中不推荐使用的功能](../relational-databases/replication/deprecated-features-in-sql-server-replication.md)