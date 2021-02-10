---
description: SortColumn 属性 (RDS)
title: SortColumn 属性 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SortColumn property [RDS]
ms.assetid: f6f80f67-f0fb-4e63-a5f5-8fdf312aac63
author: rothja
ms.author: jroth
ms.openlocfilehash: 5a2acfc204928fb23cea7a466e3baaa0a1ebb8aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049167"
---
# <a name="sortcolumn-property-rds"></a>SortColumn 属性 (RDS)
指示对记录进行排序所依据的列。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.SortColumn = String  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *字符串*  
 一个 **字符串** 值，该值表示对记录进行排序所依据的列的名称或别名。  
  
## <a name="remarks"></a>备注  
 **SortColumn**、 [SortDirection](./sortdirection-property-rds.md)、 [FilterValue](./filtervalue-property-rds.md)、 [FilterCriterion](./filtercriterion-property-rds.md)和 [FilterColumn](./filtercolumn-property-rds.md)属性提供客户端缓存上的排序和筛选功能。 排序功能按一个列中的值对记录进行排序。 筛选功能显示基于查找条件的记录子集，而完整的 [记录集](../ado-api/recordset-object-ado.md) 则保留在缓存中。 [Reset](./reset-method-rds.md)方法将执行条件，并将当前 **记录集** 替换为可更新的 **记录集**。  
  
 若要对 **记录集** 进行排序，必须先保存所有挂起的更改。 如果你使用的是 **RDS。DataControl**，可以使用 [SubmitChanges](./submitchanges-method-rds.md) 方法。 例如，如果你的 **RDS。DataControl** 名为 ADC1，则你的代码将为 `ADC1.SubmitChanges` 。 如果使用 ADO **记录集**，则可以使用其 [UpdateBatch](../ado-api/updatebatch-method.md) 方法。 使用 **UpdateBatch** 是使用 [CreateRecordset](./createrecordset-method-rds.md)方法创建的 **记录集** 对象的建议方法。 例如，你的代码可以是 `myRS.UpdateBatch` 或 `ADC1.Recordset.UpdateBatch` 。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [FilterColumn、FilterCriterion、FilterValue、SortColumn 和 SortDirection 属性和 Reset 方法示例 (VBScript) ](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [排序属性](../ado-api/sort-property.md)   
 [SortDirection 属性 (RDS)](./sortdirection-property-rds.md)