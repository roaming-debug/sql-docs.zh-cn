---
description: get_OLEDBCommand 方法
title: get_OLEDBCommand 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- get_OLEDBCommand method [ADO]
ms.assetid: 23d551f5-3d5b-434b-ade6-fef15f1710e7
author: rothja
ms.author: jroth
ms.openlocfilehash: f287d19a4a34e9c71f06aab63910465ade3982d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024191"
---
# <a name="get_oledbcommand-method"></a>get_OLEDBCommand 方法
返回基础 OLE DB 命令，首先将在 ADO 命令上设置的所有参数信息传播到 OLE DB 命令。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT get_OLEDBCommand(  
      IUnknown **ppOLEDBCommand  
);  
```  
  
#### <a name="parameters"></a>参数  
 *ppOLEDBCommand*  
 弄一个指针，指向将写入基础 OLE DB 命令的 IUnknown 指针的指针位置。  
  
## <a name="applies-to"></a>应用于  
 [IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))