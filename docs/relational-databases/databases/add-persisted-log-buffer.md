---
description: 向数据库添加永久日志缓冲区
title: 向数据库添加永久日志缓冲区
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- PMEM
- persistent memory
- persisted log buffer
- add log file
- create log buffer
- remove log buffer
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: 63425d2a96cffb585a5b36c0614ee06b1586bbf8
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237991"
---
# <a name="add-persisted-log-buffer-to-a-database"></a>向数据库添加永久日志缓冲区
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 将永久日志缓冲区添加到 [!INCLUDE[sqlv15](../../includes/sssql19-md.md)] 中的数据库。  
  
## <a name="permissions"></a>权限

需要对数据库拥有 ALTER 权限。  

## <a name="configure-persistent-memory-device-linux"></a>配置永久内存设备 (Linux)

在 [Linux](../../linux/sql-server-linux-configure-pmem.md) 中配置永久内存设备。

## <a name="configure-persistent-memory-device-windows"></a>配置永久内存设备 (Windows)

在 [Windows](/windows-server/storage/storage-spaces/deploy-pmem/) 中配置永久内存设备。
  
## <a name="add-a-persisted-log-buffer-to-a-database"></a>向数据库添加永久日志缓冲区  

下面的示例添加永久日志缓冲区。

```sql
ALTER DATABASE <MyDB> 
  ADD LOG FILE 
  (
    NAME = <DAXlog>, 
    FILENAME = '<Filepath to DAX Log File>', 
    SIZE = 20MB
  );
```

要放置新日志文件的卷必须使用 DAX (NTFS) 格式，或者必须使用 DAX 选项 (XFS/EXT4) 装载。

## <a name="remove-a-persisted-log-buffer"></a>删除永久日志缓冲区

要安全地删除永久日志缓冲区，必须将该数据库置于单用户模式，才能排空永久日志缓冲区。

下面的示例删除永久日志缓冲区。

```sql
ALTER DATABASE <MyDB> SET SINGLE_USER;
ALTER DATABASE <MyDB> REMOVE FILE <DAXlog>;
ALTER DATABASE <MyDB> SET MULTI_USER;
```

## <a name="limitations"></a>限制

[透明数据加密 (TDE)](../security/encryption/transparent-data-encryption.md) 与永久日志缓冲区不兼容。

[可用性组](../../t-sql/statements/create-availability-group-transact-sql.md)只能在次要副本上使用此功能，因为在主副本上需要使用正常的日志编写语义。 但是，必须在所有节点上（最好在 DAX 卷或装载上）创建小型日志文件。

## <a name="backup-and-restore-operations"></a>备份和还原操作

适用正常还原条件。 如果将永久日志缓冲区还原到 DAX 卷或装载，它将继续正常工作，否则请将其安全删除。
  
## <a name="next-steps"></a>后续步骤

- [工作原理（运行速度更快）：NVDIMM 上的非易失性内存 SQL Server 日志结尾缓存](/archive/blogs/bobsql/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm)
- [公开的数据：SQL Server 2016 的延迟和持续性](https://channel9.msdn.com/Shows/Data-Exposed/Latency-and-Durability-with-SQL-Server-2016)
- [使用 Windows Server 2016/SQL Server 2016 SP1 中的存储类内存的事务提交延迟加速](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)