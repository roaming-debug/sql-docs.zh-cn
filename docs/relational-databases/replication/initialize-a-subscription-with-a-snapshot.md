---
description: 使用新发布的快照初始化订阅
title: 使用快照初始化订阅
ms.custom: ''
ms.date: 03/23/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], initializing subscriptions
- initializing subscriptions [SQL Server replication], snapshots
ms.assetid: 77a9ade2-cdc0-4ae9-a02d-6e29d7c2ada0
author: MashaMSFT
ms.author: mathoma
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2936d9c53f3dd99bcd112de3743107128da25fc2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479918"
---
# <a name="initialize-a-subscription-with-a-snapshot-for-a-new-publication"></a>使用新发布的快照初始化订阅

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

本文描述初始化复制发布时发生的过程。 初始快照将应用于订阅服务器。

## <a name="snapshot-for-a-new-publication"></a>新发布的快照

默认情况下，在创建发布后捕获快照。
快照将复制到快照文件夹。 对于使用“新建发布向导”创建的合并发布，将发生此默认行为。

### <a name="snapshot-is-applied-to-subscriber"></a>快照应用于订阅服务器

新快照由代理应用于订阅服务器。 应用发生在订阅的初始同步期间。 应用哪个代理取决于发布的类型：

- 对于事务和快照发布   ：
  - 分发代理。

- 对于合并发布  ：
  - 合并代理。

### <a name="type-of-publication"></a>发布的类型

下表显示了每种发布类型的快照内容。

&nbsp;

| 快照用于的发布类型 | 快照的内容 |
| :---------------------------------------- | :----------------------- |
| <ul> <li>快照发布</li> <li>事务发布</li> <li>不使用参数化筛选器的合并发布</li> </ul> | <ul> <li>架构</li> <li>大容量复制程序 (BCP) 文件中的数据</li> <li>约束</li> <li>扩展属性</li> <li>索引</li> <li>触发器</li> <li>复制所需的系统表</li> </ul> <br/>请参阅[创建并应用快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。 |
| <ul> <li>使用参数化筛选器的合并发布</li> </ul> | <ul> <li>架构快照（复制脚本、已发布对象，但没有数据）</li> <li>属于订阅分区的数据</li> </ul> <br/>请参阅[带有参数化筛选器的合并发布的快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。 |
| | |

#### <a name="two-part-process-with-merge-publication-that-uses-parameterized-filters"></a>使用参数化筛选器的合并发布的两部分过程

对于使用参数化筛选器的合并发布，将使用以下两部分过程创建快照：

1. 此时会创建一个架构快照，其中包含以下项：
   - 复制脚本。
   - 已发布对象的架构。
   - _（但没有数据。）_

2. 然后，使用快照初始化每个订阅。 快照包括以下项：
   - 脚本和架构（从架构快照复制）。
   - 属于订阅分区的数据。

## <a name="type-of-replication"></a>复制类型

快照中包含的文件类型取决于复制类型和发布中的项目。

&nbsp;

| 复制类型 | 常用快照文件 |
| :------------------ | :-------------------- |
| 快照复制或<br/>事务复制 | &bullet; 架构 (.sch) <br/>&bullet; 数据 (.bcp) <br/>&bullet; 约束和索引 (.dri) <br/>&bullet; 压缩的快照文件 (.cab) <br/>&bullet; 触发器 (.tag)，仅用于更新订阅服务器 <br/><br/>&bullet; 约束 (.idx)。 |
| 合并复制                                      | &bullet; 架构 (.sch) <br/>&bullet; 数据 (.bcp) <br/>&bullet; 约束和索引 (.dri) <br/>&bullet; 压缩的快照文件 (.cab) <br/>&bullet; 触发器 (.trg) <br/><br/>&bullet; 系统表数据 (.sys) <br/>&bullet; 冲突表 (.cft)。 |
| | |

### <a name="snapshot-folder"></a>快照文件夹

通过将文件复制到默认的快照文件夹，或复制到用于快照的备用文件夹，可以传输这些文件   。

配置分发服务器时指定快照文件夹。 创建发布时指定备用文件夹。

### <a name="resume-transfer-after-interruption"></a>中断后恢复传输

如果传输由于不可靠的连接而中断，则网络连接恢复后将自动恢复将文件传输到快照文件夹的过程。

为提高效率，恢复时不会重新发送中断前已传输完毕的任何文件。

## <a name="snapshot-options"></a>快照选项

使用快照初始化订阅时，有几个选项可用。 可以：

- 指定一个备用快照文件夹位置，以替代默认的快照文件夹位置或者与之并存。 有关详细信息，请参阅[修改快照选项](../../relational-databases/replication/snapshot-options.md)。

- 压缩快照，以便在可移动介质上进行存储或者通过速度较低的网络进行传输。 有关详细信息，请参阅 [Compressed Snapshots](../../relational-databases/replication/snapshot-options.md#compressed-snapshots)。

- 在应用快照之前或之后执行 Transact-SQL 脚本。 有关详细信息，请参阅[在应用快照之前和之后执行脚本](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)。

- 使用文件传输协议 (FTP) 传输快照文件。 有关详细信息，请参阅[通过 FTP 传输快照](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)。

## <a name="see-also"></a>另请参阅

[初始化订阅](../../relational-databases/replication/initialize-a-subscription.md)

[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)
