---
description: MSSQLSERVER_7105
title: MSSQLSERVER_7105
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7105 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: beb0e6bb3f960972b0f79413cd9186b2d005b7f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194114"
---
# <a name="mssqlserver_7105"></a>MSSQLSERVER_7105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|7105|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|TXT_PGNOTEXIST|
|消息正文|LOB 数据类型节点的数据库 ID %d (页 %S_PGID，槽 %d)不存在。 这通常是由于可以读取数据页上未提交的数据的事务所致。 请运行 DBCC CHECKTABLE|
||

## <a name="explanation"></a>说明

当无法访问数据库页行引用的大型对象 (LOB) 数据时，查询可能会遇到消息 7105。

此错误的严重性级别为 22，因此服务器将终止连接。 此错误消息也将写入 SQL ERRORLOG 文件和 EventID=7105 的 Windows 应用程序事件日志中。

## <a name="possible-causes"></a>可能的原因

存在以下任一原因时，可能会出现此错误：

- 数据库页中或数据库页引用的 LOB 页结构中存在数据库损坏问题。
- 出现故障的查询使用的是 `READ UNCOMMITTED ISOLATION LEVEL` 或 `NOLOCK` 查询提示。
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引擎中存在问题，导致查询失败并出现此错误。

请参阅“解决方案”和[详细信息](#more-information)部分，确定具体问题的原因以及合适的解决方案。

## <a name="user-action"></a>用户操作

1. 如消息中所示，首先应该是针对数据库运行 `DBCC CHECKDB` 或针对出现问题的表运行 `DBCC CHECKTABLE`。

    - 消息中提供了数据库 ID。
    - 若要在不运行 `DBCC CHECKDB` 的情况下精确找到受影响的表，你将需要找出出现错误的查询所访问的表。 一种方法是使用 SQL 探查器来跟踪查询。 但是，在 [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] 和 [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] R2 中，你或许可使用 system_health 扩展事件会话来查找查询。 若要详细了解如何使用 system_health 会话，请参阅此链接：[使用 system_health 会话](../extended-events/use-the-system-health-session.md)。

    - 与任何数据库一致性问题一样，可通过从不包含此问题的已知完好备份中进行还原来解决这些错误。

    - 但是，如果无法从备份中还原，请按照针对 `DBCC CHECKDB` 或 `DBCC CHECKTABLE` 的建议来解决这些错误。 这可能会导致数据丢失。 有关使用 CHECKDB 以及数据库损坏原因的详细信息，请参阅下文：[如何解决 DBCC CHECKDB 报告的数据库一致性错误](https://support.microsoft.com/kb/2015748)。
  
1. 出现此错误的原因可能是，访问表的查询使用的是隔离级别的 `READ UNCOMMITTED` 或 `NOLOCK` 查询提示（也称为脏读）。

   - 如果 `DBCC CHECKDB` 或 `DBCC CHECKTABLE` 未显示与此表和 LOB 数据相关联的任何错误，则最有可能是因为使用了脏读。 如果你的应用程序出现这种情况，那么你需要避免使用脏读或重试查询。
  
   - 如果你发现这是导致此错误的原因，则实际上不存在数据库一致性问题。

## <a name="more-information"></a>详细信息

如果数据库损坏是导致此问题的原因，则 `DBCC CHECKDB` 和/或 `DBCC CHECKTABLE` 应报告错误。 但是，这些命令不会报告消息 7105。 从 CHECKDB 中遇到的错误将取决于对 LOB 结构的引用中或 LOB 结构本身的损坏内容。

- 如果数据库页行未正确引用有效的 LOB 页，可能会看到如下错误：

    > 消息 8929，级别 16，状态 1，行 1  
    对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039828480（行内数据类型）：在由 RID = (1:179:1) 标识的数据记录所拥有的 ID 为 131203072 的行外数据中发现错误  
    消息 8964，级别 16，状态 1，行 1  
    表错误:对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039894016（LOB 数据类型）。 位于页 (1:177)、槽 1 且文本 ID 为 131203072 的行外数据节点未被引用。  
    消息 8965，级别 16，状态 1，行 1  
    表错误:对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039894016（LOB 数据类型）。 位于页 (255:177)、槽 1 且文本 ID 为 131203072 的行外数据节点由页 (1:179) 的槽 1 引用，但扫描过程中未检测到该节点  

- 在不同的情况下出现此问题可能导致不同的错误组合。 在此示例中：  

    > 数据库页 1:179，槽 1 引用了在数据库中无效的 LOB 页（页 255:177）。 页 (1:177) 是有效的 LOB 页，但从未被任何数据库页引用。 因此在这种情况下，问题在于页 1:179 的槽 1 中的行引用页 255:177 而不是 1:177。

- 确定 `DBCC CHECKDB` 错误是否与 LOB 页问题相关的关键是查找“行外数据”和“LOB 数据类型”这两个短语。

    > 消息 8929 是与引用 LOB 页的数据库页相关的错误。  
消息 8964 是一个错误，指示没有任何数据库页引用 LOB 页。  
消息 8965 是一个错误，指示某个 LOB 页被数据库页引用，但它不是有效页

    在许多涉及到这些类型的错误的情况下，进行修复将导致删除指向 LOB 数据的行和 LOB 数据本身。 修复算法将尝试仅删除影响相关数据库行的 LOB 片段，但无法找到在所有情况下都这样做，具体情况视 LOB“树结构”中的损坏内容而定。

- 在此处显示的示例中，CHECKTABLE 使用 `REPAIR_ALLOW_DATA_LOSS` 返回的消息如下所示：

    > 修复:页 (1:179) 的槽 1 上已删除的记录：对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039828480（行内数据类型）。 将重新生成索引。  
    修复:页 (1:177) 的槽 1 上已删除的 ID 为 131203072 的行外数据列：对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039894016（LOB 数据类型）。  
    消息 8929，级别 16，状态 1，行 1  
    对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039828480（行内数据类型）：在由 RID = (1:179:1) 标识的数据记录所拥有的 ID 为 131203072 的行外数据中发现错误  
            该错误已修复。  
    消息 8964，级别 16，状态 1，行 1  
    表错误:对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039894016（LOB 数据类型）。 位于页 (1:177)、槽 1 且文本 ID 为 131203072 的行外数据节点未被引用。  
            该错误已修复。  
    消息 8965，级别 16，状态 1，行 1  
    表错误:对象 ID 2137058649，索引 ID 0，分区 ID 72057594038910976，分配单元 ID 72057594039894016（LOB 数据类型）。 位于页 (255:177)、槽 1 且文本 ID 为 131203072 的行外数据节点由页 (1:179) 的槽 1 引用，但扫描过程中未检测到该节点。  
            无法修复此错误

    显示 `Could not repair this error` 具有误导性的最后一条消息。 此错误已修复，因为指向无效页 (255:177) 的数据库页行已被删除。