---
description: Append 方法（ADOX 视图）
title: )  (ADOX 视图的 Append 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Views::raw_Append
- Views::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 6070fd58-3237-4c77-a966-5b39ce5d57e4
author: rothja
ms.author: jroth
ms.openlocfilehash: 757778961d7b66dd147b8b013b48062dae0cbd91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172273"
---
# <a name="append-method-adox-views"></a>Append 方法（ADOX 视图）
创建新的 [视图](./view-object-adox.md) 对象并将其追加到 [Views](./views-collection-adox.md) 集合。  
  
## <a name="syntax"></a>语法  
  
```  
  
Views.Append Name, Command  
```  
  
#### <a name="parameters"></a>参数  
 *名称*  
 一个 **字符串** 值，该值指定要创建的视图的名称。  
  
 *命令*  
 一个 ADO [命令](../ado-api/command-object-ado.md) 对象，表示要创建的视图。  
  
## <a name="remarks"></a>备注  
 使用 **Command** 对象中指定的名称和属性在数据源中创建新视图。  
  
 如果用户指定的命令文本表示过程而不是视图，则行为取决于提供程序。 如果提供程序不支持保留命令，则 **Append** 将会失败。  
  
> [!NOTE]
>  使用 Microsoft Jet 的 OLE DB 提供程序时， **Views** 集合 **Append** 方法将允许您在 *Command* 参数中指定 **过程**，而不是 **视图**。 该 **过程** 将添加到数据源，并将添加到 **Views** 集合。 **追加** 后，如果刷新了 **过程** 和 **视图** 集合，则该 **过程** 将不再出现在 **Views** 集合中并且会出现在 "**过程**" 集合中。  
  
## <a name="applies-to"></a>应用于  
 [视图集合 (ADOX)](./views-collection-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 视图追加方法示例 ](./views-append-method-example-vb.md)   
 [Append 列 (追加方法) ](./append-method-adox-columns.md)   
 [将方法追加 (ADOX 组) ](./append-method-adox-groups.md)   
 [Append 索引 (Append 方法) ](./append-method-adox-indexes.md)   
 [追加方法 (ADOX 密钥) ](./append-method-adox-keys.md)   
 [附加方法 (ADOX 过程) ](./append-method-adox-procedures.md)   
 [Append 表 (追加方法) ](./append-method-adox-tables.md)   
 [Append 方法（ADOX 用户）](./append-method-adox-users.md)