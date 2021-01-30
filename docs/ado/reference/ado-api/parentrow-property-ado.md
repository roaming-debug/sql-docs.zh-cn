---
description: ParentRow 属性 (ADO)
title: " (ADO) 的 ParentRow 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::put_ParentRow
- ADORecordConstruction::ParentRow
- ADORecordConstruction::putParentRow
helpviewer_keywords:
- ParentRow property [ADO]
ms.assetid: 5ea8029b-eda4-490b-ae84-2ad036fb582f
author: rothja
ms.author: jroth
ms.openlocfilehash: 115689c06ce4bdb387b126ecc6087e63c705dcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170590"
---
# <a name="parentrow-property-ado"></a>ParentRow 属性 (ADO)
设置 **ADORecordConstruction** 对象上 OLE DB **行** 对象的容器，使该行的父对象变为 ADO **记录** 对象。  
  
 只写。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT put_ParentRow([in] IUnknown* pParent);  
```  
  
## <a name="parameters"></a>参数  
 *pParent*  
 行的容器。  
  
## <a name="return-values"></a>返回值  
 此属性方法返回标准的 HRESULT 值，包括 S_OK 和 E_FAIL。  
  
## <a name="applies-to"></a>应用于  
 [ADORecordConstruction 接口](./adorecordconstruction-interface.md)