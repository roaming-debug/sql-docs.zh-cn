---
description: MultiPoint
title: MultiPoint | Microsoft Docs
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- MultiPoint geometry subtype [SQL Server]
ms.assetid: 2aaab211-3aba-4dbd-90b7-095d997b1f62
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea276d706d2c1e95f352755cb0b794861e45b42a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475418"
---
# <a name="multipoint"></a>MultiPoint
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  MultiPoint 是零个点或更多个点的集合。 **MultiPoint** 实例的边界为空。  
  
## <a name="examples"></a>示例  

### <a name="example-a"></a>示例 A。
下面的示例创建一个 `geometry MultiPoint` 实例，该实例的 SRID 为 23 且包含两个点：一个点的坐标为 (2, 3)，另一个点的坐标为 (7, 8)，Z 值为 9.5。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT((2 3), (7 8 9.5))', 23);  
```  
  
### <a name="example-b"></a>示例 B。 
以下示例使用 `STMPointFromText()` 表示 `MultiPoint` 实例。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STMPointFromText('MULTIPOINT((2 3), (7 8 9.5))', 23);  
```  
  
### <a name="example-c"></a>示例 C.
下面的示例使用方法 `STGeometryN()` 来检索有关集合中第一个点的说明。  
  
```sql  
SELECT @g.STGeometryN(1).STAsText();  
```  
  
## <a name="see-also"></a>另请参阅  
 [点](../../relational-databases/spatial/point.md)   
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
