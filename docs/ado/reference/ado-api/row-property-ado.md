---
description: Row 属性 (ADO)
title: " (ADO) 的行属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::PutRow
- ADORecordConstruction::GetRow
- ADORecordConstruction::get_Row
- ADORecordConstruction::Row
- ADORecordConstruction::put_Row
helpviewer_keywords:
- Row property [ADO]
ms.assetid: 21019d89-2dd1-4a26-ac6f-384b81d66949
author: rothja
ms.author: jroth
ms.openlocfilehash: cf90fe1219103713797dfd04b1475d7fc77c80a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170347"
---
# <a name="row-property-ado"></a>Row 属性 (ADO)
获取或设置 [ADORecordConstruction 接口](./adorecordconstruction-interface.md)对象上或的 OLE DB **行** 对象。 使用 **put_Row** 设置 **行** 对象时，会将行转换为 ADO **记录** 对象。  
  
## <a name="readwritesyntax"></a>读/写。语法  
  
```  
HRESULT get_Row([out, retval] IUnknown** ppRow);  
HRESULT put_Row([in] IUnknown* pRow);  
```  
  
## <a name="parameters"></a>参数  
 *ppRow*  
 指向 OLE DB **行** 对象的指针。  
  
 *PRow*  
 一个 OLE DB **行** 对象。  
  
## <a name="return-values"></a>返回值  
 此属性方法返回标准的 HRESULT 值，包括 S_OK 和 E_FAIL。  
  
## <a name="applies-to"></a>应用于  
 [ADORecordConstruction 接口](./adorecordconstruction-interface.md)