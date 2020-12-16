---
title: 为内存优化对象定义持续性 | Microsoft Docs
description: 了解 SQL Server 中的内存优化表的持续性选项：SCHEMA_AND_DATA（默认）和 SCHEMA_ONLY。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 0fe85fbf-8e8d-4983-96fd-d04b3c7d6d65
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 299695e0602b41623550a90e84aca9d6e6f633a2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481228"
---
# <a name="defining-durability-for-memory-optimized-objects"></a>为内存优化对象定义持续性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  有两个用于内存优化表的持续性选项：  
  
 SCHEMA_AND_DATA（默认值）  
 此选项提供架构和数据的持续性。 数据持续性的级别取决于提交事务时其作为完全持久还是延迟持续性。 完全持久事务对数据和架构提供相同的持续性保证，与基于磁盘的表类似。 延迟持续性将提高性能，但如果服务器崩溃或进行故障转移，则可能会丢失数据。 （有关延迟持续性的详细信息，请参阅 [控制事务持续性](../../relational-databases/logs/control-transaction-durability.md)。）  
  
 SCHEMA_ONLY  
 此选项可确保表架构的持续性。 当重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库中发生重新配置时，表架构仍然存在，但将丢失表中的数据。 （这与 tempdb 中的表不同；对于 tempdb 中的表，表及其数据都会在重新启动时丢失。）创建非持久表的典型情况是用于存储瞬时数据（如用于 ETL 进程的临时表）。 SCHEMA_ONLY 持续性可避免事务日志记录和检查点，这样可大幅减少 I/O 操作数。  
  
 使用默认 SCHEMA_AND_DATA 表时，对于基于磁盘的表， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供相同的持续性保证：  
  
 事务持续性  
 提交对内存优化表作出（DDL 或 DML）更改的完全持久事务时，对持久内存优化表作出的更改为永久更改。  
  
 向内存优化表提交延迟的持久事务时，仅在将内存中事务日志保存到磁盘后，该事务才成为持久事务。 （有关延迟持续性的详细信息，请参阅 [控制事务持续性](../../relational-databases/logs/control-transaction-durability.md)。）  
  
 重新启动持续性  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在崩溃或计划关闭后重新启动时，将对内存优化的持久表重新实例化，使其恢复为关闭或崩溃之前的状态。  
  
 介质故障持续性  
 如果发生故障或损坏的磁盘包含持久内存优化的对象的一个或多个持久化副本，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份和还原功能将内存优化表还原到新介质上。  
  
## <a name="see-also"></a>另请参阅  
 [创建和管理用于内存优化对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
