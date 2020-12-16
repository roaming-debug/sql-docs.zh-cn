---
description: 为 Nearest Neighbor 查询空间数据
title: 查询最近的邻域的空间数据 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 7af4ad5d-484e-45b4-aa16-83c33b358bb6
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dd7654036e8ac2efb787a44f8f5cf7849854c570
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473158"
---
# <a name="query-spatial-data-for-nearest-neighbor"></a>为 Nearest Neighbor 查询空间数据
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  Nearest Neighbor 查询是用于空间数据的常用查询。 Nearest Neighbor 查询用于查找与特定的空间对象最接近的空间对象。 例如，网站的存储区定位器必须时常查找与客户位置最接近的存储区位置。  
  
 可以用多种有效查询格式编写 Nearest Neighbor 查询，但是要使 Nearest Neighbor 查询使用空间索引，则必须使用下面的语法。  
  
## <a name="syntax"></a>语法  
  
```  
SELECT TOP ( number )  
        [ WITH TIES ]  
        [ * | expression ]   
        [, ...]  
    FROM spatial_table_reference, ...   
        [ WITH   
            (   
                [ INDEX ( index_ref ) ]   
                [ , SPATIAL_WINDOW_MAX_CELLS = <value>]   
                [ ,... ]   
            )   
        ]  
    WHERE   
        column_ref.STDistance ( @spatial_ object )   
            {   
                [ IS NOT NULL ] | [ < const ] | [ > const ]   
                | [ <= const ] | [ >= const ] | [ <> const ] ]   
            }  
            [ AND { other_predicate } ]   
    }  
    ORDER BY column_ref.STDistance ( @spatial_ object ) [ ,...n ]  
[ ; ]  
  
```  
  
## <a name="nearest-neighbor-query-and-spatial-indexes"></a>Nearest Neighbor 查询和空间索引  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，使用 **TOP** 和 **ORDER BY** 子句对空间数据列执行 Nearest Neighbor 查询。 对于空间列数据类型， **ORDER BY** 子句包含对 `STDistance()` 方法的调用。 **TOP** 子句指示针对该查询要返回的对象数。  
  
 为了使 Nearest Neighbor 查询使用空间索引，必须满足以下要求：  
  
1.  空间索引必须存在于其中一个空间列上，并且 `STDistance()` 方法必须在 **WHERE** 和 **ORDER BY** 子句中使用该空间列。  
  
2.  **TOP** 子句不能包含 **PERCENT** 语句。  
  
3.  **WHERE** 子句必须包含 `STDistance()` 方法。  
  
4.  如果 **WHERE** 子句中有多个谓词，则必须使用 `STDistance()` AND **连词将包含** 方法的谓词连接到其他谓词。 `STDistance()` 方法不能是 **WHERE** 子句的可选部分。  
  
5.  **ORDER BY** 子句中的第一个表达式必须使用 `STDistance()` 方法。  
  
6.  `STDistance()` ORDER BY **子句中第一个** 表达式的排序顺序必须是 **ASC**。  
  
7.  必须筛选掉 `STDistance` 返回 **NULL** 的所有行。  
  
> [!WARNING]  
>   将 **geography** 或 **geometry** 数据类型作为参数的方法将返回 **NULL** ，前提是 SRID 对于这些数据类型不同。  
  
 建议将新的空间索引分割用于在 Nearest Neighbor 查询中使用的索引。 有关空间索引分割的详细信息，请参阅 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)。  
  
## <a name="example-1"></a>示例 1  
 下面的代码示例显示可以使用空间索引的 Nearest Neighbor 查询。 该示例使用 `Person.Address` 数据库中的 `AdventureWorks2016` 表。  
  
```sql  
USE AdventureWorks2016  
GO  
DECLARE @g geography = 'POINT(-121.626 47.8315)';  
SELECT TOP(7) SpatialLocation.ToString(), City FROM Person.Address  
WHERE SpatialLocation.STDistance(@g) IS NOT NULL  
ORDER BY SpatialLocation.STDistance(@g);  
```  
  
 在 SpatialLocation 列上创建一个空间索引，以便了解 Nearest Neighbor 查询如何使用空间索引。 有关创建空间索引的详细信息，请参阅 [Create, Modify, and Drop Spatial Indexes](../../relational-databases/spatial/create-modify-and-drop-spatial-indexes.md)。  
  
## <a name="example-2"></a>示例 2  
 下面的代码示例显示不能使用空间索引的 Nearest Neighbor 查询。  
  
```sql  
USE AdventureWorks2016  
GO  
DECLARE @g geography = 'POINT(-121.626 47.8315)';  
SELECT TOP(7) SpatialLocation.ToString(), City FROM Person.Address  
ORDER BY SpatialLocation.STDistance(@g);  
```  
  
 该查询缺少在语法部分中指定的窗体中使用 **的** WHERE `STDistance()` 子句，因此它无法使用空间索引。  
  
## <a name="see-also"></a>另请参阅  
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
