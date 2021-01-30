---
description: AddNew 方法 (ADO)
title: 在 ADO)  (AddNew 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AddNew
- Recordset15::raw_AddNew
helpviewer_keywords:
- AddNew method [ADO]
ms.assetid: a9f54be9-5763-45d0-a6eb-09981b03bc08
author: rothja
ms.author: jroth
ms.openlocfilehash: 4aedf124f13374c5756519b86a60c1164c0df046
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161735"
---
# <a name="addnew-method-ado"></a>AddNew 方法 (ADO)
为可更新的 [记录集](./recordset-object-ado.md) 对象创建新记录。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.AddNew FieldList, Values  
```  
  
#### <a name="parameters"></a>参数  
 *记录集*  
 **记录集** 对象。  
  
 *字段列表*  
 可选。 单个名称，或者新记录中字段的名称或序号位置的数组。  
  
 *值*  
 可选。 单个值，或新记录中的字段的值数组。 如果 *Fieldlist* 是一个数组，则 *值* 也必须是具有相同成员数的数组;否则，将发生错误。 字段名称的顺序必须与每个数组中的字段值顺序相匹配。  
  
## <a name="remarks"></a>备注  
 使用 **AddNew** 方法创建并初始化新记录。 使用 [支持](./supports-method.md) 方法 with **adAddNew** ([CursorOptionEnum](./cursoroptionenum.md) 值) ，验证是否可以将记录添加到当前 **记录集** 对象。  
  
 调用 **AddNew** 方法之后，新记录将成为当前记录，并在调用 [Update](./update-method.md) 方法后保持最新。 由于新记录将追加到 **记录集中**，因此 **，对更新** 进行的调用将移过 **记录集** 的末尾，使 **EOF** 为 True。 如果 **记录集** 对象不支持书签，则在移动到另一记录后，您可能无法访问新记录。 根据游标类型，可能需要调用 [Requery](./requery-method.md) 方法以使新记录可访问。  
  
 如果在编辑当前记录或添加新记录时调用 **AddNew** ，ADO 将调用 **Update** 方法来保存所有更改，然后创建新记录。  
  
 **AddNew** 方法的行为取决于 **记录集** 对象的更新模式，以及是否传递 *Fieldlist* 和 *Values* 参数。  
  
 在 *即时更新模式下* (在调用 **update** 方法) 时，提供程序将更改写入基础数据源，调用不带参数的 **AddNew** 方法会将 [EditMode](./editmode-property.md) 属性设置为 **adEditAdd** ([EditModeEnum](./editmodeenum.md) 值) 。 提供程序在本地缓存任何字段值更改。 调用 **Update** 方法会将新记录发送到数据库，并将 **EditMode** 属性重置为 **adEditNone** (**EditModeEnum** 值) 。 如果传递了 *Fieldlist* 和 *Values* 参数，则 ADO 会立即将新记录发布到数据库 (不需要 **更新** 调用) ; **EditMode** 属性值不会更改 (**adEditNone**) 。  
  
 在 *批处理更新模式下* (提供程序将在其中缓存多个更改，并仅在调用 [UpdateBatch](./updatebatch-method.md) 方法) 时将这些更改写入基础数据源，调用不带参数的 **AddNew** 方法会将 **EditMode** 属性设置为 **adEditAdd**。 提供程序在本地缓存任何字段值更改。 调用 **Update** 方法会将新记录添加到当前 **记录集**，但访问接口不会将更改发布到基础数据库中，也不会将 **EditMode** 重置为 **adEditNone**，直到调用 **UpdateBatch** 方法。 如果传递 *Fieldlist* 和 *VALUES* 参数，ADO 会将新记录发送到缓存中的存储提供程序，并将 **EditMode** 设置为 **adEditAdd**;需要调用 **UpdateBatch** 方法将新记录发布到基础数据库。  
  
## <a name="example"></a>示例  
 下面的示例演示如何对包含的字段列表和值列表使用 AddNew 方法，以了解如何将字段列表和值列表作为数组包含在内。  
  
```  
create table aa1 (intf int, charf char(10))  
insert into aa1 values (2, 'aa')  
  
Dim cn As New adodb.Connection  
Dim rs As New adodb.Recordset  
Dim cmd As New adodb.Command  
  
cn.ConnectionString = "Provider=SQLOLEDB;Data Source=alexverb2;uid=sa;pwd=foo$bar00;"  
  
cn.Open  
rs.Open "select * from xxx..aa1", cn, adOpenKeyset, adLockOptimistic  
  
Dim fieldsArray(1) As Variant  
fieldsArray(0) = "intf"  
fieldsArray(1) = "charf"  
Dim values(1) As Variant  
values(0) = 4  
values(1) = "as"  
rs.AddNew fieldsArray, values  
rs.Update  
```  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [将 AddNew 方法示例 (VB) ](./addnew-method-example-vb.md)   
 [VBScript 方法示例 (VBScript) ](./addnew-method-example-vbscript.md)   
 [ (VC + +) 的 AddNew 方法示例 ](./addnew-method-example-vc.md)   
 [CancelUpdate 方法 (ADO) ](./cancelupdate-method-ado.md)   
 [EditMode 属性](./editmode-property.md)   
 [Requery 方法](./requery-method.md)   
 [支持方法](./supports-method.md)   
 [Update 方法](./update-method.md)   
 [UpdateBatch 方法](./updatebatch-method.md)