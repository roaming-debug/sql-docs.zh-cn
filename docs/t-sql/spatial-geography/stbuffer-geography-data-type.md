---
description: STBuffer（geography 数据类型）
title: STBuffer（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STBuffer (geography Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geography Data Type)
ms.assetid: cb4deab8-642b-44d9-b3d9-85114d64021e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1d39e58c6dd4fa648d8d4118414925777eb3535b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92038310"
---
# <a name="stbuffer-geography-data-type"></a>STBuffer（geography 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  返回一个地理对象，该对象表示所有与 geography 实例的距离小于或等于指定值的点的并集。  
  
 这种 geography 数据类型方法支持大于半球的 FullGlobe 实例或空间实例。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *distance*  
 类型为 float（在 .NET Framework 中为 double）的值，用于指定与围绕其计算缓冲区的 geography 实例的距离。  
  
 缓冲区的最大距离不能超过 0.999  π * minorAxis \* minorAxis / majorAxis（~0.999 \* 1/2 的地球圆周）或整个地球\* 。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
 STBuffer() 计算缓冲区的方式与 [BufferWithTolerance](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md) 相同：指定 tolerance = abs(distance) \* .001 且 relative = false。  
  
 负的缓冲区将删除 geography 实例的给定距离的边界内的所有点。  
  
 `STBuffer()` 在某些情况下将返回 FullGlobe 实例；例如，当缓冲区距离大于从赤道到极地的距离时，`STBuffer()` 返回 FullGlobe 实例。 缓冲区不能超过完整的地球。  
  
 在缓冲区的距离超过下列限制的 FullGlobe 实例中，此方法将引发 ArgumentException：  
  
 0.999 \* *π* * minorAxis \* minorAxis / majorAxis（~0.999 \* 1/2 地球的周长）  
  
 最大距离限制允许将缓冲区构造得尽可能灵活。  
  
 理论缓冲区与所计算的缓冲区之间的误差是 max(tolerance, extents * 1.E-7)，其中 tolerance = distance \* .001。 有关盘区的详细信息，请参阅 [geography 数据类型方法引用](./stequals-geography-data-type.md)。  
  
## <a name="examples"></a>示例  
 下面的示例创建 `LineString``geography` 实例。 然后，它使用 `STBuffer()` 返回该实例的 1 米内的区域。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STBuffer(1).ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [BufferWithTolerance（geography 数据类型）](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)   
 [地域实例上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
