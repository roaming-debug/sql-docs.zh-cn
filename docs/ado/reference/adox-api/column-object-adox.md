---
description: 列对象 (ADOX)
title: 列对象 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Column
helpviewer_keywords:
- Column object [ADOX]
ms.assetid: 6e772783-1bc8-4ea7-94b2-7d7a52ea5c47
author: rothja
ms.author: jroth
ms.openlocfilehash: fd44882eaf9dafacdfa54bf3149cf8e82d7d86a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054402"
---
# <a name="column-object-adox"></a>列对象 (ADOX)
表示表、索引或键中的列。  
  
## <a name="remarks"></a>备注  
 下面的代码创建一个新 **列**：  
  
 `Dim obj As New Column`  
  
 使用 **Column** 对象的属性和集合，可以：  
  
-   标识具有 [Name 属性 (ADOX) ](./name-property-adox.md) 属性的列。  
  
-   指定列的数据类型，该数据类型具有 [Type 属性 (键)  (ADOX) ](./type-property-key-adox.md) 属性。  
  
-   确定列是否为固定长度，或者它是否可以包含属性为的 null 值 [ (ADOX) ](./attributes-property-adox.md) 属性。  
  
-   指定 [DefinedSize 属性 (ADOX) ](./definedsize-property-adox.md) 属性的列的最大大小。  
  
-   对于数值数据值，请通过 [NumericScale 属性指定刻度 (ADOX) ](./numericscale-property-adox.md) 属性。  
  
-   对于 "数值数据" 值，指定 [精度属性 (ADOX) ](./precision-property-adox.md) 属性的最大精度。  
  
-   指定 [ (adox) 的目录对象 ](./catalog-object-adox.md) ，该对象拥有具有 [PARENTCATALOG 属性 (ADOX) ](./parentcatalog-property-adox.md) 属性的列。  
  
-   对于键列，请指定相关表中的相关列的名称，其中 [RelatedColumn 属性 (ADOX) ](./relatedcolumn-property-adox.md) 属性。  
  
-   对于 "索引列"，请指定排序顺序是按排序顺序 [属性 (ADOX) ](./sortorder-property-adox.md) 属性进行升序或降序。  
  
-   使用 [Properties 集合 (ADO) ](../ado-api/properties-collection-ado.md) 集合访问特定于提供程序的属性。  
  
> [!NOTE]
>  数据提供程序不支持 **列** 对象的所有属性。 如果为提供程序不支持的属性设置了值，则会发生错误。 对于新的 **列** 对象，当对象追加到集合时，将发生错误。 对于现有对象，在设置属性时会发生此错误。  
>   
>  创建 **列** 对象时，对于可选属性是否存在适当的默认值并不保证提供程序支持该属性。 有关提供程序支持的属性的详细信息，请参阅提供程序文档。  
  
 本部分包含以下主题。  
  
-   [列对象属性、方法和事件](./column-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [列和表追加方法、Name 属性示例 (VB) ](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [连接关闭方法，表类型属性示例 (VB) ](./connection-close-method-table-type-property-example-vb.md)   
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ADOX 代码示例： NumericScale 和 Precision 属性示例 (VB) ](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [ParentCatalog 属性示例 (VB) ](./parentcatalog-property-example-vb.md)   
 [SortOrder 属性示例 (VB) ](./sortorder-property-example-vb.md)   
 [列集合 (ADOX) ](./columns-collection-adox.md)   
 [属性集合 (ADO)](../ado-api/properties-collection-ado.md)