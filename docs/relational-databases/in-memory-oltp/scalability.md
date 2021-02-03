---
title: 可伸缩性 | Microsoft Docs
description: 了解 SQL Server 中内存优化表的磁盘上存储的可伸缩性增强功能，例如使用多个线程来保留表。
ms.custom: ''
ms.date: 08/27/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a4891c57-56bb-49f4-9bb5-f11b745279e5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b3f4141afeb26c7be406a3c810f2403978f016f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236128"
---
# <a name="scalability"></a>可伸缩性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 包含内存优化表磁盘存储的可伸缩性增强功能。 

## <a name="multiple-threads-to-persist-memory-optimized-tables"></a>要保留内存优化表的多个线程  
  
[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中曾经存在单个脱机检查点线程以扫描内存优化表更改的事务日志并将其保存在检查点文件（如数据和差异文件）中。 在具有较大内核数的计算机中，单个脱机检查点线程可能落后。  
  
从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，有多个并发线程负责保存内存优化表的更改。  
  
## <a name="multi-threaded-recovery"></a>多线程恢复
在以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，日志应用属于恢复操作的一部分，是单个线程。 从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，日志应用是多线程。  
  
## <a name="merge-operation"></a>合并操作  
合并操作现在是多线程。  
   
> [!NOTE]
> 手动合并已禁用，因为多线程合并需要与负荷保持一致。 

## <a name="dynamic-management-views"></a>动态管理视图  
DMV [sys.dm_db_xtp_checkpoint_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-stats-transact-sql.md) 和 [sys.dm_db_xtp_checkpoint_files (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) 发生了显著变化。  

## <a name="storage-management"></a>存储管理
内存中 OLTP 引擎将基于文件流继续使用内存优化文件组，但该文件组中的单个文件脱离文件流。 这些文件完全由内存中 OLTP 引擎管理（例如对于创建、删除和垃圾回收）。 

> [!NOTE]
> 不支持 [DBCC SHRINKFILE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅   
[创建和管理用于内存优化对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)     
[Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)    
[ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)    
