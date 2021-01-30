---
description: Attributes 属性 (ADO)
title: ADO)  (特性属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Attributes
- Field20::Attributes
- _Parameter::Attributes
helpviewer_keywords:
- Attributes property [ADO]
ms.assetid: acc15d40-68a6-4ba9-85bd-12d331aecaa6
author: rothja
ms.author: jroth
ms.openlocfilehash: 72cbd650b2ab3c66231383e89b048bb03c42b6f5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164810"
---
# <a name="attributes-property-ado"></a>Attributes 属性 (ADO)
指示对象的一个或多个特征。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **长整型** 值。  
  
 对于 [连接](./connection-object-ado.md) 对象，" **属性** " 属性是 "读/写"，其值可以是一个或多个 [XactAttributeEnum](./xactattributeenum.md) 值的总和。 默认值为零 (0)。  
  
 对于 [参数](./parameter-object.md) 对象，" **属性** " 属性是 "读/写"，其值可以是任何一个或多个 [ParameterAttributesEnum](./parameterattributesenum.md) 值的总和。 默认值为 **adParamSigned**。  
  
 对于 [字段](./field-object.md) 对象，" **属性** " 属性可以是一个或多个 [FieldAttributeEnum](./fieldattributeenum.md) 值的总和。 它通常是只读的。 但是，对于已附加到 [记录](./record-object-ado.md)的 [字段](./fields-collection-ado.md)集合的新 **字段** 对象，仅在指定了 **字段** 的 [值](./value-property-ado.md)属性并且数据访问接口已成功添加新 **字段** 之后，**属性** 才是可读/**写的。** [](./update-method.md)  
  
 对于 [属性](./property-object-ado.md) 对象，" **属性** " 属性是只读的，它的值可以是任何一个或多个 [PropertyAttributesEnum](./propertyattributesenum.md) 值的总和。  
  
## <a name="remarks"></a>备注  
 使用 " **属性** " 属性可设置或返回 **连接** 对象、 **参数** 对象、 **字段** 对象或 **属性** 对象的特征。  
  
 设置多个属性时，可以对相应的常量求和。 如果将属性值设置为包含不兼容常量的总和，则会发生错误。  
  
> [!NOTE]
>  **远程数据服务使用情况** 此属性在客户端 **连接** 对象上不可用。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [连接对象 (ADO)](./connection-object-ado.md)  
        [字段对象](./field-object.md)  
    :::column-end:::
    :::column:::
        [参数对象](./parameter-object.md)  
        [属性对象 (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [属性和名称属性示例 (VB) ](./attributes-and-name-properties-example-vb.md)   
 [特性和名称属性示例 (VC + +) ](./attributes-and-name-properties-example-vc.md)   
 [AppendChunk 方法 (ADO) ](./appendchunk-method-ado.md)   
 [BeginTrans、CommitTrans 和 RollbackTrans 方法 (ADO) ](./begintrans-committrans-and-rollbacktrans-methods-ado.md)   
 [GetChunk 方法 (ADO)](./getchunk-method-ado.md)