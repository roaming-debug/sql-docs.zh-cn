---
title: 配置复制 (SSMS)
description: 了解如何配置 Linux 上的 SQL Server 复制。 使用 SQL Server Management Studio (SSMS) 或 Transact-SQL 复制存储过程配置复制。
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/20/2018
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: bec92d2f03d646d57a02b07cfb7fd97e039d5674
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235929"
---
# <a name="configure-sql-server-replication-on-linux"></a>配置 Linux 上的 SQL Server 复制

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] 介绍 Linux 上的 SQL Server 实例的 SQL Server 复制。

有关复制的详细信息，请参阅 [SQL Server 复制文档](../relational-databases/replication/sql-server-replication.md)。

使用 SQL Server Management Studio (SSMS) 或 Transact-SQL 复制存储过程配置 Linux 上的复制。

* 若要使用 SSMS，请按照本文中的说明进行操作。

  在 Windows 操作系统上使用 SSMS 连接到 SQL Server 实例。 有关背景和说明，请参阅[使用 SSMS 管理 Linux 上的 SQL Server](./sql-server-linux-manage-ssms.md)。
  
* 有关存储过程的示例，请参阅[配置 Linux 上的 SQL Server 复制](sql-server-linux-replication-tutorial-tsql.md)教程。

## <a name="prerequisites"></a>先决条件

在配置发布服务器、分发服务器和订阅服务器之前，需要为 SQL Server 实例完成几个配置步骤。

1. 启用 SQL Server 代理以使用复制代理。 在所有 Linux 服务器上，在终端中运行以下命令。

  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
  sudo systemctl restart mssql-server
  ```

1. 为复制配置 SQL Server 实例。 若要为复制配置 SQL Server 实例，请在参与复制的所有实例上运行 `sys.sp_MSrepl_createdatatypemappings`。

  ```sql
  USE msdb
  GO
  exec sys.sp_MSrepl_createdatatypemappings;
  GO
  ```

1. 创建快照文件夹。 SQL Server 代理需要快照文件夹来读取/写入。 在分发服务器上创建快照文件夹。

  若要创建快照文件夹并向 `mssql` 用户授予访问权限，请运行以下命令：

  ```bash
  sudo mkdir /var/opt/mssql/data/ReplData/
  sudo chown mssql /var/opt/mssql/data/ReplData/
  sudo chgrp mssql /var/opt/mssql/data/ReplData/
  ```

## <a name="configure-and-monitor-replication-with-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 配置和监视复制

### <a name="configure-the-distributor"></a>配置分发服务器
  
配置分发服务器： 

1. 在 SSMS 上，连接到对象资源管理器中的 SQL Server 实例。

1. 右键单击“复制”文件夹，然后单击“配置分发...”。

1. 按照配置分发向导上的说明操作操作。

### <a name="create-publication-and-articles"></a>创建发布和项目

创建发布和项目：

1. 在对象资源管理器中，单击“复制” > “本地发布”> “新建发布...”  。

1. 按照“新建发布”向导上的说明配置复制的类型和属于该发布的项目。

### <a name="configure-the-subscription"></a>配置订阅

若要在对象资源管理器中配置订阅，请单击“复制” > “本地订阅”> “新建订阅...”  。

### <a name="monitor-replication-jobs"></a>监视复制作业

使用复制监视器监视复制作业。

在对象资源管理器中，右键单击“复制”，然后单击“启动复制监视器”。

## <a name="next-steps"></a>后续步骤

[概念：Linux 上的 SQL Server 复制](sql-server-linux-replication.md)

[复制存储过程](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)。
