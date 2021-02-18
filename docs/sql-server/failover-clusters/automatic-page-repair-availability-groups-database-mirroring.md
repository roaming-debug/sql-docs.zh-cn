---
title: 可用性组和数据库镜像的自动页修复
description: 了解当数据库参与 Always On 可用性组或数据库镜像时，如何自动修复某些类型的页损坏。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- automatic page repair
- Availability Groups [SQL Server], automatic page repair
- database mirroring [SQL Server], automatic page repair
- suspect pages [SQL Server]
ms.assetid: cf2e3650-5fac-4f34-b50e-d17765578a8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 111a5ad98210207fcdf55d3656953ddda6ac38c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346181"
---
# <a name="automatic-page-repair-availability-groups-database-mirroring"></a>自动页修复（可用性组：数据库镜像）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据库镜像和 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]支持自动页修复。 在某些类型的错误导致页损坏，使其无法读取后，数据库镜像伙伴（主体或镜像）或可用性副本（主副本或辅助副本）将尝试自动修复该页。 无法读取该页的伙伴/副本将从其伙伴或从其他副本请求该页的新副本。 如果此请求成功，则将以可读副本替换不可读的页，并且这通常会解决该错误。  
  
 一般来说，数据库镜像和 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 以等效方式处理 I/O 错误。 下面将具体介绍几个差异。  
  
> [!NOTE]  
>  自动页修复有别于 DBCC 修复。 自动页修复会保留所有数据。 相反，使用 DBCC REPAIR_ALLOW_DATA_LOSS 选项更正错误可能需要删除某些页（从而会删除数据）。  
  
-   [导致自动页修复尝试的错误类型](#ErrorTypes)  
  
-   [无法自动修复的页类型](#UnrepairablePageTypes)  
  
-   [处理主体/主数据库上的 i/o 错误](#PrimaryIOErrors)  
  
-   [处理镜像/辅助数据库上的 i/o 错误](#SecondaryIOErrors)  
  
-   [开发人员最佳做法](#DevBP)  
  
-   [如何查看自动页修复尝试](#ViewAPRattempts)  
  
##  <a name="error-types-that-cause-an-automatic-page-repair-attempt"></a><a name="ErrorTypes"></a> 导致自动页修复尝试的错误类型  
 数据库镜像自动页修复只尝试修复特定数据文件中的页，此数据文件是指对其执行的操作由于下表中列出的某一错误而失败的数据文件。  
  
|错误号|说明|导致自动页修复尝试的实例|  
|------------------|-----------------|---------------------------------------------------------|  
|823|仅当操作系统对数据执行循环冗余检查 (CRC) 失败时才执行此操作。|ERROR_CRC。 此错误的操作系统值为 23。|  
|824|逻辑错误。|逻辑数据错误，例如残缺写或错误的页校验和。|  
|829|页已标记为还原已挂起。|全部。|  
  
 若要查看最近的 823 CRC 错误和 824 错误，请参阅 [msdb](../../relational-databases/system-tables/suspect-pages-transact-sql.md) 数据库中的 [suspect_pages](../../relational-databases/databases/msdb-database.md) 表。  

  
##  <a name="page-types-that-cannot-be-automatically-repaired"></a><a name="UnrepairablePageTypes"></a> Page Types That Cannot Be Automatically Repaired  
 页自动修复不能修复以下控制页类型：  
  
-   文件头页（页 ID 0）。  
  
-   页 9（数据库引导页）。  
  
-   分配页：全局分配映射 (GAM) 页、共享全局分配映射 (SGAM) 页和页可用空间 (PFS) 页。  
  
 
##  <a name="handling-io-errors-on-the-principalprimary-database"></a><a name="PrimaryIOErrors"></a> 处理主体/主数据库上的 I/O 错误  
 在主体/主数据库中，仅当数据库处于 SYNCHRONIZED 状态且主体/主数据库仍向镜像/辅助数据库发送数据库日志记录时，才会尝试自动页修复。 自动页修复尝试中操作的基本顺序如下：  
  
1.  当主体/主数据库中的数据页上发生读取错误时，主体/主数据库将使用相应的错误状态在 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) 表中插入一行。 对于数据库镜像，主体服务器将从镜像服务器请求该页的副本。 对于 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]，主体数据库将向所有辅助数据库广播请求，并且从第一个响应中获取该页。 此请求指定当前在刷新日志末尾的页 ID 和 LSN。 此页将标记为“还原已挂起” 。 这将使它在自动页修复尝试期间不可访问。 修复尝试期间对此页的访问尝试将失败，并显示错误 829（还原已挂起）。  
  
2.  收到页请求后，镜像/辅助数据库将等待，直到将日志重做到请求中指定的 LSN 处。 然后，镜像/辅助数据库将在其数据库副本中尝试访问此页。 如果可以访问此页，则镜像/辅助数据库将此页的副本发送到主体/主数据库。 否则，镜像/辅助数据库将向主体/主数据库返回错误，并且自动页修复失败。  
  
3.  主体/主数据库处理包含此页新副本的响应。  
  
4.  自动页修复尝试修复可疑页后，此页将在 **suspect_pages** 表中标记为已还原 (**event_type** = 5)。  
  
5.  如果此页 I/O 错误导致出现任何 [延迟的事务](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)，则修复此页后，主体/主数据库将尝试解决这些事务。  
  
 
##  <a name="handling-io-errors-on-the-mirrorsecondary-database"></a><a name="SecondaryIOErrors"></a> 处理镜像/辅助数据库上的 I/O 错误  
 处理在镜像/辅助数据库中发生的数据页 I/O 错误通常采用与数据库镜像和 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]相同的方式。  
  
1.  对于数据库镜像，如果镜像服务器在其重做日志记录时遇到一个或多个页 I/O 错误，则镜像会话将进入 SUSPENDED 状态。 对于 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]，如果辅助副本在其重做日志记录时遇到一个或多个页 I/O 错误，则辅助数据库将进入 SUSPENDED 状态。 此时，镜像/辅助数据库使用相应的错误状态在 **suspect_pages** 表中插入一行。 然后，镜像/辅助数据库从主体/主数据库请求此页的副本。  
  
2.  主体/主数据库服务器尝试在其数据库副本中访问此页。 如果可以访问此页，则主体/主数据库将此页的副本发送到镜像/辅助数据库。  
  
3.  如果镜像/辅助数据库收到它请求的每一页的副本，则镜像/辅助数据库将尝试恢复镜像会话。 如果自动页修复尝试修复了可疑页，此页将在 **suspect_pages** 表中标记为已还原 (**event_type** = 4)。  
  
     如果镜像/辅助数据库未从主体/主数据库收到它请求的页，则自动页修复尝试将失败。 对于数据库镜像，镜像会话将保持挂起。 对于 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]，辅助数据库将保持挂起。 如果手动恢复镜像会话或辅助数据库，则损坏的页将在同步阶段再次导致错误。  
  
 
##  <a name="developer-best-practice"></a><a name="DevBP"></a> Developer Best Practice  
 自动页修复是一个运行在后台的异步进程。 因此，请求不可读的页的数据库操作将失败，并且不管导致失败的条件是什么均返回错误代码。 当开发用于镜像数据库或可用性数据库的应用程序时，应截获失败操作的异常。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误代码为 823、824 或 829，则应稍后重试操作。  
  

##  <a name="how-to-view-automatic-page-repair-attempts"></a><a name="ViewAPRattempts"></a> 如何：查看自动页修复尝试  
 下面的动态管理视图返回对应于给定可用性数据库或镜像数据库上最新自动页修复尝试的行，每个数据库最多可对应 100 行。  
  
-   **AlwaysOn 可用性组：**  
  
     [sys.dm_hadr_auto_page_repair (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
     为针对任何可用性数据库（位于服务器实例为任何可用性组承载的可用性副本上）的每一个自动页修复尝试都返回一行。  
  
-   **数据库镜像：**  
  
     [sys.dm_db_mirroring_auto_page_repair (Transact-SQL)](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)  
  
     对服务器实例上所有镜像数据库的每个自动页修复尝试返回一行。  
  
 
## <a name="see-also"></a>另请参阅  
 [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  


