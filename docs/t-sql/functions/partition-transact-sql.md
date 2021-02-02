---
description: $PARTITION (Transact-SQL)
title: $PARTITION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- $partition_TSQL
- $partition
dev_langs:
- TSQL
helpviewer_keywords:
- $PARTITION function
- partitions [SQL Server], numbers
ms.assetid: abc865d0-57a8-49da-8821-29457c808d2a
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: f9175fbea9320b7401946693092f6a342560dae5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99100758"
---
# <a name="partition-transact-sql"></a>$PARTITION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中任何指定的分区函数返回分区号，一组分区列值将映射到该分区号中。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
[ database_name. ] $PARTITION.partition_function_name(expression)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *database_name*  
 包含分区函数的数据库的名称。  
  
 partition_function_name  
 对其应用一组分区列值的任何现有分区函数的名称。  
  
 *expression*  
 其数据类型必须匹配或可隐式转换为其对应分区列数据类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 表达式也可以是当前参与 partition_function_name 的分区列的名称。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 $PARTITION 返回从 1 到分区函数的分区数之间的 int 值。  
  
 $PARTITION 将针对任何有效值返回分区号，无论此值当前是否存在于使用分区函数的分区表或索引中。  
  
## <a name="examples"></a>示例  
  
### <a name="a-getting-the-partition-number-for-a-set-of-partitioning-column-values"></a>A. 获得一组分区列值的分区号  
 以下示例将创建一个将表或索引划分为四个分区的分区函数 `RangePF1`。 $PARTITION 用于确定将表示 `10` 的分区列的值 `RangePF1` 置于表的第 1 分区。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PARTITION FUNCTION RangePF1 ( INT )  
AS RANGE FOR VALUES (10, 100, 1000) ;  
GO  
SELECT $PARTITION.RangePF1 (10) ;  
GO  
```  
  
### <a name="b-getting-the-number-of-rows-in-each-nonempty-partition-of-a-partitioned-table-or-index"></a>B. 获取分区表或索引的每个非空分区的行数  
 以下示例将返回包含数据的表 `TransactionHistory` 的每个分区的行数。 `TransactionHistory` 表使用分区函数 `TransactionRangePF1`，并在 `TransactionDate` 列上进行分区。  
  
 若要执行此示例，必须首先对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库运行 PartitionAW.sql 脚本。 有关详细信息，请参阅 [PartitioningScript](https://go.microsoft.com/fwlink/?LinkId=201015)。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT $PARTITION.TransactionRangePF1(TransactionDate) AS Partition,   
COUNT(*) AS [COUNT] FROM Production.TransactionHistory   
GROUP BY $PARTITION.TransactionRangePF1(TransactionDate)  
ORDER BY Partition ;  
GO  
```  
  
### <a name="c-returning-all-rows-from-one-partition-of-a-partitioned-table-or-index"></a>C. 返回分区表或索引的一个分区的所有行  
 以下示例将返回表 `5` 第 `TransactionHistory` 分区的所有行。  
  
> [!NOTE]  
>  若要执行此示例，必须首先对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库运行 PartitionAW.sql 脚本。 有关详细信息，请参阅 [PartitioningScript](https://go.microsoft.com/fwlink/?LinkId=201015)。  
  
```sql  
SELECT * FROM Production.TransactionHistory  
WHERE $PARTITION.TransactionRangePF1(TransactionDate) = 5 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)  
  
  
