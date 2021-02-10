---
description: UpdateBatch 方法
title: UpdateBatch 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::UpdateBatch
- Recordset15::raw_UpdateBatch
helpviewer_keywords:
- UpdateBatch method [ADO]
ms.assetid: 23f9314c-b027-4a51-aeae-50caa2977740
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e754d71d3b616ebd8b44af399c6386e940c8ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056222"
---
# <a name="updatebatch-method"></a>UpdateBatch 方法
将所有挂起的批更新写入磁盘。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.UpdateBatch AffectRecords, PreserveStatus  
```  
  
#### <a name="parameters"></a>参数  
 *AffectRecords*  
 可选。 一个 [AffectEnum](./affectenum.md) 值，指示 **UpdateBatch** 方法将影响的记录数。  
  
 *PreserveStatus*  
 可选。 一个 **布尔** 值，指定是否应提交 [状态](./status-property-ado-recordset.md) 属性指示的本地更改。 如果此值设置为 " **True**"，则在更新完成后，每条记录的 " **状态** " 属性保持不变。  
  
## <a name="remarks"></a>备注  
 在批处理更新模式下修改 **记录集** 对象时使用 **UpdateBatch** 方法，以将 **记录集** 对象中所做的所有更改传输到基础数据库。  
  
 如果 **Recordset** 对象支持批处理更新，则可以在调用 **UpdateBatch** 方法之前，将多个更改缓存到本地一个或多个记录。 如果在调用 **UpdateBatch** 方法时编辑当前记录或添加新记录，ADO 将自动调用 [Update](./update-method.md) 方法，以便在将批处理更改传输到提供程序之前保存当前记录的所有挂起的更改。 只应将批处理更新用于键集或静态游标。  
  
> [!NOTE]
>  如果将 **adAffectGroup** 指定为此参数的值，则在当前 **记录集中** 没有可见记录时将导致错误， (例如筛选器) 不匹配任何记录。  
  
 如果由于与基础数据发生冲突，尝试传输任何或所有记录的更改失败 (例如，其他用户) 已删除记录，则该提供程序会将警告返回到 [错误](./errors-collection-ado.md) 集合，并出现运行时错误。 使用 " [筛选器](./filter-property.md) " 属性 (**AdFilterAffectedRecords**) "和" [状态](./status-property-ado-recordset.md) "属性来查找存在冲突的记录。  
  
 若要取消所有挂起的批更新，请使用 [CancelBatch](./cancelbatch-method-ado.md) 方法。  
  
 如果设置了 [唯一表](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)和 [更新重新同步](./update-resync-property-dynamic-ado.md)动态属性，并且 **记录集** 是对多个表执行联接操作的结果，则在执行 **UpdateBatch** 方法时，将根据 [更新重新同步](./update-resync-property-dynamic-ado.md)属性的设置，隐式执行重新 [同步](./resync-method.md)方法。  
  
 对数据源执行批处理的单个更新的顺序并不一定与它们在本地 **记录集** 上执行的顺序相同。 更新顺序取决于提供程序。 当对彼此相关的更新进行编码时，请考虑这一点，例如插入或更新的外键约束。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 的 UpdateBatch 和 CancelBatch 方法示例 ](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [ (VC + +) 的 UpdateBatch 和 CancelBatch 方法示例 ](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [CancelBatch 方法 (ADO) ](./cancelbatch-method-ado.md)   
 [ADO)  (Clear 方法 ](./clear-method-ado.md)   
 [LockType 属性 (ADO) ](./locktype-property-ado.md)   
 [Update 方法](./update-method.md)