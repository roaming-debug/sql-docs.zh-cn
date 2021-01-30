---
description: ADOStreamConstruction 接口
title: ADOStreamConstruction 接口 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADOStreamConstruction
helpviewer_keywords:
- ADOStreamConstruction interface [ADO]
ms.assetid: 92f5a939-3e1a-4b14-a9dd-90e6ce2dec74
author: rothja
ms.author: jroth
ms.openlocfilehash: 1138d00d291ffdabf415177a948bf87b1c6502a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171591"
---
# <a name="adostreamconstruction-interface"></a>ADOStreamConstruction 接口
**ADOStreamConstruction** 接口用于从 c/c + + 应用程序中的 OLE DB **ISTREAM** 对象构造 ADO **流** 对象。  
  
## <a name="properties"></a>属性  
  
|properties|说明|  
|-|-|  
|[流](./stream-property.md)|读/写。 获取/设置 OLE DB **流** 对象。|  
  
## <a name="methods"></a>方法  
 无。  
  
## <a name="events"></a>事件  
 无。  
  
## <a name="remarks"></a>备注  
 给定 OLE DB **IStream** 对象 (`pStream`) ，将 ADO **流** 对象的构造 (`adoStr`) 数量，以执行以下三个基本操作：  
  
1.  创建 ADO **流** 对象：  
  
    ```  
    Stream20Ptr adoStr;  
    adoStr.CreateInstance(__uuidof(Stream));  
    ```  
  
2.  查询 **Stream** 对象上的 **IADOStreamConstruction** 接口：  
  
    ```  
    adoStreamConstructionPtr adoStrConstruct=NULL;  
    adoStr->QueryInterface(__uuidof(ADOStreamConstruction),  
                         (void**)&adoStrConstruct);  
    ```  
  
 调用 `IADOStreamConstruction::get_Stream` 属性方法，设置 ADO **流** 对象上的 OLE DB **IStream** 对象：  
  
```  
IUnknown *pUnk=NULL;  
pRowset->QueryInterface(IID_IUnknown, (void**)&pUnk);  
adoStrConstruct->put_Stream(pUnk);  
```  
  
 生成的 `adoStr` 对象现在表示从 OLE DB **IStream** 对象构造的 ADO **流** 对象。  
  
## <a name="requirements"></a>要求  
 **版本：** ADO 2.0 或更高版本  
  
 **库：** msado15.dll  
  
 **UUID：** 00000283-0000-0010-8000-00AA006D2EA4  
  
## <a name="see-also"></a>另请参阅  
 [ADO API 参考](./ado-api-reference.md)