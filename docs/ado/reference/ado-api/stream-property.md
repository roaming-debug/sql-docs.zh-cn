---
description: Stream 属性
title: Stream 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADOStreamConstruction::GetStream
- ADOStreamConstruction::PutStream
- ADOStreamConstruction::put_Stream
- ADOStreamConstruction::Stream
- ADOStreamConstruction::get_Stream
helpviewer_keywords:
- Stream property
ms.assetid: 4a44f9f6-0265-4c00-8def-d85b6af923b1
author: rothja
ms.author: jroth
ms.openlocfilehash: 21ae22283af2dff79094e87dc51f25cb359a6ff4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020302"
---
# <a name="stream-property"></a>Stream 属性
获取或设置 **ADOStreamConstruction** 对象上/的 OLE DB **流** 对象。  
  
 读/写。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT get_Stream([out, retval] IUnknown** ppStream);  
HRESULT put_Stream([in] IUnknown* pStream);  
```  
  
## <a name="parameters"></a>参数  
 *ppStream*  
 指向 OLE DB **流** 对象的指针。  
  
 *pStream*  
 一个 OLE DB 的 **流** 对象。  
  
## <a name="return-values"></a>返回值  
 此属性方法返回标准的 HRESULT 值。 这包括 S_OK 和 E_FAIL。  
  
## <a name="applies-to"></a>应用于  
 [ADOStreamConstruction 接口](./adostreamconstruction-interface.md)