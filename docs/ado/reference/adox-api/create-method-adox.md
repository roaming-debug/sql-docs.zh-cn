---
description: Create 方法 (ADOX)
title: Create Method (ADOX) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Catalog::raw_Create
- _Catalog::Create
helpviewer_keywords:
- Create method [ADOX]
ms.assetid: 64f5c21c-b581-42d8-bdc7-c4f1bebaf105
author: rothja
ms.author: jroth
ms.openlocfilehash: b44be7497ca6952dd88d6f18ac0b42a6c989db36
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172207"
---
# <a name="create-method-adox"></a>Create 方法 (ADOX)
创建新的目录。  
  
## <a name="syntax"></a>语法  
  
```  
  
Catalog.Create ConnectString  
```  
  
#### <a name="parameters"></a>参数  
 *ConnectString*  
 用于连接到数据源的 **字符串** 值。  
  
## <a name="remarks"></a>备注  
 **Create** 方法创建新的 ADO 连接并打开与 *ConnectString* 中指定的数据源的 [连接](../ado-api/connection-object-ado.md)。 如果成功，新 **连接** 对象将分配给 [ActiveConnection](./activeconnection-property-adox.md) 属性。  
  
 如果提供程序不支持创建新目录，则会发生错误。  
  
## <a name="applies-to"></a>应用于  
 [目录对象 (ADOX)](./catalog-object-adox.md)  
  
## <a name="see-also"></a>另请参阅  
 [Create Method (VB) ](./create-method-example-vb.md)   
 [ActiveConnection 属性 (ADOX)](./activeconnection-property-adox.md)