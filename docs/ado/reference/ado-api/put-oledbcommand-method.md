---
description: put_OLEDBCommand 方法
title: put_OLEDBCommand 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- put_OLEDBCommand method [ADO]
ms.assetid: ca6a5804-bf5c-4afc-99db-22904bc0b33d
author: rothja
ms.author: jroth
ms.openlocfilehash: f3407244fa0042bd9c248998e7684d729e46665d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040807"
---
# <a name="put_oledbcommand-method"></a>put_OLEDBCommand 方法
此方法不执行任何操作，并且始终返回 S_OK。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT put_OLEDBCommand(  
      IUnknown *pOLEDBCommand  
);  
```  
  
#### <a name="parameters"></a>参数  
 *pOLEDBCommand*  
 中指向 OLE DB Command 对象的指针。  
  
## <a name="applies-to"></a>应用于  
 [IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))