---
description: Sort 属性
title: 排序属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::get_Sort
- Recordset15::put_Sort
- Recordset15::PutSort
- Recordset15::GetSort
- Recordset15::Sort
helpviewer_keywords:
- DESC [ADO]
- ASC [ADO]
- Sort property [ADO]
ms.assetid: 3683ffa0-6f93-4906-9533-ef6942f24f39
author: rothja
ms.author: jroth
ms.openlocfilehash: ffd17de7783ff6d27b8b14d70e41fa6839e211dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040597"
---
# <a name="sort-property"></a>Sort 属性
指示 [记录记录集](./recordset-object-ado.md) 的一个或多个字段名称，以及是否按升序或降序对每个字段进行排序。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值，该值指示作为排序依据的 **记录集** 中的字段名称。 每个名称之间用逗号分隔，后面可以跟空白和关键字 **ASC**，后者按升序对字段进行排序 **，或使用降序对** 字段进行排序。 默认情况下，如果未指定关键字，则字段将按升序排序。  
  
## <a name="remarks"></a>备注  
 此属性需要将 [CursorLocation](./cursorlocation-property-ado.md) 属性设置为 **adUseClient**。 如果索引不存在，则将为 **Sort** 属性中指定的每个字段创建临时索引。  
  
 排序操作非常高效，因为数据不会进行物理重新排列，而只是按索引指定的顺序进行访问。  
  
 当 **sort** 属性的值是空字符串以外的任何值时，**排序** 属性顺序优先于用于打开 **记录集** 的 SQL 语句中包含的 order **by** 子句中指定的顺序。  
  
 在访问 **排序** 属性之前，不必打开 **记录集**;它可以在实例化 **Recordset** 对象之后随时设置。  
  
 如果将 **Sort** 属性设置为空字符串，则会将行重置为其原始顺序并删除临时索引。 不会删除现有索引。  
  
 假设 **记录集** 包含三个名为 " *firstName*"、" *middleInitial*" 和 " *lastName*" 的字段。 将 **Sort** 属性设置为字符串 " `lastName DESC, firstName ASC` "，这会按姓氏以降序对 **记录集** 进行排序，然后按名字以升序排序。 忽略中间的初始。  
  
 不能将字段命名为 "ASC" 或 "DESC"，因为这些名称与关键字 **ASC** 和 **DESC** 冲突。 您可以通过在返回 **记录集** 的查询中使用 **AS** 关键字，为具有冲突名称的字段创建别名。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 对属性示例排序 ](./sort-property-example-vb.md)   
 [ (VC + + 的排序属性示例) ](./sort-property-example-vc.md)   
 [优化 Property-Dynamic (ADO) ](./optimize-property-dynamic-ado.md)   
 [SortColumn 属性 (RDS) ](../rds-api/sortcolumn-property-rds.md)   
 [SortDirection 属性 (RDS)](../rds-api/sortdirection-property-rds.md)