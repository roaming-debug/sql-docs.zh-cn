---
description: FilterColumn 属性 (RDS)
title: FilterColumn 属性 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterColumn property [ADO]
ms.assetid: 0a5473e8-8ce6-4518-83fb-4920b827e285
author: rothja
ms.author: jroth
ms.openlocfilehash: 8235600daed0998a9ba5cb3876b18f7f7a954f1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049397"
---
# <a name="filtercolumn-property-rds"></a>FilterColumn 属性 (RDS)
指示用于计算筛选条件的列。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.FilterColumn = String  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *字符串*  
 一个 **字符串** 值，该值指定要对其计算筛选条件的列。 筛选条件是在 [FilterCriterion](./filtercriterion-property-rds.md) 属性中指定的。  
  
## <a name="remarks"></a>备注  
 [SortColumn](./sortcolumn-property-rds.md)、 [SortDirection](./sortdirection-property-rds.md)、 [FilterValue](./filtervalue-property-rds.md)、 [FilterCriterion](./filtercriterion-property-rds.md)和 **FilterColumn** 属性提供客户端缓存上的排序和筛选功能。 排序功能按一个列中的值对记录进行排序。 筛选功能显示基于查找条件的记录子集，而完整的 [记录集](../ado-api/recordset-object-ado.md) 则保留在缓存中。 [Reset](./reset-method-rds.md)方法将执行条件，并将当前 **记录集** 替换为可更新的 **记录集**。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [FilterColumn、FilterCriterion、FilterValue、SortColumn 和 SortDirection 属性和 Reset 方法示例 (VBScript) ](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterCriterion 属性 (RDS) ](./filtercriterion-property-rds.md)   
 [FilterValue 属性 (RDS) ](./filtervalue-property-rds.md)   
 [SortColumn 属性 (RDS) ](./sortcolumn-property-rds.md)   
 [SortDirection 属性 (RDS)](./sortdirection-property-rds.md)