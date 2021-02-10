---
description: Append 方法（ADOX 过程）
title: )  (ADOX 过程的 Append 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Procedures::Append
- Procedures::raw_Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 38e3492c-c1e1-42e3-a71a-befdc90204db
author: rothja
ms.author: jroth
ms.openlocfilehash: babc7e08a399643781ec2293f2dd9f214e3ad48c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050498"
---
# <a name="append-method-adox-procedures"></a>Append 方法（ADOX 过程）
将新 [过程](./procedure-object-adox.md) 对象添加到 [过程](./procedures-collection-adox.md) 集合。  
  
## <a name="syntax"></a>语法  
  
```  
  
Procedures.Append Name, Command  
```  
  
#### <a name="parameters"></a>参数  
 *名称*  
 一个 **字符串** 值，该值指定要创建并追加的过程的名称。  
  
 *命令*  
 一个 ADO [命令](../ado-api/command-object-ado.md) 对象，表示要创建和追加的过程。  
  
## <a name="remarks"></a>备注  
 使用 **Command** 对象中指定的名称和属性在数据源中创建一个新过程。  
  
 如果用户指定的命令文本表示视图而不是过程，则行为取决于所使用的提供程序。 如果提供程序不支持保留命令，则 **Append** 将会失败。  
  
> [!NOTE]
>  使用适用于 Microsoft Jet 的 OLE DB 提供程序时 **，过程** 集合 **Append** 方法将允许您在 *Command* 参数中指定 **视图**，而不是 **过程**。 **视图** 将添加到数据源，并将添加到 "**过程**" 集合中。 **追加** 后，如果刷新了 "**过程**" 和 "**视图**" 集合，则该 **视图** 将不再出现在 "**过程**" 集合中，并将显示在 "**视图**" 集合中。  
  
## <a name="applies-to"></a>应用于  
 [过程集合 (ADOX)](./procedures-collection-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [步骤 Append 方法示例 (VB) ](./procedures-append-method-example-vb.md)   
 [Append 列 (追加方法) ](./append-method-adox-columns.md)   
 [将方法追加 (ADOX 组) ](./append-method-adox-groups.md)   
 [Append 索引 (Append 方法) ](./append-method-adox-indexes.md)   
 [追加方法 (ADOX 密钥) ](./append-method-adox-keys.md)   
 [Append 表 (追加方法) ](./append-method-adox-tables.md)   
 [ADOX 用户 (追加方法) ](./append-method-adox-users.md)   
 [Append 方法（ADOX 视图）](./append-method-adox-views.md)