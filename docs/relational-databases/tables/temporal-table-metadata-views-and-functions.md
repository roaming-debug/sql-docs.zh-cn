---
description: 临时表元数据视图和函数
title: 临时表元数据视图和函数 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: e5d23ec9-7d18-40f6-add4-bea13132d0b9
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a552f940165a2bf45810dd4f6abbbd4f71513299
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477178"
---
# <a name="temporal-table-metadata-views-and-functions"></a>临时表元数据视图和函数


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 包括多个元数据库视图和函数，可以让管理员检索有关临时表的信息。

以下元数据视图公开了有关临时表的信息：

- [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)
- [sys.periods (Transact-SQL)](../../relational-databases/system-catalog-views/sys-periods-transact-sql.md)

 以下元数据函数公开了有关临时表的信息：

- [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)

- [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md)

- [COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md)

## <a name="next-steps"></a>后续步骤

- [临时表](../../relational-databases/tables/temporal-tables.md)
- [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [临时表安全性](../../relational-databases/tables/temporal-table-security.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
