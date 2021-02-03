---
title: PolyBase 中的常见问题解答 |Microsoft Docs
description: 比较 PolyBase 和链接服务器，并比较大数据群集中的 PolyBase 和独立实例中的 PolyBase。 了解 PolyBase 2019 中的新增功能。
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.openlocfilehash: 70ffec480513c7a002a8691f2624a604bc82ead0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235807"
---
# <a name="frequently-asked-questions"></a>常见问题

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="polybase-vs-linked-servers"></a>PolyBase 与 链接服务器
下表主要说明了 PolyBase 和 链接服务器之间的功能差异：

|PolyBase | 链接服务器|
|--------------------------|--------------------------|  
|数据库范围的对象|实例范围的对象|
|使用 ODBC 驱动程序|使用 OLEDB 提供程序|
|支持对所有数据源执行只读操作，仅支持对 HADOOP 和数据池数据源的插入操作|支持读写操作|
|可横向扩展来自单个连接的远程数据源查询 |无法横向扩展来自单个连接的远程数据源查询|
|支持谓词下推|支持谓词下推|
|可用性组不需要单独配置|可用性组中的每个实例均需要单独配置|
|仅基本身份验证|基本身份验证和集成身份验证|
|适用于处理大量行的分析查询|适用于返回单行或少量行的 OLTP 查询|
|使用外部表的查询不能参与分布式事务|分布式查询可以参与分布式事务|

## <a name="whats-new-in-polybase-2019"></a>PolyBase 2019 中的新增功能 

[!INCLUDE[sssql19](../../includes/sssql19-md.md)]中的 PolyBase 现可从更多种数据源读取数据。 这些外部数据源中的数据可作为外部表存储在 SQL Server 上。 PolyBase 还支持对这些外部数据源进行下推计算（不包括 ODBC 泛型类型）。

### <a name="compatible-data-sources"></a>兼容的数据源

- SQL Server
- Oracle
- Teradata
- MongoDB
- 兼容的 ODBC 泛型类型
  
> [!NOTE]
> PolyBase 可允许使用第三方 ODBC 驱动程序连接到外部数据源。 这些驱动程序不随 PolyBase 一起提供，可能不会按预期工作。 有关详细信息，请参阅有关 PolyBase ODBC 泛型配置的[指南](../../relational-databases/polybase/polybase-configure-odbc-generic.md)。  

## <a name="polybase-in-big-data-clusters-vs-polybase-in-stand-alone-instances"></a>大数据群集中的 PolyBase 与独立实例中的 PolyBase

下表主要说明了 [!INCLUDE[sssql19](../../includes/sssql19-md.md)]独立安装和 [!INCLUDE[sssql19](../../includes/sssql19-md.md)]大数据群集提供的 PolyBase 功能：

|Feature |大数据群集|独立实例|
|--------------------------|--------------------------|---------|   
|为 SQL Server、Oracle、Teradata 和 Mongo DB 创建外部数据源 |X|X|
|使用可兼容的第三方 ODBC 驱动程序创建外部数据源 | | X|
|为 HADOOP 数据源创建外部数据源 | X| X|
|为 Azure Blob 存储创建外部数据源 | X| X|
|在 SQL Server 数据池上创建外部表 | X| |
|在 SQL Server 存储池上创建外部表 | X| |
|执行横向扩展查询 | X| X（仅限 Windows） |

> [!NOTE]
>此表未说明最新的 [!INCLUDE[sssql19](../../includes/sssql19-md.md)] CTP 提供的功能。 有关可用功能的信息，请参阅发行说明。 有关使用 ODBC 泛型连接器进行连接的详细信息，请访问我们的[配置 ODBC 泛型类型的操作指南](polybase-configure-odbc-generic.md)。
