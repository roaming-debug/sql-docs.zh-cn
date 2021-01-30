---
description: Update 方法
title: Update 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Update
helpviewer_keywords:
- Update method [ADO]
ms.assetid: 6b2a9c31-1a7e-40db-8a53-30720d0f6cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 279d0e82bff4d71a2c3b18bbdc7ff88f0b9581a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172428"
---
# <a name="update-method"></a>Update 方法
保存对记录[集](./recordset-object-ado.md)对象的当前行所做的任何更改，或[记录](./record-object-ado.md)对象的[字段](./fields-collection-ado.md)集合。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.Update Fields, Values  
record.Fields.Update  
```  
  
#### <a name="parameters"></a>参数  
 *Fields*  
 可选。 表示单个名称的 **变量** ，或表示要修改的一个或多个字段的名称或序号位置的 **variant** 数组。  
  
 *值*  
 可选。 一个表示单个值的 **变量** ，或一个表示新记录中的一个或多个字段的值的 **变体** 数组。  
  
## <a name="remarks"></a>备注  
  
## <a name="recordset"></a>记录集  
 使用 **Update** 方法保存对 **记录集** 对象的当前记录所做的任何更改，因为调用了 [AddNew](./addnew-method-ado.md) 方法或更改了现有记录中的任何字段值。 **Recordset** 对象必须支持更新。  
  
 若要设置字段值，请执行以下操作之一：  
  
-   为 [字段](./field-object.md) 对象的 [Value](./value-property-ado.md) 属性赋值，并调用 **Update** 方法。  
  
-   使用 **更新** 调用作为参数传递字段名称和值。  
  
-   使用 **更新** 调用传递字段名称和值数组的数组。  
  
 使用字段和值的数组时，两个数组中的元素数必须相等。 并且字段名称的顺序必须与字段值的顺序匹配。 如果字段和值的数量和顺序不匹配，将发生错误。  
  
 如果 **Recordset** 对象支持批处理更新，则可以在调用 [UpdateBatch](./updatebatch-method.md) 方法之前，将多个更改缓存到本地一个或多个记录。 如果在调用 **UpdateBatch** 方法时编辑当前记录或添加新记录，ADO 将自动调用 **Update** 方法，以便在将批处理更改传输到提供程序之前保存当前记录的所有挂起的更改。  
  
 如果在调用 **update** 方法之前从要添加或编辑的记录中移动，ADO 将自动调用 **update** 以保存更改。 如果要取消对当前记录所做的任何更改，或者放弃新添加的记录，则必须调用 [CancelUpdate](./cancelupdate-method-ado.md) 方法。  
  
 在调用 **Update** 方法之后，当前记录保持为最新。  
  
## <a name="record"></a>记录  
 **Update** 方法完成对 **Record** 对象的 [fields](./fields-collection-ado.md)集合中的字段的添加、删除和更新。  
  
 例如， **删除方法删除的字段** 会立即标记为删除，但仍保留在集合中。 若要实际从提供程序的集合中删除这些字段，必须调用 **Update** 方法。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [字段集合 (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [记录集对象 (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [Update 和 CancelUpdate 方法示例 (VB) ](./update-and-cancelupdate-methods-example-vb.md)   
 [Update 和 CancelUpdate 方法示例 (VC + +) ](./update-and-cancelupdate-methods-example-vc.md)   
 [AddNew 方法 (ADO) ](./addnew-method-ado.md)   
 [CancelUpdate 方法 (ADO) ](./cancelupdate-method-ado.md)   
 [EditMode 属性](./editmode-property.md)   
 [UpdateBatch 方法](./updatebatch-method.md)