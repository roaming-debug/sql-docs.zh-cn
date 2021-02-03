---
description: BufferWithTolerance（geometry 数据类型）
title: BufferWithTolerance（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
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
- BufferWithTolerance (geometry Data Type)
ms.assetid: 7049d37a-3e72-4e93-87a1-c96a6f0e2b99
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b84197d386d92da8593449142ae3e13b54b8a605
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205798"
---
# <a name="bufferwithtolerance-geometry-data-type"></a>BufferWithTolerance（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回一个几何图形对象，该对象表示所有与 **geometry** 实例的距离小于或等于指定值的点值的并集，允许存在指定的公差。
  
## <a name="syntax"></a>语法  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *distance*  
 一个 **float** 表达式，用于指定与围绕其计算缓冲区的 **geometry** 实例的距离。  
  
 tolerance  
 一个 float 表达式，用于指定缓冲区距离的公差。  
  
 *公差* 是指理想的缓冲区距离与返回的线性近似值之间的最大偏差。  
  
 例如，点的理想缓冲区距离为圆圈，但是这必须与多边形近似。 公差越小，多边形具有的点就越多，这将增加结果的复杂度，但可减少错误。  
  
 *relative*  
 一个指定 tolerance 值是相对值还是绝对值的 bit 值。 如果为“TRUE”或 1，则公差为相对值，并按 *tolerance* 参数与该实例的边框直径之间的乘积进行计算。 如果为“FALSE”或 0，则公差为绝对值，并且 tolerance 值为理想缓冲区距离与返回的线性近似值之间的绝对最大偏差。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="exceptions"></a>例外  
 *tolerance* 参数必须大于零。 如果 *tolerance* <= 0，则引发 `System.ArgumentOutOfRangeException`。  
  
> [!NOTE]  
>  由于 *tolerance* 的类型为 **float**，如果指定的 tolerance 值很小，则可能会由于浮点类型舍入问题而引发 `System.Runtime.InteropServices.COMException`。  
  
## <a name="remarks"></a>备注  
 如果 distance > 0，则返回 Polygon 或 MultiPolygon 实例。  
  
> [!NOTE]  
>  由于 distance 的类型为 float，因此，极小的值可能在计算中等于零。 如果发生这种情况，则会返回执行调用的 geometry 实例的副本。 请参阅 [float 和 real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md)。  
  
 如果 distance = 0，则会返回执行调用的 geometry 实例的副本。  
  
 如果 distance < 0  
  
-   当实例维度为 0 或 1 时，将返回空的 **GeometryCollection** 实例。  
  
-   当实例维度为 2 或更大时，将返回负缓冲区。  
  
    > [!NOTE]  
    >  负缓冲区还可能会创建空的 GeometryCollection 实例。  
  
 负缓冲区将删除 **geometry** 实例的给定距离的边界内的所有点。  
  
 理论缓冲区与计算缓冲区之间的误差为 max(tolerance, extents \* 1.E-7)，其中 tolerance 是 tolerance 参数的值。 有关盘区的详细信息，请参阅 [geometry 数据类型方法引用](./spatial-types-geometry-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 下面的示例创建 `Point` 实例，并使用 `BufferWithTolerance()` 获取环绕该实例的大致缓冲区。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(3 3)', 0);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [STBuffer（geometry 数据类型）](../../t-sql/spatial-geometry/stbuffer-geometry-data-type.md)   
 [几何图形实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
