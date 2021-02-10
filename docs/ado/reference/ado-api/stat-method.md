---
description: Stat 方法
title: Stat 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Stat
helpviewer_keywords:
- Stat method [ADO]
ms.assetid: 99a2b2d4-e6b1-4205-b011-72d024ea7240
author: rothja
ms.author: jroth
ms.openlocfilehash: 684e824edc2238d2a6ba18d629c53fbab7ed7d3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051328"
---
# <a name="stat-method"></a>Stat 方法
检索有关 [流](./stream-object-ado.md) 对象的信息。  
  
## <a name="syntax"></a>语法  
  
```  
  
Long stream.Stat(StatStg, StatFlag)  
```  
  
## <a name="return-value"></a>返回值  
 一个 **长整型** 值，指示操作的状态。  
  
#### <a name="parameters"></a>参数  
 *StatStg*  
 将使用流的相关信息填充 STATSTG 结构。 ADO 流对象使用的 **Stat** 方法的实现并不填充结构的所有字段。  
  
 *StatFlag*  
 指定此方法不返回 STATSTG 结构中的某些成员，从而保存内存分配操作。 值取自 STATFLAG 枚举。 STATFLAG 枚举有两个值  
  
|返回的常量|Value|  
|--------------|-----------|  
|STATFLAG_DEFAULT|0|  
|STATFLAG_NONAME|1|  
  
## <a name="remarks"></a>备注  
 为 ADO 流对象实现的 Stat 方法版本将填充 STATSTG 结构的以下字段：  
  
 *pwcsName*  
 包含流名称的字符串（如果有），并且未指定 StatFlag 值 STATFLAG_NONAME。  
  
 *cbSize*  
 指定流或字节数组的大小（以字节为单位）。  
  
 *mtime*  
 指示此存储、流或字节数组的上次修改时间。  
  
 *ctime*  
 指示此存储、流或字节数组的创建时间。  
  
 *atime*  
 指示此存储、流或字节数组的上次访问时间。  
  
 如果在 StatFlag 参数中指定 STATFLAG_NONAME，则不返回流的名称。  
  
 如果未在 StatFlag 参数中指定 STATFLAG_NONAME，并且没有可用于当前流的名称，则将 E_NOTIMPL 此值。  
  
## <a name="applies-to"></a>应用于  
 [流对象 (ADO)](./stream-object-ado.md)