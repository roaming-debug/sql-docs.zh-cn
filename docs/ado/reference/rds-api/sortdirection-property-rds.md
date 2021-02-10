---
description: SortDirection 属性 (RDS)
title: SortDirection 属性 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SortDirection property [RDS]
ms.assetid: 1d9d8715-e4ad-4ff3-bf7f-f1dc0532d8c2
author: rothja
ms.author: jroth
ms.openlocfilehash: b736b616578eb3d0a8efa91c7b2b76515f33b96a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049124"
---
# <a name="sortdirection-property-rds"></a>SortDirection 属性 (RDS)
指示排序顺序是升序还是降序。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.SortDirection = value  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *值*  
 一个 **布尔** 值，如果设置为 **True**，则指示排序方向为升序。 **False** 指示降序。  
  
## <a name="remarks"></a>备注  
 [SortColumn](./sortcolumn-property-rds.md)、 **SortDirection**、 [FilterValue](./filtervalue-property-rds.md)、 [FilterCriterion](./filtercriterion-property-rds.md)和 [FilterColumn](./filtercolumn-property-rds.md)属性提供客户端缓存上的排序和筛选功能。 排序功能通过使用一列中的值对记录进行排序。 筛选功能显示基于查找条件的记录子集，而完整的 [记录集](../ado-api/recordset-object-ado.md) 则保留在缓存中。 [Reset](./reset-method-rds.md)方法将执行条件，并将当前 **记录集** 替换为可更新的 **记录集**。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [FilterColumn、FilterCriterion、FilterValue、SortColumn 和 SortDirection 属性和 Reset 方法示例 (VBScript) ](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [排序属性](../ado-api/sort-property.md)   
 [SortColumn 属性 (RDS)](./sortcolumn-property-rds.md)