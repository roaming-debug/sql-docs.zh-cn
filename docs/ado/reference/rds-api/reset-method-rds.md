---
description: Reset 方法 (RDS)
title: 在 RDS)  (重置方法 |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Reset method [ADO]
ms.assetid: 3957197a-f543-4d6b-9e11-67a77c2063b7
author: rothja
ms.author: jroth
ms.openlocfilehash: b4d1fa7427fd4c55ea8c53702ef4ac6a4a68b9b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166136"
---
# <a name="reset-method-rds"></a>Reset 方法 (RDS)
基于指定的排序和筛选器属性对客户端 **记录集** 执行排序或筛选。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.Reset(value)  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *value*  
 可选。 如果要对当前 "已筛选" 行集进行筛选， () 默认值为 **True** 的 **布尔** 值。 **False** 表示对原始行集进行筛选，删除所有以前的筛选选项。  
  
## <a name="remarks"></a>备注  
 [SortColumn](./sortcolumn-property-rds.md)、 [SortDirection](./sortdirection-property-rds.md)、 [FilterValue](./filtervalue-property-rds.md)、 [FilterCriterion](./filtercriterion-property-rds.md)和[FilterColumn](./filtercolumn-property-rds.md)属性提供客户端缓存上的排序和筛选功能。 排序功能按一个列中的值对记录进行排序。 筛选功能根据查找条件显示记录子集，而完整的 [记录集](../ado-api/recordset-object-ado.md) 则保留在缓存中。 **Reset** 方法将执行条件，并将当前 **记录集** 替换为可更新的 **记录集**。  
  
 如果对尚未提交的原始数据进行了更改，则 **Reset** 方法将失败。 首先，使用 [SubmitChanges](./submitchanges-method-rds.md) 方法在读/写 **记录集中** 保存所有更改，然后使用 **Reset** 方法对记录进行排序或筛选。  
  
 如果要在行集上执行多个筛选器，可以将可选的 *布尔* 参数与 **Reset** 方法一起使用。 以下示例介绍如何执行此操作：  
  
```  
ADC.SQL = "Select au_lname from authors"  
ADC.Refresh    ' Get the new rowset.  
  
ADC.FilterColumn = "au_lname"  
ADC.FilterCriterion = "<"  
ADC.FilterValue = "'M'"  
ADC.Reset         ' Rowset now has all Last Names < "M".  
  
ADC.FilterCriterion = ">"  
ADC.FilterValue = "'F'"  
' Passing True is not necessary, because it is the   
' default filter on the current "filtered" rowset.  
ADC.Reset(TRUE)     ' Rowset now has all Last   
                    ' Names < "M" and > "F".  
  
ADC.FilterCriterion = ">"  
ADC.FilterValue = "'T'"  
' Filter on the original rowset, throwing out the  
' previous filter options.  
ADC.Reset(FALSE)   ' Rowset now has all Last Names > "T".  
```  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [FilterColumn、FilterCriterion、FilterValue、SortColumn 和 SortDirection 属性和 Reset 方法示例 (VBScript) ](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)