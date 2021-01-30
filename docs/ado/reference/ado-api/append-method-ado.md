---
description: Append 方法 (ADO)
title: ADO)  (追加方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _DynaCollection::Append
helpviewer_keywords:
- Append method [ADO]
ms.assetid: f8a9bbed-ba9c-4698-945d-317ad22d2e92
author: rothja
ms.author: jroth
ms.openlocfilehash: 03e454407c1b01b34ea918f81a404d579f8a3809
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167890"
---
# <a name="append-method-ado"></a>Append 方法 (ADO)
将对象追加到集合。 如果集合是 [字段](./fields-collection-ado.md)，则可在将新字段对象追加到集合之前创建新的 [字段](./field-object.md) 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
collection.Append object  
fields.Append Name, Type, DefinedSize, Attrib, FieldValue  
```  
  
#### <a name="parameters"></a>参数  
 *集合*  
 集合对象。  
  
 *字段*  
 **字段** 集合。  
  
 *object*  
 一个对象变量，表示要追加的对象。  
  
 *名称*  
 一个包含新 **字段** 对象名称的 **字符串** 值，不能与 *字段* 中的任何其他对象同名。  
  
 *Type*  
 一个 [DataTypeEnum](./datatypeenum.md) 值，其默认值为 **adEmpty**，指定新字段的数据类型。 ADO 不支持以下数据类型，不应在将新字段追加到 [记录集对象 (ado)](./recordset-object-ado.md)： **adIDispatch**、 **adIUnknown**、 **adVariant** 时使用。  
  
 *DefinedSize*  
 可选。 一个 **长整型** 值，表示新字段的定义的大小（以字符或字节为单位）。 此参数的默认值是从 *类型* 派生的。 具有大于255字节的 *DefinedSize* 的字段将被视为可变长度列。 未指定 *DefinedSize* 的默认值。  
  
 *恶性*  
 可选。 一个 [FieldAttributeEnum](./fieldattributeenum.md) 值，其默认值为 **adFldDefault**，用于指定新字段的特性。 如果未指定此值，则字段将包含派生自 *类型* 的特性。  
  
 *FieldValue*  
 可选。 一个表示新字段的值的 **变量** 。 如果未指定，则使用 null 值追加字段。  
  
## <a name="remarks"></a>备注  
  
## <a name="parameters-collection"></a>Parameters 集合  
 必须先设置[参数](./parameter-object.md)对象的[Type](./type-property-ado.md)属性，然后再将其追加到[Parameters](./parameters-collection-ado.md)集合。 如果选择可变长度的数据类型，则还必须将 " [Size](./size-property-ado-parameter.md) " 属性设置为大于零的值。  
  
 使用存储过程或参数化查询，自行描述参数可最大程度地减少对提供程序的调用，从而提高性能。 但是，您必须知道与要调用的存储过程或参数化查询相关联的参数的属性。  
  
 使用 [CreateParameter](./createparameter-method-ado.md) 方法创建具有相应属性设置的 **参数** 对象，并使用 **Append** 方法将它们添加到 [Parameters](./parameters-collection-ado.md) 集合。 这使你可以设置和返回参数值，而无需调用提供程序来获取参数信息。 如果要写入未提供参数信息的提供程序，则必须使用此方法手动填充 **参数** 集合以使用参数。  
  
## <a name="fields-collection"></a>字段集合  
 仅当向 [记录](./record-object-ado.md)对象添加 **字段** 对象，而不是 **记录集** 对象时， *FieldValue* 参数才有效。 使用 **Record** 对象，可以同时追加字段并提供值。 对于 **recordset** 对象，您必须在 **记录集** 关闭时创建字段，然后打开该 **记录集** 并向字段分配值。  
  
> [!NOTE]
>  对于附加到 **Record** 对象的 **Fields** 集合的新 **字段** 对象，必须先设置 [Value](./value-property-ado.md)属性，然后才能指定任何其他 **字段** 属性。 首先，必须在名为的 **字段** 集合上分配和 [更新](./update-method.md)**值** 属性的特定值。 然后，可以访问其他属性，例如 [类型](./type-property-ado.md) 或 [属性](./attributes-property-ado.md) 。 以下数据 **(类型的****字段** 对象无法追加到 **DataTypeEnum**) ，并且将导致发生错误： **adArray**、 **adChapter**、 **adEmpty**、 **adPropVariant** 和 **adUserDefined**。 此外，ADO 不支持以下数据类型： **adIDispatch**、 **adIUnknown** 和 **adIVariant**。 对于这些类型，追加时不会发生错误，但使用可能产生不可预知的结果，包括内存泄漏。  
  
## <a name="recordset"></a>记录集  
 如果在调用 **Append** 方法之前未设置 [CursorLocation](./cursorlocation-property-ado.md)属性，则在调用 [Recordset](./recordset-object-ado.md)对象的 [Open](./open-method-ado-recordset.md)方法时， **CursorLocation** 将设置为 **adUseClient** (一个 [CursorLocationEnum](./cursorlocationenum.md)值自动) 。  
  
 如果对打开的 **记录集** 的 **字段** 集合或已设置 [ActiveConnection](./activeconnection-property-ado.md)属性的 **记录集** 调用 **Append** 方法，将会发生运行时错误。 您只能向未打开且尚未连接到数据源的 **记录集** 追加字段。 当使用 [CreateRecordset](../rds-api/createrecordset-method-rds.md)方法制造 **Recordset** 对象或将其分配给对象变量时，通常会出现这种情况。  
  
## <a name="record"></a>记录  
 如果对打开的 **记录** 的 **字段** 集合调用 **Append** 方法，则不会发生运行时错误。 新字段将添加到 **Record** 对象的 "**字段**" 集合中。 如果 **记录** 是从 **记录集** 派生的，则新字段将不会出现在 **recordset** 对象的 "**字段**" 集合中。  
  
 通过将值分配给字段对象，可以创建不存在的字段并将其追加到 **字段集合中** ，就像它已存在于集合中一样。 分配将触发 **字段** 对象的自动创建和追加，然后将完成分配。  
  
 将 **字段** 追加到 **Record** 对象的 **Fields** 集合后，调用 **fields** 集合的 **Update** 方法来保存更改。  
  
## <a name="applies-to"></a>应用于  
  
- [字段集合 (ADO)](./fields-collection-ado.md)  
- [参数集合 (ADO)](./parameters-collection-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Append 和 CreateParameter 方法示例 (VB) ](./append-and-createparameter-methods-example-vb.md)   
 [附加和 CreateParameter 方法示例 (VC + +) ](./append-and-createparameter-methods-example-vc.md)   
 [CreateParameter 方法 (ADO) ](./createparameter-method-ado.md)   
 [ (ADO 字段集合的 Delete 方法) ](./delete-method-ado-fields-collection.md)   
 [ (ADO 参数集合的 Delete 方法) ](./delete-method-ado-parameters-collection.md)   
 [ADO 记录集 (Delete 方法) ](./delete-method-ado-recordset.md)   
 [Update 方法](./update-method.md)