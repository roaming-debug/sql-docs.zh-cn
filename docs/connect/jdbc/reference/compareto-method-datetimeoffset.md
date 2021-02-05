---
description: compareTo 方法 (DateTimeOffset)
title: compareTo 方法 (DateTimeOffset) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e4cf2ea4-0fe9-40ce-ba79-f2a2b616997e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02e36e2e64a6a628fd088fab448b30ea0c238450
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176237"
---
# <a name="compareto-method-datetimeoffset"></a>compareTo 方法 (DateTimeOffset)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将此 DateTimeOffset 对象与另一个基于 GMT 时间的 DateTimeOffset 对象进行比较。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int compareTo(DateTimeOffset other)  
```  
  
#### <a name="parameters"></a>参数  
 要与当前实例进行比较的 [DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) 值。  
  
## <a name="return-value"></a>返回值  
 下表介绍了此方法的返回值：  
  
|返回值|说明|  
|------------------|-----------------|  
|0|两个 DateTimeOffset 对象都表示相同的时间点。|  
|负数|此 DateTimeOffset 对象表示在 other 之前的时间点。|  
|正数|此 DateTimeOffset 对象表示在 other 之后的时间点。|  
  
## <a name="remarks"></a>注解  
 当两个 DateTimeOffset 对象具有相同的 GMT 时间时，没有基于偏移量的对象的附加排序。  
  
## <a name="see-also"></a>另请参阅  
 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [DateTimeOffset 成员](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
