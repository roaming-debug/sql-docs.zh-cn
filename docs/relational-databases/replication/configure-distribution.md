---
title: 配置分发 | Microsoft Docs
description: SQL Server 分发服务器托管分发数据库，其中存储复制的元数据和历史记录以及事务复制的事务。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- remote Distributors [SQL Server replication]
- transactional replication, configuring distribution
- distribution databases [SQL Server replication], sizing
- Distributors [SQL Server replication], configuring
- distribution databases [SQL Server replication], about distribution databases
- distribution databases [SQL Server replication]
- merge replication [SQL Server replication], configuring distribution
ms.assetid: 94d52169-384e-4885-84eb-2304e967d9f7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c6b01d713b7f13b5949c759b1e36f02d9f9726e0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477888"
---
# <a name="configure-distribution"></a>配置分发
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  分发服务器是指包含分发数据库的服务器，其中存储所有类型复制的元数据和历史记录数据以及事务复制的事务。 若要建立复制，必须配置分发服务器。 只能为每台发布服务器分配一个分发服务器实例，但是多台发布服务器可共享一台分发服务器。 分发服务器在其所在服务器上使用以下附加资源：  
  
-   额外的磁盘空间，如果发布的快照文件存储在分发服务器上（通常如此）。  
  
-   存储分发数据库所需的附加磁盘空间。  
  
-   运行于分发服务器上的复制代理执行推送订阅所需的附加处理器资源。  
  
 选作分发服务器的服务器应有足够的磁盘空间和处理器运算能力，以支持该服务器上的复制和任何其他活动。 配置分发服务器时，需指定下列内容：  
  
-   快照文件夹，默认情况下供使用此分发服务器的所有发布服务器使用。 请确保此文件夹已共享并设置了适当的权限。 有关详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
-   分发数据库的名称和文件位置。 分发数据库创建后不能重命名。 若要对数据库使用其他名称，必须禁用并重新配置分发。  
  
-   获得授权使用分发服务器的发布服务器。 如果要指定分发服务器而非发布服务器运行实例，还必须指定发布服务器连接到远程分发服务器所用密码。  
  
 对于事务复制，配置分发后，建议您：  
  
-   适当调整分发数据库的大小。 在系统承受典型负载的情况下对复制进行测试，以确定存储命令需要多少空间。 确保数据库足以存储命令，而无需频繁地自动增大。 有关更改数据库大小的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
-   针对分发数据库设置 **sync with backup** 选项。 有关详细信息，请参阅[快照复制和事务复制的备份和还原策略](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md)和[为事务复制启用协调备份（复制 Transact-SQL 编程）](../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md)。  
  
## <a name="local-and-remote-distributors"></a>本地分发服务器和远程分发服务器  
 默认情况下，分发服务器与发布服务器是同一台服务器（本地分发服务器），但也可以是与发布服务器不同的服务器（远程分发服务器）。 通常，在下列情况下，要选用远程分发服务器：  
  
-   希望复制对发布服务器的影响降低到最小（例如，如果发布服务器是一台 OLTP 服务器），将处理卸载到另一台计算机上。  
  
-   为多台发布服务器配置一台集中的分发服务器。  
  
 与合并复制相比，远程分发服务器较常用于事务复制中，原因有二：  
  
-   分发服务器在事务复制中发挥着更大的作用，因为所有复制的事务都要写入分发数据库中并从中读取。  
  
-   合并复制拓扑通常使用请求订阅，因此代理在每台订阅服务器上运行，而不是所有代理都在分发服务器上运行。 有关详细信息，请参阅[订阅发布](../../relational-databases/replication/subscribe-to-publications.md)。 在大多数情况下，应为将本地分发服务器用于合并复制。  
  
 若要配置发布和分发，请参阅 [Configure Publishing and Distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)。  
  
 若要修改发布服务器和分发服务器属性，请参阅 [View and Modify Distributor and Publisher Properties](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [保护分发服务器](../../relational-databases/replication/security/secure-the-distributor.md)  
  
  
