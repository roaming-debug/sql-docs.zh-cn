---
description: MakeValid（geography 数据类型）
title: MakeValid（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- MakeValid method (geography)
ms.assetid: f67038e3-4f62-4465-994e-e95ac27d8ada
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 7a3ca43dea611fb3fb74167032bd72f045e39b41
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88422411"
---
# <a name="makevalid-geography-data-type"></a>MakeValid（geography 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  将无效的 geography 实例转换为具有有效开放地理空间信息联盟 (OGC) 类型的有效 geography 实例。  
  
 如果输入对象针对 STIsValid() 返回 False，则 `MakeValid()` 将无效实例转换为有效实例。  
  
 这种 geography 数据类型方法支持大于半球的 FullGlobe 实例或空间实例。  
  
## <a name="syntax"></a>语法  
  
```  
  
.MakeValid ()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
 此方法可能更改 geography 实例的类型。 此外，geography 实例的点可能会略有偏移。 某些方法（如 NumPoint()）产生的结果可能会发生变化。  
  
 在无效的空间实例与赤道相交且 EnvelopeAngle() = 180 的情况下，将返回 FullGlobe 实例。 `MakeValid()` geography 数据类型方法将以最佳方式尝试返回有效的实例，但不保证结果的准确性或精确性。  
  
> [!NOTE]  
>  无效的对象可以存储在数据库中。 可对无效实例（即 STIsValid() 对其返回 False 的那些实例）执行的方法是用于检查有效性或允许导出的方法：STIsValid()、MakeValid()、STAsText()、STAsBinary()、ToString()、AsTextZM() 和 AsGml()。  
  
 此方法不精确。  
  
## <a name="examples"></a>示例  
 第一个示例创建一个与其自身重叠的无效 `LineString` 实例，并使用 `STIsValid()` 来确认该实例是无效实例。 `STIsValid()` 针对无效实例返回值 0。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(0 2, 1 1, 1 0, 1 1, 2 2)', 4326);  
SELECT @g.STIsValid();  
```  
  
 第二个示例使用 `MakeValid()` 使该实例有效并测试该实例是否的确有效。 `STIsValid()` 针对有效实例返回值 1。  
  
```  
SET @g = @g.MakeValid();  
SELECT @g.STIsValid();  
```  
  
 第三个示例验证是否已将该实例更改为有效实例。  
  
```  
SELECT @g.ToString();  
```  
  
 在此示例中，在选择 `LineString` 实例时，值将作为有效的 `MultiLineString` 实例返回。  
  
```  
MULTILINESTRING ((0 2, 1 1, 2 2), (1 1, 1 0))  
```  
  
## <a name="see-also"></a>另请参阅  
 [STIsValid（geometry 数据类型）](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [地理实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
