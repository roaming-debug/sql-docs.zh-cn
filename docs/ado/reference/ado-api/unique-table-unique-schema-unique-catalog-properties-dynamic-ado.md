---
description: " (ADO 的唯一表、唯一架构、唯一目录 Properties-Dynamic) "
title: " (ADO) 控制对记录集基表的更改 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Unique Table property [ADO]
- Unique Schema property [ADO]
- Unique Catalog property [ADO]
ms.assetid: d0e775d8-e353-46a1-ad10-ed4cc240dfaa
author: rothja
ms.author: jroth
ms.openlocfilehash: 1cd94edb4b1e2c2fba12221a3371592c34eb8c50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056355"
---
# <a name="unique-table-unique-schema-unique-catalog-properties-dynamic-ado"></a> (ADO 的唯一表、唯一架构、唯一目录 Properties-Dynamic) 
使您能够在 [记录集中](./recordset-object-ado.md) 控制对多个基表上的联接操作所形成的特定基表的修改。  
  
-   **唯一表** 指定允许更新、插入和删除的基表的名称。  
  
-   **Unique 架构** 指定表所有者的 *架构* 或名称。  
  
-   **Unique catalog** 指定包含表的数据库的 *目录* 或名称。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值，该值是表、架构或目录的名称。  
  
## <a name="remarks"></a>备注  
 所需基表由其目录、架构和表名唯一标识。 设置 **Unique table** 属性后，将使用 **唯一架构** 或 **唯一目录** 属性的值来查找基表。 它在设置 **唯一的表** 属性前设置，但不是必需的，也可以是 **唯一的架构** 和唯一的 **目录** 属性。  
  
 **唯一表** 的主键被视为整个 **记录集** 的主键。 这是用于需要主键的任何方法的键。  
  
 当设置 **唯一表** 时， [Delete](./delete-method-ado-recordset.md) 方法只影响指定的表。 [AddNew](./addnew-method-ado.md)、 [Resync](./resync-method.md)、 [Update](./update-method.md)和 [UpdateBatch](./updatebatch-method.md)方法会影响 **记录集** 的任何适当的基础基表。  
  
 在执行任何自定义重新同步之前，必须指定 **唯一表**。 如果尚未指定 **唯一表** ， [Resync 命令](./resync-command-property-dynamic-ado.md) 属性将不起作用。  
  
 如果找不到唯一的基表，将产生运行时错误。  
  
 当 [CursorLocation](./cursorlocation-property-ado.md)属性设置为 **adUseClient** 时，这些动态属性将追加到 **Recordset** 对象 [properties](./properties-collection-ado.md) collection。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [记录集对象 (ADO)](./recordset-object-ado.md)