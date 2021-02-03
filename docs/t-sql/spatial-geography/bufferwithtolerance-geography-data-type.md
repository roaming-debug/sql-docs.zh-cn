---
description: BufferWithTolerance（geography 数据类型）
title: BufferWithTolerance（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithTolerance_TSQL
- BufferWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- BefferWithTolerance method
ms.assetid: f1783e6b-0f17-464f-b1c7-1c3f7d8aa042
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 51be64effcbbb75abe7830949d8bdf83b5637307
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205046"
---
# <a name="bufferwithtolerance-geography-data-type"></a>BufferWithTolerance（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回表示所有点值的并集的几何对象，这些点到 geography 实例的距离小于或等于指定值，允许存在指定公差。  
  
这种 geography 数据类型方法支持大于半球的 FullGlobe 实例或空间实例。  
  
## <a name="syntax"></a>语法  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
_distance_  
一个 float 表达式，用于指定与围绕其计算缓冲区的 geography 实例的距离。  
  
缓冲区的最大距离不得超过 0.999 \* _π_  * minorAxis \* minorAxis / majorAxis（~0.999 \* 1/2 的地球圆周）或整个地球。  
  
tolerance  
一个 float 表达式，用于指定缓冲区距离的公差。  
  
理想的缓冲区距离与返回的线性近似之间的最大偏差是 tolerance 值。  
  
例如，点的理想缓冲区距离为圆圈，但是此距离必须与多边形近似。 容差越小，多边形的点就越多。 此结果虽增加了结果的复杂性，但最大限度地减少错误。  
  
最小限制为距离的 0.1%，任何小于此限制的公差都将向上舍入到此最小限制。  
  
_relative_  
一个指定 tolerance 值是相对值还是绝对值的 bit 值。 如果值为“TRUE”或“1”，容差是相对的。 此值是 tolerance 参数与椭圆体的角范围 \* 赤道半径的乘积。 如果值为“FALSE”或“0”，容差是绝对的。 tolerance 值是理想的缓冲区距离与返回的线性近似之间的最大绝对偏差。  
  
## <a name="return-types"></a>返回类型  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
如果 distance 不是数字 (NAN)，或如果 distance 是正/负无穷大，此方法会抛出 ArgumentException。  如果 tolerance 为零 (0)，而不是数字 (NaN)、负数或正/负无穷大，此方法也会抛出 ArgumentException。  
  
`STBuffer()` 在某些情况下将返回 FullGlobe 实例；例如，当缓冲区距离大于从赤道到两极的距离时，`STBuffer()` 在两极返回 FullGlobe 实例。  
  
在缓冲区的距离超过下列限制的 FullGlobe 实例中，此方法将引发 ArgumentException：  
  
0.999 \* _π_ * minorAxis \* minorAxis / majorAxis（~0.999 \* 1/2 地球的周长）  
  
理论缓冲区与计算缓冲区之间的误差为 max(tolerance, extents \* 1.E-7)，其中 tolerance 是 tolerance 参数的值。 有关盘区的详细信息，请参阅 [geography 数据类型方法引用](./stequals-geography-data-type.md)。  
  
此方法不精确。  
  
## <a name="examples"></a>示例  
下面的示例创建 `Point` 实例，并使用 `BufferWithTolerance()` 获取环绕该实例的大致缓冲区。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
[STBuffer（geography 数据类型）](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)   
[地理实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
