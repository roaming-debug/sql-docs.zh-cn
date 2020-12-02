---
description: EnvelopeCenter（geography 数据类型）
title: EnvelopeCenter（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EnvelopeCenter
- EnvelopeCenter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeCenter method
ms.assetid: dee9d807-faad-45b8-b3f3-7e8aa7d07147
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: c6e7173111dd48e54c85410b89d5a450adfe1169
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124373"
---
# <a name="envelopecenter-geography-data-type"></a>EnvelopeCenter（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回可用作 geography 实例的边界圆圆心的点。  
  
实例中的每个点都被描述为矢量。 为了确定边界圆，矢量从地球中心延伸到地球表面上的点。 边界圆的中心点的计算方法为，计算所有矢量的平均值。 对于闭环，无论是在 Polygon 实例中，还是在 LineString 实例中，第一个点和最后一个点都只使用一次。  
  
这种 geography 数据类型方法支持大于半球的 FullGlobe 实例或空间实例   。  
  
## <a name="syntax"></a>语法  
  
```  
  
EnvelopeCenter( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
此方法返回一个点。 在与 `EnvelopeAngle()` 一起使用时，`EnvelopeCenter()` 返回 geography 实例的一个边框圆。  
  
> [!NOTE]  
>  `EnvelopeCenter()` 返回 geography 实例的一个边框圆，但是不保证结果能够产生最小的边框圆。 与此相反，geometry 数据类型方法 `STEnvelope()` 在应用于 geometry 实例时可以保证返回最小的边框圆。  
  
在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本中，将表示此实例信封的圆的中心作为“点”返回。 对于所有根据 `EnvelopeAngle()` = 180 定义的大型对象，`EnvelopeCenter()` 将返回 (90,0)。  
  
此方法不精确。  
  
## <a name="examples"></a>示例  
  
```  
DECLARE @g geography = 'LINESTRING(-120 45, -120 0, -90 0)';  
SELECT @g.EnvelopeCenter().ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
[Geography 实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
[EnvelopeAngle（geography 数据类型）](../../t-sql/spatial-geography/envelopeangle-geography-data-type.md)  
  
  
