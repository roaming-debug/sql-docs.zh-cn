---
description: MSSQLSERVER_605
title: MSSQLSERVER_605 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 605 (Database Engine error)
ms.assetid: d8d3a22e-1ff8-48a4-891f-4c8619437e24
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d2ff4a40812fdedba5d3ed6b0f8b198f497a64a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211393"
---
# <a name="mssqlserver_605"></a>MSSQLSERVER_605
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|605|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|WRONGPAGE|  
|消息正文|尝试在数据库 %d 中提取逻辑页 %S_PGID 失败。 该逻辑页属于分配单元 %I64d，而非 %I64d。|  
  
## <a name="explanation"></a>说明  
此错误通常表示指定数据库中的页或分配已损坏。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会在根据页链接或使用索引分配映射 (IAM) 读取属于表的页时，检测到此损坏。 分配给表的所有页必须属于与该表相关联的分配单元之一。 如果页眉中包含的分配单元 ID 不匹配与表相关联的分配单元 ID，将引发此异常。 错误消息中列出的第一个分配单元 ID 是页眉中显示的 ID，而第二个分配单元值则是与表相关联的 ID。  
  
**数据损坏错误**  
  
严重级别为 21 表示可能存在数据损坏。 可能的原因包括损坏的页链、损坏的 IAM 或该对象的 [sys.objects](~/relational-databases/system-catalog-views/sys-objects-transact-sql.md) 目录视图中存在无效条目。 这些错误通常由硬件或磁盘设备驱动程序故障而引起。  
  
**暂时性错误**  
  
严重级别为 12 表示可能存在暂时性错误，即在缓存中出现错误，但不表示对磁盘上的数据造成破坏。 暂时性的 605 错误可由以下条件引发：  
  
-   操作系统过早地通知 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已完成某个 I/O 操作；尽管不存在实际的数据损坏，但显示错误消息。  
  
 - 运行带有优化器提示 NOLOCK 的查询，或将事务隔离级别设置为 READ UNCOMMITTED。 当使用 NOLOCK 或 READ UNCOMMITTED 事务隔离级别的查询尝试读取被其他用户移走或更改的数据时，将发生 605 错误。 若要验证是否为暂时性的 605 错误，请稍后重新运行该查询。 
  
通常，如果在数据访问期间发生该错误，但后续的 DBCC CHECKDB 操作在没有出错的情况下完成，则 605 错误可能是暂时的。  
  
## <a name="user-action"></a>用户操作  
如果 605 错误不是暂时性的，则说明问题很严重，必须运行以下任务来纠正该问题：  
  
1.  运行以下查询，确定与消息中指定的分配单元相关联的表。 用错误消息中说明的分配单元替换 `allocation_unit_id`。  
  
    ```sql  
    USE`database_name`;  
  
    GO  
  
    SELECT au.allocation_unit_id, OBJECT_NAME(p.object_id) AS table_name, fg.name AS filegroup_name,  
  
    au.type_desc AS allocation_type, au.data_pages, partition_number  
  
    FROM sys.allocation_units AS au  
  
    JOIN sys.partitions AS p ON au.container_id = p.partition_id  
  
    JOIN sys.filegroups AS fg ON fg.data_space_id = au.data_space_id  
  
    WHERE au.allocation_unit_id = `allocation_unit_id` OR au.allocation_unit_id = `allocation_unit_id`  
  
    ORDER BY au.allocation_unit_id;  
  
    GO  
    ```
  
2.  对与错误消息中说明的第二个分配单元 ID 相关联的表，执行不带 REPAIR 子句的 DBCC CHECKTABLE。  
  
3.  尽快执行不带 REPAIR 子句的 DBCC CHECKDB，以确定整个数据库中的总体损坏程度。  
  
4.  检查错误日志以查找经常随 605 错误一起发生的其他错误，并检查 Windows 事件日志以查找任何与系统或硬件有关的问题。 修复日志中包含的所有与硬件相关的问题。  
  
如果问题与硬件无关，请执行以下任务之一：  
  
1.  从已知的干净备份中还原数据库。 可以利用页面还原备份功能来还原损坏的页。  
  
2.  运行带有 REPAIR 子句的 DBCC CHECKDB（这是步骤 3 中所执行 DBCC CHECKDB 操作推荐的做法），以修复损坏。 如果运行具有 REPAIR 子句的 DBCC CHECKDB 无法解决存在的问题，请与主要支持提供商联系。 提供 DBCC CHECKDB 的输出以供查看。  
  
    > [!CAUTION]  
    > 如果您不确定运行带有 REPAIR 子句的 DBCC CHECKDB 会对数据造成何种影响，请在运行该语句前与您的主要支持提供商联系。  
  
## <a name="see-also"></a>另请参阅  
[DBCC CHECKTABLE (Transact-SQL)](~/t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
  
