---
description: FilterValue 属性 (RDS)
title: FilterValue 属性 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterValue property [ADO]
ms.assetid: 28f17186-b842-4cf9-b320-a9bb941c481b
author: rothja
ms.author: jroth
ms.openlocfilehash: 44e1eca1ac4a949a3d46fe3a1ec58ff0c0b882dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049387"
---
# <a name="filtervalue-property-rds"></a>FilterValue 属性 (RDS)
指示用于筛选记录的值。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.FilterValue = String  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *字符串*  
 一个 **字符串** 值，该值表示用于筛选记录 (例如或) 的数据值 `'Programmer'` `125` 。  
  
## <a name="remarks"></a>备注  
 [SortColumn](./sortcolumn-property-rds.md)、 [SortDirection](./sortdirection-property-rds.md)、 **FilterValue**、 [FilterCriterion](./filtercriterion-property-rds.md)和 [FilterColumn](./filtercolumn-property-rds.md)属性提供客户端缓存上的排序和筛选功能。 排序功能按一个列中的值对记录进行排序。 筛选功能显示基于查找条件的记录子集，而完整的 [记录集](../ado-api/recordset-object-ado.md) 则保留在缓存中。 [Reset](./reset-method-rds.md)方法将执行条件，并将当前 **记录集** 替换为可更新的 **记录集**。  
  
 空值导致类型不匹配错误。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [FilterColumn、FilterCriterion、FilterValue、SortColumn 和 SortDirection 属性和 Reset 方法示例 (VBScript) ](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn 属性 (RDS) ](./filtercolumn-property-rds.md)   
 [FilterCriterion 属性 (RDS) ](./filtercriterion-property-rds.md)   
 [SortColumn 属性 (RDS) ](./sortcolumn-property-rds.md)   
 [SortDirection 属性 (RDS)](./sortdirection-property-rds.md)