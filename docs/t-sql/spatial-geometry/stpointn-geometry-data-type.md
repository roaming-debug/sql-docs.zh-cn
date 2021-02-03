---
description: STPointN（geometry 数据类型）
title: STPointN（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STPointN_TSQL
- STPointN (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STPointN (geometry Data Type)
ms.assetid: 8f0bb3b7-5cd9-42c2-b9f8-f04628653bd0
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 78db87953b38208585bd9ea68c75766cfd3765b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184635"
---
# <a name="stpointn-geometry-data-type"></a>STPointN（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回 **geometry** 实例中的指定点。
  
## <a name="syntax"></a>语法  
  
```  
  
.STPointN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 一个 **int** 表达式，其值介于 1 和 **geometry** 实例中的点数之间。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
 开放地理空间信息联盟 (OGC) 类型：Point  
  
## <a name="remarks"></a>备注  
 如果 **geometry** 实例是用户创建的，则 `STPointN()` 返回由 *expression* 通过按照点的原始输入顺序对点进行排序而指定的点。  
  
 如果 **geometry** 实例是系统构建的，则 `STPointN()` 返回由 *expression* 通过按照点的输出顺序对所有点进行排序而指定的点，点的排序顺序为：首先按几何图形，然后按几何图形中的环（如果适用），最后按环中的点。 此顺序是确定的。  
  
 如果使用小于 1 的值来调用此方法，则会引发 ArgumentOutOfRangeException。  
  
 如果使用大于实例中点数的值来调用此方法，则返回 Null。  
  
## <a name="examples"></a>示例  
 下面的示例创建 `LineString` 实例，并使用 `STPointN()` 检索实例说明中的第二个点。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STPointN(2).ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

