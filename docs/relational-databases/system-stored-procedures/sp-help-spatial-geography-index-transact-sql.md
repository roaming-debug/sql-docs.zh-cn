---
description: sp_help_spatial_geography_index (Transact-SQL)
title: sp_help_spatial_geography_index (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_index
- sp_help_spatial_geography_index_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_index procedure
ms.assetid: c9bf5675-eafc-4d71-bfdb-da963384fa0c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: adccb4ff123d5ea91691d57c03556c51441abed3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212050"
---
# <a name="sp_help_spatial_geography_index-transact-sql"></a>sp_help_spatial_geography_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 **地理** 空间索引的一组指定属性的名称和值。 以表格式返回结果。 可以选择是返回索引的一组核心属性还是返回索引的所有属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_spatial_geography_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ] 'verboseoutput' ]   
     [ , [ @query_sample = ] 'query_sample' ]   
```  
  
## <a name="arguments"></a>参数  
 请参阅 [空间索引存储过程的参数和属性](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)。  
  
## <a name="properties"></a>属性  
 请参阅 [空间索引存储过程的参数和属性](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)。  
  
## <a name="permissions"></a>权限  
 必须为用户分配一个 PUBLIC 角色以便能够访问该过程。 需要服务器和对象的 READ ACCESS 权限。  
  
## <a name="remarks"></a>备注  
  
## <a name="example"></a>示例  
 下面的示例使用 `sp_help_spatial_geography_index` 来调查 **\@ qs** 中给定查询示例的表 **geography_col** 上定义的 **地理** 空间索引 **SIndx_SpatialTable_geography_col2** 。 此示例只返回指定索引的核心属性。  
  
```  
declare @qs geography  
        ='POLYGON((-90.0 -180, -90 180.0, 90 180.0, 90 -180, -90 -180.0))';  
exec sp_help_spatial_geography_index 'geography_col', 'SIndx_SpatialTable_geography_col2', 0, @qs;  
```  
  
 **Geography** 实例的边界框是整个地球。  
  
## <a name="requirements"></a>要求  
  
## <a name="see-also"></a>另请参阅  
 [空间索引存储过程](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geography_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)   
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
