---
description: GetDataProviderDSO 方法
title: GetDataProviderDSO 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- GetDataProviderDSO Method [ADO]
ms.assetid: 5a4c6bd5-0c79-4f81-a977-0561392d8d50
author: rothja
ms.author: jroth
ms.openlocfilehash: c784e5c3951f78ef28bfd0d0008570d1b1e68ca8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167274"
---
# <a name="getdataproviderdso-method"></a>GetDataProviderDSO 方法
从形状提供程序检索基础 OLE DB 数据源对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT GetDataProviderDSO(  
      IUnknown **ppDataProviderDSOIUnknown  
);  
```  
  
#### <a name="parameters"></a>参数  
 *ppDataProviderDSOIUnknown*  
 弄 指向指针的指针，该指针返回基础 OLE DB 数据源对象的 IUnknown。  
  
## <a name="remarks"></a>备注  
 此方法不会 addref 接口指针。 如果调用方计划持有指针，则调用方必须执行所需的 addref 和 release。  
  
## <a name="applies-to"></a>适用于  
 [IDSOShapeExtensions 接口](./idsoshapeextensions-interface.md)