---
title: 使用内存优化表的要求 | Microsoft Docs
description: 了解内存中 OLTP 的使用要求，包括 SQL 数据库版本、内存及存储注意事项和安装。
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47d9a7e8-c597-4b95-a58a-dcf66df8e572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5a7c64a949ad4eabfbd02ad78229b04a919ab032
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237186"
---
# <a name="requirements-for-using-memory-optimized-tables"></a>使用内存优化表的要求
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  若要了解如何在 Azure DB 中使用内存中 OLTP，请参阅 [Get started with In-Memory in SQL Database](/azure/azure-sql/in-memory-oltp-overview)（在 SQL 数据库中使用内存中功能入门）。  
  
 除了需要满足 [安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)外，还要满足以下使用内存中 OLTP 的要求：  
  
-   任何版本的 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1（或更高版本）。 对于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 和 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM（SP1 预览版），需要 Enterprise、Developer 或 Evaluation 版。
    
    > [!NOTE]
    > 内存中 OLTP 要求 64 位版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要有足够的内存来保留内存优化表和索引中的数据，以及额外的内存来支持联机工作负荷。 有关详细信息，请参阅 [估算内存优化表的内存需求](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) 。  

-   在虚拟机 (VM) 中运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，请确保有足够的内存分配给 VM 以提供内存优化表和索引所需的内存。 用于保证 VM 内存分配的配置选项可称为“内存预留”或“最小 RAM”（使用动态内存时），具体取决于 VM 主机应用程序。 请确保这些设置足以满足 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的数据库需求。
  
-   可用磁盘空间需为你的持久内存优化表大小的两倍。  
  
-   处理器必须支持指令 **cmpxchg16b** 才能使用内存中 OLTP。 所有新式 64 位处理器都支持 **cmpxchg16b**。  
  
     如果你使用的是虚拟机，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 显示因处理器较旧而生成的错误消息，请检查虚拟机主机应用程序是否将配置选项设置为允许使用 **cmpxchg16b**。 如果没有设置，你可以使用支持 **cmpxchg16b** 的 Hyper-V，而无需修改配置选项。  
  
-   内存中 OLTP 作为 **数据库引擎服务** 的一部分进行安装。  
  
     要安装报表生成（[确定表或存储过程是否应移植到内存中 OLTP](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)）和 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]（通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器管理内存中 OLTP），[请下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。   
  
## <a name="important-notes-on-using-hek_2"></a>有关使用 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 的重要说明  
  
-   自 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 起，对内存优化表没有大小限制（不同于可用内存）。 

-   在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中，数据库中所有持久表在内存中的总大小不应超过 250 GB。 有关详细信息，请参阅 [估算内存优化表的内存需求](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)。  

> [!NOTE]
> 从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 开始，Standard 和 Express 版本支持内存中 OLTP，但它们对可用于给定数据库中内存优化表的内存量设置了配额。 在 Standard 版本中，每个数据库的配额是 32GB；在 Express 版本中，每个数据库的配额是 352MB。 
  
-   如果创建一个或多个包含内存优化表的数据库，应通过向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务启动帐户授予 SE_MANAGE_VOLUME_NAME 用户权限，启用即时文件初始化 (IFI)。 如果没有 IFI，内存优化存储文件（数据和差异文件）将在创建时初始化，这会对工作负荷性能产生负面影响。 有关 IFI 的详细信息，包括如何启用它，请参阅[数据库即时文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)。
  
## <a name="see-also"></a>另请参阅  
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
 [数据库实例文件初始化](../../relational-databases/databases/database-instant-file-initialization.md)  
 [内存体系结构指南](../../relational-databases/memory-management-architecture-guide.md)
