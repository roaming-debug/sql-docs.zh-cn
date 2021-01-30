---
description: sp_help_spatial_geography_histogram (Transact-SQL)
title: sp_help_spatial_geography_histogram (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geography_histogram_TSQL
- sp_help_spatial_geography_histogram
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geography_histogram
ms.assetid: 5c5bd319-055d-4cd6-8c5a-06354cc056cc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3554c8b385a14f588c4f7f8dd6248ba36ad7b16d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212067"
---
# <a name="sp_help_spatial_geography_histogram-transact-sql"></a>sp_help_spatial_geography_histogram (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  简化为空间索引键入网格参数的过程。  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_spatial_geography_histogram [ @tabname =] 'tabname'   
     [ , [ @colname = ] 'columnname' ]   
     [ , [ @resolution = ] 'resolution' ]  
     [ , [ @sample = ] 'tablesample' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @tabname = ] 'tabname'` 已为其指定了空间索引的表的限定名称或非限定名称。  
  
 仅当指定了限定表时才需要引号。 如果提供的是完全限定名称（包括数据库名称），则数据库名称必须是当前数据库的名称。 *tabname* 的值为 **sysname**，无默认值。  
  
`[ @colname = ] 'columnname'` 指定的空间列的名称。 *columnname* 是 **sysname**，无默认值。  
  
`[ @resolution = ] 'resolution'` 边界框的分辨率。 有效值为 10 到 5000。 *解析* 是 **tinyint**，无默认值。  
  
`[ @sample = ] 'sample'` 使用的表的百分比。 有效值为0至100。 *tablesample* 为 **float**。 默认值为100。  
  
## <a name="property-valuereturn-value"></a>属性值/返回值  
 返回表值。 下面的网格描述表的列内容。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cellid**|**int**|表示每个单元的唯一 ID，起始计数为1。|  
|**芯**|**geography**|是表示每个单元的矩形多边形。 该单元形状与用于空间索引的单元形状相同。|  
|**row_count**|**bigint**|指示接触或包含单元的空间对象数 。|  
  
## <a name="permissions"></a>权限  
 用户必须是 **公共** 角色的成员。 需要服务器和对象的 READ ACCESS 权限。  
  
## <a name="remarks"></a>备注  
 SSMS 空间选项卡显示结果的图形表示形式。 您可以针对空间窗口查询结果，以获取近似的结果项数。  
  
> [!NOTE]  
>  表中的对象可能涵盖多个单元，因此表中单元之和可能大于实际对象数。  
  
 **Geography** 类型的边界框是整个地球。  
  
## <a name="examples"></a>示例  
 下面的示例对 `Person.Address` 数据库中的表调用 sp_help_spatial_geography_histogram [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 。  
  
```  
EXEC sp_help_spatial_geography_histogram @tabname = Person.Address, @colname = SpatialLocation, @resolution = 64, @sample = 30;  
```  
  
## <a name="see-also"></a>另请参阅  
 [空间索引存储过程 &#40;Transact-sql&#41;](./spatial-index-stored-procedures-arguments-and-properties.md)  
  
