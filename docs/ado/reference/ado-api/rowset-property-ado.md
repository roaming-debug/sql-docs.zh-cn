---
description: Rowset 属性 (ADO)
title: " (ADO) 的行集属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordsetConstruction::PutRowset
- ADORecordsetConstruction::GetRowset
- ADORecordsetConstruction::Rowset
- ADORecordsetConstruction::put_Rowset
- ADORecordsetConstruction::get_Rowset
helpviewer_keywords:
- Rowset property [ADO]
ms.assetid: 7d359294-4ff2-47e0-8111-0c221b24d80e
author: rothja
ms.author: jroth
ms.openlocfilehash: 4df128f2885dc3f4a582c34ffdb436dc255b538c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051528"
---
# <a name="rowset-property-ado"></a>Rowset 属性 (ADO)
获取或设置 **ADORecordsetConstruction** 对象上/的 OLE DB **行集** 对象。 使用 put_Rowset 时，行集会转换为 ADO **记录集** 对象。  
  
 读/写。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT get_Rowset([out, retval] IUnknown** ppRowset);  
HRESULT put_Rowset([in] IUnknown* pRowset);  
```  
  
## <a name="parameters"></a>参数  
 *ppRowset*  
 指向 OLE DB **行集** 对象的指针。  
  
 *PRowset*  
 一个 OLE DB **行集** 对象。  
  
## <a name="return-values"></a>返回值  
 此属性方法返回标准的 HRESULT 值，包括 S_OK 和 E_FAIL。  
  
## <a name="applies-to"></a>应用于  
 [ADORecordsetConstruction 接口](./adorecordsetconstruction-interface.md)