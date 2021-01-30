---
description: sp_help_spatial_geometry_histogram (Transact-SQL)
title: sp_help_spatial_geometry_histogram (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_histogram
- sp_help_spatial_geometry_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_histogram
ms.assetid: 036aaf61-df3e-40f7-aa4e-62983c5a37bd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 75609570d3c021b350fc06cbaf35f32dee4bd8ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183602"
---
# <a name="sp_help_spatial_geometry_histogram-transact-sql"></a>sp_help_spatial_geometry_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  简化为空间索引键入范围框和网格参数的过程。  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_spatial_geometry_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @xmin = ] 'minx' ]   
     [ , [ @ymin = ] 'miny' ]   
     [ ,.[ @xmax = ] 'maxx' ]  
     [ , [ @ymax = ] 'maxy' ]  
     [ , [ @sample = ] 'sample' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @tabname = ] 'tabname'` 已为其指定了空间索引的表的限定名称或非限定名称。  
  
 仅当指定了限定表时才需要引号。 如果提供的是完全限定名称（包括数据库名称），则数据库名称必须是当前数据库的名称。 *tabname* 的值为 **sysname**，无默认值。  
  
`[ @colname = ] 'colname'` 指定的空间列的名称。 *colname* 是一个 **sysname**，无默认值。  
  
`[ @resolution = ] 'resolution'` 边界框的分辨率。 有效值为 10 到 5000。 *解析* 是 **tinyint**，无默认值。  
  
`[ @xmin = ] 'xmin'` 是 X 的最小范围框属性。 *xmin* 是一个 **float**，无默认值。  
  
`[ @ymin = ] 'ymin'` 为 Y 的最小范围框属性。 *ymin* 是一个 **float**，无默认值。  
  
`[ @xmax = ] 'xmax'` 是 X 最大范围框属性。 *xmax* 是一个 **float**，无默认值。  
  
`[ @ymax = ] 'ymax'` 是最大值边界框属性。 *ymax* 是一个 **float**，无默认值。  
  
`[ @sample = ] 'sample'` 使用的表的百分比。 有效值为0至100。 *示例* 是一个 **浮点数**。 默认值为100。  
  
## <a name="property-valuereturn-value"></a>属性值/返回值  
 返回表值。 下面的网格描述表的列内容。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|表示每个单元的唯一 ID，从 1 开始计数。|  
|**芯**|**geometry**|是表示每个单元的矩形多边形。 该单元形状与用于空间索引的单元形状相同。|  
|**row_count**|**bigint**|指示接触或包含单元的空间对象数 。|  
  
## <a name="permissions"></a>权限  
 用户必须是 **公共** 角色的成员。 需要服务器和对象的 READ ACCESS 权限。  
  
## <a name="remarks"></a>备注  
 SSMS 空间选项卡显示结果的图形表示形式。 您可以针对空间窗口查询结果，以获取近似的结果项数。 表中的对象可能涵盖多个单元，因此单元之和可能大于实际对象数。  
  
 可能会向结果集中添加一个附加行，用于保存在范围框之外或与范围框边界接触的对象数。 该行的 **cellid** 为0，该行的 **单元格** 包含表示边界框的 **LineString** 。 此行表示在范围框之外的整个空间。  
  
## <a name="examples"></a>示例  
 下面的示例创建一个示例表，然后对该表调用 **sp_help_spatial_geometry_histogram** 。  
  
 `USE AdventureWorksDW2012`  
  
 `GO`  
  
 `-- Set database compatibility for circular arc segments`  
  
 `ALTER DATABASE AdventureWorksDW2012`  
  
 `SET COMPATIBILITY_LEVEL = 110;`  
  
 `GO`  
  
 `-- Create table to execute sp_help_spatial_geometry_histogram on`  
  
 `CREATE TABLE TownSites`  
  
 `(`  
  
 `Location geometry NULL,`  
  
 `SiteName nvarchar(50) NULL`  
  
 `)`  
  
 `GO`  
  
 `-- Insert site data into table`  
  
 `DECLARE @g geometry;`  
  
 `SET @g = geometry::Parse('POINT(0 0)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Booth Map';`  
  
 `SET @g = geometry::Parse('POLYGON((1 1, 1 2, 2 2, 2 1, 1 1))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Town Hall';`  
  
 `SET @g = geometry::Parse('CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-1 0, 0 -1, 1 0),(1 0, 1 2, -1 0)))');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Park';`  
  
 `SET @g = geometry::Parse('CIRCULARSTRING(1 5, 2 2, 5 1)');`  
  
 `INSERT INTO TownSites(Location, SiteName)`  
  
 `SELECT @g, N'Main Road';`  
  
 `-- Call proc to see data within bounding box`  
  
 `EXEC sp_help_spatial_geometry_histogram @tabname = TownSites, @colname = Location, @resolution = 64, @xmin = -2, @ymin = -2, @xmax = 3, @ymax = 3, @sample = 100;`  
  
 `GO`  
  
 `DROP TABLE TownSites;`  
  
 `GO`  
  
## <a name="see-also"></a>另请参阅  
 [空间索引存储过程 &#40;Transact-sql&#41;](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
