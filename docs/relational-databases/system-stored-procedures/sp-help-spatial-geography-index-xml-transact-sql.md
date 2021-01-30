---
description: sp_help_spatial_geography_index_xml (Transact-SQL)
title: sp_help_spatial_geography_index_xml (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_index_xml_TSQL
- sp_help_spatial_geography_index_xml
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_index_xml procedure
ms.assetid: 821d4127-3ce5-4474-8561-043404a20d81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81bad34a1eb0d45eafb84bacf1cc65871e73e2b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158428"
---
# <a name="sp_help_spatial_geography_index_xml-transact-sql"></a>sp_help_spatial_geography_index_xml (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 **地理** 空间索引的一组指定属性的名称和值。 可以选择是返回索引的一组核心属性还是返回索引的所有属性。  
  
 在一个显示了所选属性的名称和值的 XML 片段中返回结果。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_spatial_geography_index_xml [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ] 'verboseoutput' ]   
     [ , [ @query_sample = ] 'query_sample' ]   
     [ ,.[ @xml_output = ] 'xml_output' ]   
```  
  
## <a name="arguments"></a>参数  
 请参阅 [空间索引存储过程的参数和属性](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)。  
  
## <a name="properties"></a>属性  
 请参阅 [空间索引存储过程的参数和属性](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md)。  
  
## <a name="permissions"></a>权限  
 必须为用户分配一个 PUBLIC 角色以便能够访问该过程。 需要服务器和对象的 READ ACCESS 权限。  
  
## <a name="remarks"></a>备注  
 包含 NULL 值的属性未包含在返回集中。  
  
## <a name="example"></a>示例  
 下面的示例使用 `sp_help_spatial_geography_index_xml` 来调查 **\@ qs** 中给定查询示例的表 **geography_col** 上定义的空间索引 **SIndx_SpatialTable_geography_col2** 。 此示例在一个显示了所选属性的名称和值的 XML 片段中返回指定索引的核心属性。  
  
 然后，对结果集运行 [XQuery](../../xquery/xquery-basics.md) ，返回特定属性。  
  
```  
declare @qs geography  
        ='POLYGON((-90.0 -180, -90 180.0, 90 180.0, 90 -180, -90 -180.0))';  
declare @x xml;  
exec sp_help_spatial_geography_index_xml 'geography_col', 'SIndx_SpatialTable_geography_col2', 0, @qs, @x output;  
select @x.value('(/Primary_Filter_Efficiency/text())[1]', 'float');  
```  
  
 与 [sp_help_spatial_geography_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)类似，此存储过程提供了对 **地理** 空间索引的属性进行更简单的编程访问，并在 XML 中报告结果集。  
  
 **地理** 的边界框是整个地球。  
  
## <a name="requirements"></a>要求  
  
## <a name="see-also"></a>另请参阅  
 [空间索引存储过程](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geography_index](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)   
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)   
 [XQuery 基础知识](../../xquery/xquery-basics.md)   
 [XQuery 语言参考](../../xquery/xquery-language-reference-sql-server.md)  
  
