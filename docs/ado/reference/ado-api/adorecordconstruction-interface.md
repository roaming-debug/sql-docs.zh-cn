---
description: ADORecordConstruction 接口
title: ADORecordConstruction 接口 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction
helpviewer_keywords:
- ADORecordConstruction interface [ADO]
ms.assetid: 52a5429e-5829-455e-be3b-31f05cbecf2d
author: rothja
ms.author: jroth
ms.openlocfilehash: feb9d7d691c69cefb17054c6162caa5e803da1b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035697"
---
# <a name="adorecordconstruction-interface"></a>ADORecordConstruction 接口
**ADORecordConstruction** 接口用于从 c/c + + 应用程序中的 OLE DB **行** 对象构造 ADO **记录** 对象。  
  
 此接口支持以下属性：  
  
## <a name="properties"></a>属性  
  
|属性|说明|  
|-|-|  
|[ParentRow](./parentrow-property-ado.md)|只写。<br />设置此 ADO **记录** 对象上 OLE DB **行** 对象的容器。|  
|[行](./row-property-ado.md)|读/写。<br />获取/设置此 ADO **记录** 对象的 OLE DB **行** 对象。|  
  
## <a name="methods"></a>方法  
 无。  
  
## <a name="events"></a>事件  
 无。  
  
## <a name="remarks"></a>备注  
 给定一个 OLE DB **Row** 对象 (`pRow`) ， () 的 ADO **Record** 对象的构造 `adoR` ，并将其分为以下三个基本操作：  
  
1.  创建 ADO **记录** 对象：  
  
    ```  
    _RecordPtr adoR;  
    adoRs.CreateInstance(__uuidof(_Record));  
    ```  
  
2.  查询 **Record** 对象上的 **IADORecordConstruction** 接口：  
  
    ```  
    adoRecordConstructionPtr adoRConstruct=NULL;  
    adoR->QueryInterface(__uuidof(ADORecordConstruction),  
                        (void**)&adoRConstruct);  
    ```  
  
3.  调用 **IADORecordConstruction：:p ut_Row** 属性方法，在 ADO **记录** 对象上设置 OLE DB **行** 对象：  
  
    ```  
    IUnknown *pUnk=NULL;  
    pRow->QueryInterface(IID_IUnknown, (void**)&pUnk);  
    adoRConstruct->put_Row(pUnk);  
    ```  
  
 生成的 **adoR** 对象现在表示从 OLE DB **行** 对象构造的 ADO **记录** 对象。  
  
 ADO **记录** 对象还可以从 OLE DB **行** 对象的容器构造。  
  
## <a name="requirements"></a>要求  
 **版本：** ADO 2.0 及更高版本  
  
 **库：** msado15.dll  
  
 **UUID：** 00000567-0000-0010-8000-00AA006D2EA4