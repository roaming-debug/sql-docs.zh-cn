---
description: 在 PolyBase 中下推计算
title: 在 PolyBase 中下推计算 | Microsoft Docs
dexcription: Enable pushdown computation to improve performance of queries on your Hadoop cluster. You can select a subset of rows/columns in an external table for pushdown.
ms.date: 11/17/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016 || =sqlallproducts-allversions'
ms.openlocfilehash: 59ff1e7807a8bdd8427e3b902bf53c111d52c7b7
ms.sourcegitcommit: 4c3949f620d09529658a2172d00bfe37aeb1a387
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2020
ms.locfileid: "96127841"
---
# <a name="pushdown-computations-in-polybase"></a>在 PolyBase 中下推计算

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

下推计算提高了外部数据源上的查询性能。 从 SQL Server 2016 开始，下推计算可用于 Hadoop 外部数据源。 SQL Server 2019 为其他类型的外部数据源引入了下推计算功能。

## <a name="enable-pushdown-computation"></a> 启用下推计算

可在下列文章中了解如何为特定类型的外部数据源配置下推计算：

- [在 Hadoop 中启用下推计算](polybase-configure-hadoop.md#pushdown)
- [配置 PolyBase 以访问 Oracle 中的外部数据](polybase-configure-oracle.md)
- [配置 PolyBase 以访问 Teradata 中的外部数据](polybase-configure-teradata.md)
- [配置 PolyBase 以访问 MongoDB 中的外部数据](polybase-configure-mongodb.md)
- [配置 PolyBase 以使用 ODBC 泛型类型访问外部数据](polybase-configure-odbc-generic.md)
- [配置 PolyBase 以访问 SQL Server 中的外部数据](polybase-configure-sql-server.md)

## <a name="select-a-subset-of-rows"></a>选择行的子集

使用谓词下推来提高从外部表中选择行子集的查询的性能。

在此示例中，SQL Server 2016 启动 map-reduce 作业，以检索与 Hadoop 上的谓词 `customer.account_balance < 200000` 相匹配的行。 由于查询无需扫描表中的所有行也能成功完成，因此只有满足谓词条件的行才会复制到 SQL Server。 与帐户余额 >= 200000 的客户数相比，余额 < 200000 的客户数是很小的，这样可以节省大量时间，并且需要的临时存储空间更少。

```sql
SELECT * FROM customer WHERE customer.account_balance < 200000
SELECT * FROM SensorData WHERE Speed > 65;  
```

### <a name="select-a-subset-of-columns"></a>选择列的子集

使用谓词下推来提高从外部表中选择列子集的查询的性能。

在此查询中，SQL Server 启动了一个 map-reduce 作业来预处理 Hadoop 分隔文本文件，因此只有 customer.name 和 customer.zip_code 这两列数据将复制到 SQL Server。

```sql
SELECT customer.name, customer.zip_code
FROM customer
WHERE customer.account_balance < 200000
```

### <a name="pushdown-for-basic-expressions-and-operators"></a>基本表达式和运算符的下推

SQL Server 允许以下谓词下推的基本表达式和运算符。

- 数字、日期和时间值的二进制比较运算符（`<`、`>`、`=`、`!=`、`<>`、`>=`、`<=`）。
- 算术运算符（`+`、`-`、`*`、`/`、`%`）。
- 逻辑运算符（`AND`、`OR`）。
- 一元运算符（`NOT`、`IS NULL`、`IS NOT NULL`）。

运算符 `BETWEEN`、`NOT`、`IN` 和 `LIKE` 可能会被向下推送。 实际具体行为取决于查询优化器如何将运算符表达式重写为一系列使用基本关系运算符的语句。

此示例中的查询有多个可以向下推送到 Hadoop 的谓词。 SQL Server 能够将 map-reduce 作业推送到 Hadoop，以便执行谓词 `customer.account_balance <= 200000`。 表达式 `BETWEEN 92656 AND 92677` 也是由可以推送到 Hadoop 的二进制和逻辑运算符组成。 `customer.account_balance AND customer.zipcode` 中的逻辑 AND 是最终表达式。

鉴于这种谓词组合，map-reduce 作业可以执行所有的 WHERE 子句。 只有满足 `SELECT` 条件的数据才会复制回到 SQL Server。

```sql
SELECT * FROM customer 
WHERE customer.account_balance <= 200000 
    AND customer.zipcode BETWEEN 92656 AND 92677
```

## <a name="examples"></a>示例

### <a name="force-pushdown"></a>强制下推

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (FORCE EXTERNALPUSHDOWN);
```

### <a name="disable-pushdown"></a>禁用下推

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (DISABLE EXTERNALPUSHDOWN);
```

## <a name="next-steps"></a>后续步骤

有关 PolyBase 的详细信息，请参阅[什么是 PolyBase？](polybase-guide.md)。
