---
description: 临时表分区
title: 临时表分区 | Microsoft Docs
ms.custom: ''
ms.date: 04/26/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 313714b8-4ad1-4c14-93a3-7f628a334a51
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83180bde505a156c5a6fd5832916e578fb73c52c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484449"
---
# <a name="partitioning-with-temporal-tables"></a>时态表分区


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


可以单独在当前表和历史记录表上使用分区。 但是，分区不能用于更改数据无系统版本控制的数据内容。

> [!NOTE]
> 分区是在 Service Pack 1 之前以及早期版本的 SQL Server 2016 中的 Enterprise Edition 功能。 在 SQL Server 2016 Service Pack 1 及更高版本的所有版本中都支持分区。

- **当前表：**

  - 当 **SWITCH IN** 为 **SWITCH IN** 时，当前表的 **SWITCH IN** 可用于加快数据加载和查询
  - **SYSTEM_VERSIONING** 为 **ON** 时，当前表的 **SWITCH IN**

- **历史记录表：**

  - 当 **SWITCH OUT** 为 **SWITCH OUT** 时，可以执行历史记录表中的 **SWITCH OUT** to purge portions of h时，可以执行历史记录表中的tory data that 时，可以执行历史记录表中的 no longer relevant.
  - 当 **SWITCH IN** 为 **SWITCH IN** 时，不允许执行 **SWITCH IN** since it can invalidate temporal data cons时，不允许执行tency.

## <a name="next-steps"></a>后续步骤

- [临时表](../../relational-databases/tables/temporal-tables.md)
- [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [临时表安全性](../../relational-databases/tables/temporal-table-security.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
