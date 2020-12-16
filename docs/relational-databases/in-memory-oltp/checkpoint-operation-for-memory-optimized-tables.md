---
title: 内存优化表的检查点操作 | Microsoft Docs
description: 了解 SQL Server 中内存优化表的检查点。 内存优化表检查点操作与基于磁盘的表不同。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47975bd5-373f-43cd-946a-da8e8088b610
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8ea64147c8a7bf9f6142b36cb048d7ad39f43b66
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465388"
---
# <a name="checkpoint-operation-for-memory-optimized-tables"></a>内存优化表的检查点操作
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  数据和差异文件中内存优化的数据需要定期出现检查点以前移事务日志的活动部分。 通过检查点，内存优化表可还原或恢复到上一个成功的检查点，然后应用事务日志的活动部分以更新内存优化表从而完成恢复。 针对基于磁盘的表和内存优化表的检查点操作是完全不同的操作。 下面介绍不同的场景以及基于磁盘的表和内存优化表的检查点行为：  
  
## <a name="manual-checkpoint"></a>手动检查点  
 发出手动检查点时，它关闭基于磁盘的表和内存优化表的检查点。 即使仅填充了活动数据文件的一部分，也将关闭该文件。  
  
## <a name="automatic-checkpoint"></a>自动检查点  
 对于基于磁盘的表和内存优化表而言，由于保留数据的方式不同，因此自动检查点的实现方式是不同的。  
  
 对于基于磁盘的表，将基于恢复间隔配置选项执行自动检查点（有关详细信息，请参阅[更改数据库的目标恢复时间 (SQL Server)](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)）。  
  
 对于内存优化表，在上次检查点后事务日志文件变得大于 1.5 GB 时，执行自动检查点。 此 1.5 GB 大小包括基于磁盘的表和内存优化表的事务日志记录。  
  
## <a name="see-also"></a>另请参阅  
 [创建和管理用于内存优化对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
