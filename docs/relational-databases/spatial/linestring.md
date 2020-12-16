---
description: LineString
title: LineString | Microsoft Docs
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- LineString geometry subtype [SQL Server]
- geometry subtypes [SQL Server]
ms.assetid: e50d0b86-8b31-4285-be71-ad05c7712cbd
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0e9a48295949e973b4e371c68f23a000f0308c69
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462968"
---
# <a name="linestring"></a>LineString
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  **LineString** 是一个一维对象，表示一系列点和连接这些点的线段。  
  
## <a name="linestring-instances"></a>LineString 实例  
 下图显示了 **LineString** 实例的示例。  
  
 ![几何 LineString 实例的示例](../../relational-databases/spatial/media/linestring.gif "几何 LineString 实例的示例")  
  
如图中所示：  
  
-   图 1 显示的是一个简单、非闭合的 **LineString** 实例。  
  
-   图 2 显示的是一个不简单、非闭合的 **LineString** 实例。  
  
-   图 3 显示的是一个闭合、简单的 **LineString** 实例，因此是一个环。  
  
-   图 4 显示的是一个闭合、不简单的 **LineString** 实例，因此不是一个环。  
  
### <a name="accepted-instances"></a>已接受的实例  
可以将已接受的 **LineString** 实例输入到一个几何图形变量中，但它们可能不是有效的 **LineString** 实例。 必须满足以下条件， **LineString** 实例才能被接受。 该实例必须由至少两个点组成，或者该实例必须为空。 下面是一些已接受的 LineString 实例。  
  
```sql  
DECLARE @g1 geometry = 'LINESTRING EMPTY';  
DECLARE @g2 geometry = 'LINESTRING(1 1,2 3,4 8, -6 3)';  
DECLARE @g3 geometry = 'LINESTRING(1 1, 1 1)';  
```  
  
`@g3` 显示 **LineString** 实例可被接受，但无效。  
  
下面的 **LineString** 实例不可接受。 它将引发 `System.FormatException`。  
  
```sql  
DECLARE @g geometry = 'LINESTRING(1 1)';  
```  
  
### <a name="valid-instances"></a>有效实例  
必须满足以下条件，LineString 实例才是有效的。  
  
1.  **LineString** 实例必须是已接受的实例。  
2.  如果 LineString 实例不为空，则它必须包含至少两个非重复点。  
3.  在两个或更多连续点的间隔范围内， **LineString** 实例不能自身重叠。  
  
以下是有效的 **LineString** 实例。  
  
```sql  
DECLARE @g1 geometry= 'LINESTRING EMPTY';  
DECLARE @g2 geometry= 'LINESTRING(1 1, 3 3)';  
DECLARE @g3 geometry= 'LINESTRING(1 1, 3 3, 2 4, 2 0)';  
DECLARE @g4 geometry= 'LINESTRING(1 1, 3 3, 2 4, 2 0, 1 1)';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid(), @g4.STIsValid();  
```  
  
以下是无效的 **LineString** 实例。  
  
```sql  
DECLARE @g1 geometry = 'LINESTRING(1 4, 3 4, 2 4, 2 0)';  
DECLARE @g2 geometry = 'LINESTRING(1 1, 1 1)';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
> [!WARNING]  
> **LineString** 重叠的检测基于不精确的浮点计算。  
  
## <a name="examples"></a>示例  
### <a name="example-a"></a>示例 A。    
下面的示例说明如何创建一个包含三个点且 SRID 为 0 的 `geometry``LineString` 实例：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(1 1, 2 4, 3 9)', 0);  
```  
  
### <a name="example-b"></a>示例 B。   
此 `LineString` 实例中的每个点都可以包含 Z（仰角）和 M（度量）值。 下面这个示例向上例中创建的 `LineString` 实例添加了 M 值。 M 和 Z 可以为 Null 值。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(1 1 NULL 0, 2 4 NULL 12.3, 3 9 NULL 24.5)', 0);  
```  
  
### <a name="example-c"></a>示例 C.   
下面的示例说明如何创建一个包含两个相同点的 `geometry LineString` 实例。 对 `IsValid` 的调用指示 **LineString** 实例无效，并且对 `MakeValid` 的调用会将 **LineString** 实例转换为 **Point**。  
  
```sql  
DECLARE @g geometry  
SET @g = geometry::STGeomFromText('LINESTRING(1 3, 1 3)',0);  
IF @g.STIsValid() = 1  
  BEGIN  
     SELECT @g.ToString() + ' is a valid LineString.';    
  END  
ELSE  
  BEGIN  
     SELECT @g.ToString() + ' is not a valid LineString.';  
     SET @g = @g.MakeValid();  
     SELECT @g.ToString() + ' is a valid Point.';    
  END  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```  
LINESTRING(1 3, 1 3) is not a valid LineString  
POINT(1 3) is a valid Point.  
```  
  
## <a name="see-also"></a>另请参阅  
 [STLength（geometry 数据类型）](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STStartPoint（geometry 数据类型）](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)   
 [STEndpoint（geometry 数据类型）](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)   
 [STPointN（geometry 数据类型）](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)   
 [STNumPoints（geometry 数据类型）](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STIsRing（geometry 数据类型）](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)   
 [STIsClosed（geometry 数据类型）](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STPointOnSurface（geometry 数据类型）](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)   
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
