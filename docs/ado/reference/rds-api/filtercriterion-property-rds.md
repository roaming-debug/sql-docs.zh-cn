---
description: FilterCriterion 属性 (RDS)
title: FilterCriterion 属性 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterCriterion property [RDS]
ms.assetid: 24eb03ba-ccfd-4353-b6af-03586b2da6fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 09f5306f6df62792f8be42ed06081596682ca0bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053148"
---
# <a name="filtercriterion-property-rds"></a>FilterCriterion 属性 (RDS)
指示要在筛选器值中使用的求值运算符。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.FilterCriterion = String  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *字符串*  
 一个 **字符串** 值，该值指定 [FilterValue](./filtervalue-property-rds.md) 对记录的求值运算符。 可以是以下任一项： <、 \<=, > 、>=、= 或 <>。  
  
## <a name="remarks"></a>备注  
 [SortColumn](./sortcolumn-property-rds.md)、 [SortDirection](./sortdirection-property-rds.md)、 [FilterValue](./filtervalue-property-rds.md)、 **FilterCriterion** 和 [FilterColumn](./filtercolumn-property-rds.md)属性提供客户端缓存上的排序和筛选功能。 排序功能按一个列中的值对记录进行排序。 筛选功能显示基于查找条件的记录子集，而完整的 [记录集](../ado-api/recordset-object-ado.md) 则保留在缓存中。 [Reset](./reset-method-rds.md)方法将执行条件，并将当前 **记录集** 替换为可更新的 **记录集**。  
  
 "！ =" 运算符对于 **FilterCriterion** 无效;请改用 "<>"。  
  
 如果同时设置了筛选和排序属性，并调用 **Reset** 方法，则首先筛选行集，然后对行集进行排序。 对于升序排序，空值位于顶部;对于降序排序，空值位于底部 (升序为默认行为) 。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [FilterColumn、FilterCriterion、FilterValue、SortColumn 和 SortDirection 属性和 Reset 方法示例 (VBScript) ](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn 属性 (RDS) ](./filtercolumn-property-rds.md)   
 [FilterValue 属性 (RDS) ](./filtervalue-property-rds.md)   
 [SortColumn 属性 (RDS) ](./sortcolumn-property-rds.md)   
 [SortDirection 属性 (RDS)](./sortdirection-property-rds.md)