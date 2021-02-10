---
description: IDSOShapeExtensions 接口
title: IDSOShapeExtensions 接口 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- IDSOShapeExtensions interface [ADO]
ms.assetid: ad4ba313-1161-4bc7-b8f6-4083305bc81e
author: rothja
ms.author: jroth
ms.openlocfilehash: 329c7b3193bd86ad05c757229477b5934d6e650d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020794"
---
# <a name="idsoshapeextensions-interface"></a>IDSOShapeExtensions 接口
获取形状提供程序的基础 OLE DB 数据源对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
interface IDSOShapeExtensions: public IUnknown {  
public:  
      HRESULT  GetDataProviderDSO(  
            IUnknown **ppDataProviderDSOIUnknown  
      );  
};  
```  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|-|-|  
|[GetDataProviderDSO 方法](./getdataproviderdso-method.md)|从形状提供程序检索基础 OLE DB 数据源对象。|  
  
## <a name="requirements"></a>要求  
 **版本：** ADO 2.0 及更高版本  
  
 **库：** msado15.dll  
  
 **UUID：** 00000283-0000-0010-8000-00AA006D2EA4