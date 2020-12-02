---
description: Oracle 发布的术语词汇表
title: 有关 Oracle 发布的术语词汇表 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Oracle publishing [SQL Server replication], glossary
ms.assetid: e21dfa4b-6144-4be7-9cbf-ca2709b2bd9f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2dae5a66a762e53a4748bf43732aed70c5c0edfc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88465130"
---
# <a name="glossary-of-terms-for-oracle-publishing"></a>Oracle 发布的术语词汇表
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  在配置和管理 Oracle 发布时，应熟悉下列 Oracle 术语。 有关 Oracle 术语的完整列表，请参阅 Oracle 联机文档。  
  
#### <a name="index-organized-tables-iot"></a>索引组织表 (IOT)  
 即数据在磁盘上按索引顺序进行物理排序的表。它类似于带有聚集索引的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。 将 IOT 作为带有聚集索引的表复制到订阅服务器。  
  
#### <a name="instance"></a>实例  
 Oracle 数据库是与实例相关联的。 实例由为数据库提供支持的内存和后台进程组成。 Oracle 实例总是映射到单个数据库，而一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例则可包含多个数据库。 有些情况下，Oracle 数据库可以具有多个实例。  
  
#### <a name="oracle-listener"></a>Oracle 侦听器  
 处理 Oracle 数据库实例的传入网络流量。 在配置到 Oracle 数据库的网络连接时，要指定发送通信流量所使用的协议以及侦听器侦听通信流量的端口。 侦听器通常配置成与 Oracle 数据库实例在同一计算机上运行，并且还可以配置成服务于一个或多个实例。  
  
#### <a name="rowid"></a>ROWID  
 指向数据库中特定行位置的指针。 由于使用 ROWID 检索行要比使用表扫描或索引快，所以复制在处理已发布表更改的过程中临时使用 ROWID。  
  
#### <a name="sequence"></a>序列  
 用于生成唯一编号的数据库对象。 复制使用序列来为已发布表的更改排序。  
  
#### <a name="sqlplus"></a>SQL\*Plus  
 用于访问和查询 Oracle 数据库的应用程序。 它类似于  sqlcmd[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
#### <a name="synonym"></a>同义词  
 对象的别名。 配置 Oracle 发布服务器时自动创建特殊公共同义词 **MSSQLSERVERDISTRIBUTOR** 。 该同义词引用 **HREPL_Distributor** 表，并提供一个逻辑指针指向为发布服务器服务的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器。  
  
 在发布 Oracle 数据库之后，再尝试将此发布服务器配置成使用不同 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器就会失败，因为此公共同义词识别的分发服务器已配置为服务于发布服务器。  
  
#### <a name="tablespace"></a>表空间  
 数据库存储单元，大致等同于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的文件组。  
  
#### <a name="tns-service-name"></a>TNS 服务名称  
 TNS（透明网络底层）是 Oracle 数据库使用的通信层。 在网络上使用 TNS 服务名称来辨别 Oracle 数据库实例。 TNS 服务名称是在配置与 Oracle 数据库的连接时分配的。 复制使用 TNS 服务名称来识别发布服务器和建立连接。  
  
#### <a name="user-schema"></a>用户架构  
 可以将用户架构看作拥有特定数据库对象集的用户。 复制管理用户架构拥有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制进程在 Oracle 数据库中创建的所有对象，但 **MSSQLSERVERDISTRIBUTOR** 公共同义词除外。  
  
## <a name="see-also"></a>另请参阅  
 [配置 Oracle 发布服务器](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [在 Oracle 发布服务器上创建的对象](../../../relational-databases/replication/non-sql/objects-created-on-the-oracle-publisher.md)   
 [非 SQL Server 发布服务器](../../../relational-databases/replication/non-sql/non-sql-server-publishers.md)   
 [Oracle 发布概述](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)  
  
  
