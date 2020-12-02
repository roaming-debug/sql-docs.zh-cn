---
description: IsValidDetailed（geometry 数据类型）
title: IsValidDetailed（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- IsValidDetailed geometry
ms.assetid: 5a31e88a-ad7b-4ef7-b773-e2571f1cb3aa
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f57155c8724d8cb27aaf06eaf491e11298ad61c2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88497030"
---
# <a name="isvaliddetailed-geometry-datatype"></a>IsValidDetailed（geometry 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

返回一条消息，该消息帮助您确定空间对象无效的问题。 当该对象无效时，只返回第一个错误。 当该对象有效时，则返回值 24400。
  
## <a name="syntax"></a>语法  
  
```  
  
.IsValidDetailed()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：nvarchar(max)  
  
 CLR 返回类型：string  
  
## <a name="remarks"></a>备注  
 下表包含可能的返回值：  
  
|返回值|说明|  
|------------------|-----------------|  
|24400|有效|  
|24401|无效，原因未知。|  
|24402|因为点 {0} 是孤立点，这在此类型的对象中是无效的，所以无效。|  
|24403|因为某一对的多边形边重叠，所以无效。|  
|24404|因为某个多边形环 {0} 与自己或某个其他环相交，所以无效。|  
|24405|因为某个多边形环与自己或某个其他环相交，所以无效。|  
|24406|因为曲线 {0} 退化到点，所以无效。|  
|24407|因为多边形环 {0} 在点 {1} 处纵弯曲成一条直线，所以无效。|  
|24408|因为多边形环 {0} 未闭合，所以无效。|  
|24409|因为多边形环 {0} 的某个部分处于某个多边形的内部，所以无效。|  
|24410|因为环 {0} 是某个多边形的第一个环、但它不是该多边形的外环，所以无效。|  
|24411|因为环 {0} 处于其多边形的外环 {1} 的外部，所以无效。|  
|24412|因为具有环 {0} 和 {1} 的多边形的内部不相连，所以无效。|  
|24413|因为曲线 {0} 中存在两条重叠的边，所以无效。|  
|24414|因为曲线 {0} 的某条边与曲线 {1} 的某条边重叠，所以无效。|  
|24415|因为某个多边形具有无效的环结构，所以无效。|  
|24416|因为在曲线 {0} 中，从点 {1} 开始的边或者是直线，或者是具有对跖端点的退化弧，所以无效。|  
  
## <a name="examples"></a>示例  
 以下无效空间对象示例阐释了 IsValidDetailed() 方法的行为。  
  
```sql  
DECLARE @p GEOMETRY = 'Polygon((2 2, 4 4, 4 2, 2 4, 2 2))'  
SELECT @p.IsValidDetailed()  
--Returns: 24404: Not valid because polygon ring (1) intersects itself or some other ring.  
```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

