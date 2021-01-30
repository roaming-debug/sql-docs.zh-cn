---
description: Append 方法（ADOX 组）
title: ) 的 ADOX 组 (追加方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Groups::raw_Append
- Groups::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 56b94fc6-7ef0-4e4a-82a3-033b94c46036
author: rothja
ms.author: jroth
ms.openlocfilehash: 859e3dea7f5dbdb84efd44aebc766eb2d195a7e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164309"
---
# <a name="append-method-adox-groups"></a>Append 方法（ADOX 组）
向[Groups](./groups-collection-adox.md)集合添加一个新的[组](./group-object-adox.md)对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
Groups.Append Group  
```  
  
#### <a name="parameters"></a>参数  
 *组*  
 要追加的 **组** 对象或要创建和追加的组的名称。  
  
## <a name="remarks"></a>备注  
 [目录](./catalog-object-adox.md)的 **Groups** 集合表示所有目录的组帐户。 [用户](./user-object-adox.md)的 **Groups** 集合仅表示该用户所属的组。  
  
 如果提供程序不支持创建组，将出现错误。  
  
> [!NOTE]
>  在将 **组** 对象追加到 **用户** 对象的 **groups** 集合之前，**目录** 的 **组** 集合中必须已经存在与要追加的组 [对象同名的](./name-property-adox.md)**组** 对象。  
  
## <a name="applies-to"></a>应用于  
 [组集合 (ADOX)](./groups-collection-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [组和用户追加，ChangePassword 方法示例 (VB) ](./groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append 列 (追加方法) ](./append-method-adox-columns.md)   
 [Append 索引 (Append 方法) ](./append-method-adox-indexes.md)   
 [追加方法 (ADOX 密钥) ](./append-method-adox-keys.md)   
 [附加方法 (ADOX 过程) ](./append-method-adox-procedures.md)   
 [Append 表 (追加方法) ](./append-method-adox-tables.md)   
 [ADOX 用户 (追加方法) ](./append-method-adox-users.md)   
 [Append 方法（ADOX 视图）](./append-method-adox-views.md)