---
description: STAsBinary（geometry 数据类型）
title: STAsBinary（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STAsBinary_TSQL
- STAsBinary (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STAsBinary (geometry Data Type)
ms.assetid: 65353777-e3e6-461c-9504-ea4d83312692
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9acacf39cd3fdd18c7c32d3ab0220dfac09ccf3f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180179"
---
# <a name="stasbinary-geometry-data-type"></a>STAsBinary（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回 geometry 实例的开放地理空间信息联盟 (OGC) 熟知二进制 (WKB) 表示形式。  
 
## <a name="syntax"></a>语法  
  
```  
  
.STAsBinary ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：varbinary(max)  
  
 CLR 返回类型：SqlBytes  
  
## <a name="examples"></a>示例  
 以下示例根据文本创建一个从 (0,0) 到 (2,3) 的 `LineString` geometry 实例。 `STAsBinary()` 以 WKB 形式返回结果。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 3)', 0);  
SELECT @g.STAsBinary();  
```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
