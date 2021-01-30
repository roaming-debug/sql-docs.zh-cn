---
description: 目录对象 (ADOX)
title: " (ADOX) 的目录对象 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog
helpviewer_keywords:
- Catalog object [ADOX]
ms.assetid: bb651639-a488-4e38-b6de-0ed99fa4dd92
author: rothja
ms.author: jroth
ms.openlocfilehash: cfb44f5e35b1f30e504cfbf83a85c5a61aceb987
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164302"
---
# <a name="catalog-object-adox"></a>目录对象 (ADOX)
包含描述数据源的架构目录)  ([表](./tables-collection-adox.md)、 [视图](./views-collection-adox.md)、 [用户](./users-collection-adox.md)、 [组](./groups-collection-adox.md)和 [过程](./procedures-collection-adox.md) 的集合。  
  
## <a name="remarks"></a>备注  
 您可以通过添加或删除对象或通过修改现有对象来修改 **目录** 对象。 某些提供程序可能不支持所有 **目录** 对象，也可能只支持查看架构信息。  
  
 使用 **Catalog** 对象的属性和方法，你可以：  
  
-   通过将 [ActiveConnection](./activeconnection-property-adox.md) 属性设置为 ADO [连接](../ado-api/connection-object-ado.md) 对象或有效的连接字符串，打开目录。  
  
-   使用 [create](./create-method-adox.md) 方法创建新的目录。  
  
-   使用 [GetObjectOwner](./getobjectowner-method-adox.md)和 [SetObjectOwner](./setobjectowner-method.md)方法确定 **目录** 中对象的所有者。  
  
 本部分包含以下主题。  
  
-   [目录对象属性、方法和事件](./catalog-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [目录 ActiveConnection 属性示例 (VB) ](./catalog-activeconnection-property-example-vb.md)   
 [Command 和 CommandText 属性示例 (VB) ](./command-and-commandtext-properties-example-vb.md)   
 [连接关闭方法，表类型属性示例 (VB) ](./connection-close-method-table-type-property-example-vb.md)   
 [Create Method (VB) ](./create-method-example-vb.md)   
 [键 Append 方法、Key Type、RelatedColumn、RelatedTable 和 UpdateRule Properties Example (VB) ](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [参数集合、Command 属性示例 (VB) ](./parameters-collection-command-property-example-vb.md)   
 [ParentCatalog 属性示例 (VB) ](./parentcatalog-property-example-vb.md)   
 [步骤 Append 方法示例 (VB) ](./procedures-append-method-example-vb.md)   
 [过程 Delete 方法示例 (VB) ](./procedures-delete-method-example-vb.md)   
 [过程 Refresh 方法示例 (VB) ](./procedures-refresh-method-example-vb.md)   
 [视图和字段集合示例 (VB) ](./views-and-fields-collections-example-vb.md)   
 [ (VB) 视图追加方法示例 ](./views-append-method-example-vb.md)   
 [Views 集合、CommandText 属性示例 (VB) ](./views-collection-commandtext-property-example-vb.md)   
 [Views Delete 方法示例 (VB) ](./views-delete-method-example-vb.md)   
 [ (VB) Views Refresh 方法示例 ](./views-refresh-method-example-vb.md)   
 [组集合 (ADOX) ](./groups-collection-adox.md)   
 [过程集合 (ADOX) ](./procedures-collection-adox.md)   
 [表集合 (ADOX) ](./tables-collection-adox.md)   
 [用户集合 (ADOX) ](./users-collection-adox.md)   
 [视图集合 (ADOX)](./views-collection-adox.md)