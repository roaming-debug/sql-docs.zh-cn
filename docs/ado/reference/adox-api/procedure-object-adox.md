---
description: 过程对象 (ADOX)
title: 过程对象 (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedure
helpviewer_keywords:
- Procedure object [ADOX]
ms.assetid: 927bcf3e-32f5-4a80-98d3-600779f0732e
author: rothja
ms.author: jroth
ms.openlocfilehash: fec4b21cf4aa619fba72f5d62fff7f6e34506de5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053872"
---
# <a name="procedure-object-adox"></a>过程对象 (ADOX)
表示存储过程。 与 ADO [命令](../ado-api/command-object-ado.md) 对象结合使用时，可以使用 **Procedure** 对象添加、删除或修改存储过程。  
  
## <a name="remarks"></a>备注  
 使用 **Procedure** 对象，您可以创建一个存储过程，而无需知道或使用该提供程序的 "create Procedure" 语法。  
  
 使用 **过程** 对象的属性，可以：  
  
-   标识具有 [Name](./name-property-adox.md) 属性的过程。  
  
-   指定可用于使用 [Command](./command-property-adox.md)属性创建或执行该过程的 ADO **命令** 对象。  
  
-   返回具有 [DateCreated](./datecreated-property-adox.md) 和 [DateModified](./datemodified-property-adox.md) 属性的日期信息。  
  
 本部分包含以下主题。  
  
-   [过程对象属性、方法和事件](./procedure-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [Command 和 CommandText 属性示例 (VB) ](./command-and-commandtext-properties-example-vb.md)   
 [参数集合、Command 属性示例 (VB) ](./parameters-collection-command-property-example-vb.md)   
 [步骤 Append 方法示例 (VB) ](./procedures-append-method-example-vb.md)   
 [过程 Delete 方法示例 (VB) ](./procedures-delete-method-example-vb.md)   
 [过程集合 (ADOX)](./procedures-collection-adox.md)