---
description: APPROX_COUNT_DISTINCT (Transact-SQL)
title: APPROX_COUNT_DISTINCT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPROX_COUNT_DISTINCT
dev_langs:
- TSQL
author: joesackmsft
ms.author: josack
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7cb07f1448835b4f8af9384b895776d896134f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202267"
---
# <a name="approx_count_distinct-transact-sql"></a>APPROX_COUNT_DISTINCT (Transact-SQL)

[!INCLUDE [sqlserver2019-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi-asa.md)]

此函数返回组中唯一非空值的近似数。 
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
APPROX_COUNT_DISTINCT ( expression )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*expression*  
任意类型（“**image**” 、“**sql_variant**” 、“**ntext**” 或“**text**” 除外）的 [表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 

## <a name="return-types"></a>返回类型
 **bigint**  
  
## <a name="remarks"></a>备注  
`APPROX_COUNT_DISTINCT( expression )` 计算组中每行的表达式，并返回组中唯一非空值的近似数。 此函数旨在跨响应速度比绝对精度更为关键的大型数据集进行聚合。  

`APPROX_COUNT_DISTINCT` 专用于大数据方案，更适合以下情形：
- 访问包含数百万行或更多行的数据集，且
- 聚合包含多个非重复值的一个或多个列

此函数实现可保证最多 2% 的错误率，概率在 97% 内。 

与详尽 COUNT DISTINCT 操作相比，`APPROX_COUNT_DISTINCT` 需要的内存更少。  与精确 COUNT DISTINCT 操作相比，鉴于 `APPROX_COUNT_DISTINCT` 占用的内存更少，因此它不太可能会将内存溢出到磁盘。 若要详细了解用于实现此目的的算法，请参阅 [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog)。

> [!NOTE]
> 使用排序规则敏感字符串，APPROX_COUNT_DISTINCT 使用二进制匹配，生成的结果与在有 BIN 排序规则（而不是 BIN2）的情况下一致。 
  
## <a name="examples"></a>示例  
  
### <a name="a-using-approx_count_distinct"></a>A. 使用 APPROX_COUNT_DISTINCT 
此示例返回订单表中不同订单键的近似数。
  
```sql
SELECT APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders;
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Approx_Distinct_OrderKey
------------------------
15164704
```
  
### <a name="b-using-approx_count_distinct-with-group-by"></a>B. 结合使用 APPROX_COUNT_DISTINCT 和 GROUP BY 
此示例按订单状态返回订单表中不同订单键的近似数。 
  
```sql
SELECT O_OrderStatus, APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders
GROUP BY O_OrderStatus
ORDER BY O_OrderStatus; 
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
O_OrderStatus                                                    Approx_Distinct_OrderKey
---------------------------------------------------------------- ------------------------
F                                                                7397838
O                                                                7387803
P                                                                388036
```
    
## <a name="see-also"></a>另请参阅
[聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT (Transact-SQL)](../../t-sql/functions/count-transact-sql.md) 
