---
title: 混合缓冲池 | Microsoft Docs
description: 了解混合缓冲池如何使持久性内存设备可通过内存总线进行访问。 启用或禁用此 SQL Server 2019 功能，查看最佳做法。
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: ''
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ccf508d0415bc6baabc9c054a95ec26cf7989880
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236518"
---
# <a name="hybrid-buffer-pool"></a>混合缓冲池
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

混合缓冲池使缓冲池对象能够引用驻留在持久性内存 (PMEM) 设备上的数据库文件中的数据页，而不是缓存在易失 DRAM 中的数据页的副本。 [!INCLUDE[sqlv15](../../includes/sssql19-md.md)] 中引入了此功能。

![混合缓冲池](./media/hybrid-buffer-pool.png)

持久性内存 (PMEM) 设备支持字节寻址，并且如果使用直接访问 (DAX) 持久性内存感知的文件系统（例如 XFS、EXT4 或 NTFS），则可以使用 OS 中的常用文件系统 API 来访问文件系统上的文件。 或者，它可以对设备上文件的内存映射执行所谓的加载和存储操作。 这允许 PMEM 感知的应用程序（如 SQL Server）访问设备上的文件，而无需遍历传统存储堆栈。

混合缓冲池使用此功能对内存映射的文件执行加载和存储操作，以利用 PMEM 设备作为缓冲池的缓存，以及存储数据库文件。 这就造成了一种独特的情况，即逻辑读取和物理读取本质上是相同的操作。 持久性内存设备可通过内存总线访问，就像常规的易失 DRAM 一样。

混合缓冲池的设备上仅缓存干净数据页。 页面标记为“脏”时，它会被复制到 DRAM 缓冲池，然后它最终被写入 PMEM 设备，并再次标记为干净。 在常规检查点操作过程中，这种情况的发生方式类似于针对标准块设备执行的操作。

混合缓冲池功能同时适用于 Windows 和 Linux。 必须使用支持 DAX (DirectAccess) 的文件系统来格式化 PMEM 设备。 XFS、EXT4 和 NTFS 文件系统都支持 DAX。 当附加、还原或创建新数据库时，SQL Server 将自动检测数据文件是否驻留在格式正确的 PMEM 设备上，并在启动时执行数据库文件的内存映射。

有关详细信息，请参阅：

* [了解和部署持久性内存 (Windows)](/windows-server/storage/storage-spaces/deploy-pmem/)
* [为 Linux 上的 SQL Server 配置持久性内存 (PMEM)](../../linux/sql-server-linux-configure-pmem.md)


## <a name="enable-hybrid-buffer-pool"></a>启用混合缓冲池

[!INCLUDE[sqlv15](../../includes/sssql19-md.md)] 引入了动态数据语言 (DDL) 来控制混合缓冲池。

以下示例为 SQL Server 实例启用混合缓冲池：

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
```

默认情况下，混合缓冲池在实例范围内处于禁用状态。 请注意，要使设置更改生效，必须重启 SQL Server 实例。 必须重启以帮助分配足够的哈希页，以计算服务器上的总 PMEM 容量。

以下示例为特定数据库启用混合缓冲池。

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = ON;
```

默认情况下，混合缓冲池在数据库范围内处于启用状态。

## <a name="disable-hybrid-buffer-pool"></a>启用混合缓冲池

以下示例在实例级别禁用混合缓冲池：

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = OFF;
```

默认情况下，在实例级别禁用混合缓冲池。 为了使此更改生效，必须重启实例。 这可确保为缓冲池分配足够的哈希页，因为现在需要考虑服务器上的 PMEM 容量。

以下示例为特定数据库禁用混合缓冲池。

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = OFF;
```

默认情况下，混合缓冲池在数据库范围内处于启用状态。

## <a name="view-hybrid-buffer-pool-configuration"></a>查看混合缓冲池配置

以下示例返回实例的当前混合缓冲池配置状态。

```sql
SELECT * FROM
sys.server_memory_optimized_hybrid_buffer_pool_configuration;
```

以下示例列出混合缓冲池 (`is_memory_optimized_enabled`) 的数据库和数据库级别设置。

```sql
SELECT name, is_memory_optimized_enabled FROM sys.databases;
```

## <a name="best-practices-for-hybrid-buffer-pool"></a>混合缓冲池的最佳做法

 - 在 Windows 上格式化 PMEM 设备时，使用可用于 NTFS 的最大分配单元大小（Windows Server 2019 中为 2 MB）并确保已为 DAX（直接访问）格式化该设备。

 - 在 Windows 上使用[内存中的锁定页面](./enable-the-lock-pages-in-memory-option-windows.md)。

 - 文件大小应为 2 MB 的倍数（模数 2 MB 应等于零）。

 - 如果混合缓冲池的服务器范围设置禁用，则任何用户数据库将不能使用该功能。

 - 如果混合缓冲池的服务器范围设置启用，你可使用数据库范围设置禁用单个用户数据库的功能。
