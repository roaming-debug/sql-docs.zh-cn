---
title: “重新生成索引”任务（维护计划）| Microsoft Docs
description: 了解如何使用重新生成索引任务通过新的填充因子对 SQL Server 数据库中的表重新创建索引。
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- reindex
- sql13.swb.maint.reindex.f1
helpviewer_keywords:
- Rebuild Index Task dialog box
ms.assetid: 33e2940b-139f-4563-b0cb-5683f08bd879
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6c95f60259fb25b457e4349b6e0798ce4fa0e6fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351181"
---
# <a name="rebuild-index-task-maintenance-plan"></a>“重新生成索引”任务（维护计划）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用“重新生成索引任务”对话框可用利用新的填充因子对数据库中的表重新创建索引。 填充因子确定索引中每页上的空白空间量，以容纳将来的扩展内容。 随着向表中添加数据，由于没有维持填充因子，可用空间将逐渐填满。 重新组织数据页和索引页可以重新建立可用空间。  
  
 **“‘重新生成索引’任务”** 使用 ALTER INDEX 语句。 有关此页所述选项的详细信息，请参阅 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)。  
  
## <a name="options"></a>选项  
 **Connection**  
 选择执行此任务时使用的服务器连接。  
  
 **新建**  
 创建一个新的服务器连接，在执行此任务时使用。 下面对 **“新建连接”** 对话框进行了介绍。  
  
 **数据库**  
 指定受此任务影响的数据库。  
  
-   **“所有数据库”**  
  
     生成的维护计划将对除 tempdb 之外的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库运行维护任务。  
  
-   **所有系统数据库**  
  
     生成的维护计划将对除 tempdb 之外的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据库运行维护任务。 对用户创建的数据库不运行维护任务。  
  
-   **所有用户数据库**  
  
     生成的维护计划将对用户创建的所有数据库运行维护任务。 但不会对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据库运行任何维护任务。  
  
-   **特定数据库**  
  
     生成的维护计划将只对所选数据库运行维护任务。 如果选择此选项，则必须至少在列表中选择一个数据库。  
  
    > [!NOTE]  
    >  只能对兼容级别被设置为 80 或更高的数据库运行维护计划。 不显示兼容级别设置为 70 或更低的数据库。  
  
 **Object**  
 将“选择”网格限制为显示表、显示视图或同时显示两者。  
  
 **选择**  
 指定受此任务影响的表或索引。 在“对象”框中选择 **“表和视图”** 时不可用。  
  
 **每页的默认可用空间**  
 删除数据库中表上的索引，并使用在创建索引时指定的填充因子重新创建索引。  
  
 **将每页可用空间更改为**  
 删除数据库中表上的索引，并使用新的、自动计算的填充因子重新创建索引，从而在索引页上保留指定的可用空间。 百分比越高，索引页上保留的可用空间就越多，并且索引增长也就越大。 有效值为 0 到 100。  
  
 **对 tempdb 中的结果进行排序**  
 使用 `SORT_IN_TEMPDB` 选项，该选项可确定在索引创建过程中生成的中间排序结果的临时存储位置。 如果不需要执行排序操作，或者可以在内存中执行排序，则系统会忽略 `SORT_IN_TEMPDB`选项。  
  
 **填充索引 (Pad index)**  
 指定索引填充  
  
 **保持索引联机**  
 使用 `ONLINE` 选项，用户可以在索引操作期间访问基础表或聚集索引数据以及任何关联的非聚集索引。  
  
> [!NOTE]
>  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的各版本中均不提供联机索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 **不要重新生成索引 | 重新脱机生成索引**  
 指定要对无法重新联机生成的索引类型执行的操作。  
  
 **MAXDOP**  
 指定一个值，以限制在并行计划执行过程中使用的处理器数量。  
  
 **使用低优先级**  
 选择此选项，以等待低优先级锁。  
  
 **等待后中止**  
 指定要在 **最大持续时间** 过后执行的操作。  
  
 **最大持续时间**  
 指定低优先级锁的等待时间。  
  
 **查看 T-SQL**  
 根据所选选项，查看针对此任务的服务器执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
> [!NOTE]  
>  当受影响的对象很多时，可能需要相当长的时间才可显示。  


[!INCLUDE[index-stats-options-reorg-5589131-2999104](../../includes/paragraph-content/index-stats-options-reorganize-maintenance-plan-include.md)]

  
## <a name="new-connection-dialog-box"></a>“新建连接”对话框  
 **连接名称**  
 输入新连接的名称。  
  
 **选择或输入服务器名称**  
 选择执行此任务时所要连接的服务器。  
  
 **“刷新”**  
 刷新可用服务器的列表。  
  
 **输入登录服务器所需的信息**  
 指定如何对服务器进行身份验证。  
  
 **使用 Windows 集成安全性**  
 使用 Windows 身份验证连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例。  
  
 **使用特定用户名和密码**  
 使用 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 身份验证连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 此选项不可用。  
  
 **用户名**  
 提供一个在进行身份验证时要使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 此选项不可用。  
  
 **密码**  
 提供一个在进行身份验证时要使用的密码。 此选项不可用。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [DBCC DBREINDEX (Transact-SQL)](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [用于索引的 SORT_IN_TEMPDB 选项](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)   
 [联机索引操作准则](../../relational-databases/indexes/guidelines-for-online-index-operations.md)   
 [联机索引操作的工作方式](../../relational-databases/indexes/how-online-index-operations-work.md)   
 [联机执行索引操作](../../relational-databases/indexes/perform-index-operations-online.md)  
  
  
