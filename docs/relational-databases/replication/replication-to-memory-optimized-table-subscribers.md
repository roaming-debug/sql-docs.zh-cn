---
description: 复制到内存优化表订阅服务器
title: 复制到内存优化表订阅服务器 | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
ms.assetid: 1a8e6bc7-433e-471d-b646-092dc80a2d1a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 41dfd13fc3686bd8bb031df86c359ce96553f833
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479788"
---
# <a name="replication-to-memory-optimized-table-subscribers"></a>复制到内存优化表订阅服务器
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  用作快照和事务复制订阅服务器（不包括对等事务复制）的表可以配置为内存优化表。 其他复制配置与内存优化表不兼容。 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]及更高版本支持此功能。  
  
## <a name="two-configurations-are-required"></a>需要进行两项配置  
  
-   **将订阅服务器数据库配置为支持复制到内存优化表**  
  
     通过使用 [sp_addsubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 或 [sp_changesubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) 将 \@memory_optimized 属性设置为 true。  
  
-   **将项目配置为支持复制到内存优化表**  
  
     通过使用 [sp_addarticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 或 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) 为项目设置 `@schema_option = 0x40000000000` 选项。  
  
#### <a name="to-configure-a-memory-optimized-table-as-a-subscriber"></a>将内存优化表配置为订阅服务器表  
  
1.  创建事务发布。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
2.  向发布添加项目。 有关详细信息，请参阅 [定义项目](../../relational-databases/replication/publish/define-an-article.md)。  
  
     如果是使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 进行配置，请将 sp_addarticle 存储过程的 \@schema_option 参数设置为   
    **0x40000000000** 及更高版本支持此功能。  
  
3.  在项目属性窗口中，将“启用内存优化”  设置为“true” 。  
  
4.  启动快照代理作业以为此发布生成初始快照。 有关详细信息，请参阅 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
5.  此时，创建新订阅。 在“新建订阅向导”  中，将“内存优化订阅”  设置为“true” 。  

 内存优化表现在应该开始从发布服务器接收更新。  
  
#### <a name="reconfigure-an-existing-transaction-replication"></a>重新配置现有的事务复制  
  
1.  转到 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的订阅属性，然后将“内存优化订阅”  设置为“true” 。 只有在重新初始化订阅之后，系统才会应用这些更改。  
  
     如果是使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 进行配置，请将 sp_addsubscription 存储过程的 \@memory_optimized 参数设置为 true。  
  
2.  转到 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的发布项目属性，然后将“启用内存优化”  设置为“true”。  
  
     如果是使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 进行配置，请将 sp_addarticle 存储过程的 \@schema_option 参数设置为   
    **0x40000000000** 及更高版本支持此功能。  
  
3.  内存优化表不支持聚集索引。 若要让复制能够通过在目标上将聚集索引转换成非聚集索引来处理此情况，请将“为内存优化项目将聚集索引转换成非聚集索引”  设置为“true”。  
  
     如果是使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 进行配置，请将 sp_addarticle 存储过程的 \@schema_option 参数设置为 0x0000080000000000。  
  
4.  重新生成快照。  
  
5.  重新初始化订阅。  
  
## <a name="remarks-and-restrictions"></a>备注和限制  
 仅支持单向事务复制。 不支持对等事务复制。  
  
 内存优化表无法发布。  
  
 分发服务器上的复制表无法配置为内存优化表。  
  
 合并复制无法包括内存优化表。  
  
 在订阅服务器上，在事务复制中涉及的表可以配置为内存优化表，但订阅服务器上的表必须满足针对内存优化表的要求。 这要求以下限制。  
 
-   复制到订阅服务器上的内存优化表的表限制为内存优化表允许的数据类型。 有关详细信息，请参阅 [内存中 OLTP 支持的数据类型](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)。  
  
-   内存优化表并非支持所有 Transact-SQL 功能。 有关详细信息，请参阅[内存中 OLTP 不支持的 Transact-SQL 构造](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)。  
  
##  <a name="modifying-a-schema-file"></a><a name="Schema"></a> 修改架构文件  
  
-   如果使用内存优化表选项 `DURABILITY = SCHEMA_AND_DATA` ，则表必须具有非聚集主键索引。  
  
-   ANSI_PADDING 必须为 ON。  
  
  
  
