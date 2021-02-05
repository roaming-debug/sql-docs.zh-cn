---
description: valueOf 方法 (java.sql.Timestamp, java.util.Calendar)
title: valueOf 方法 (java.sql.Timestamp, java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7320c383-0b06-446d-963b-7005e50324a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 232eeb75b783d463df17d7a836ab95b9b1b0ac8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181135"
---
# <a name="valueof-method-javasqltimestamp-javautilcalendar"></a>valueOf 方法 (java.sql.Timestamp, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  创建一个 DateTimeOffset 对象，表示按照距 GMT（给定 java.sql.Timestamp 值以及指示偏移量的 java.util.Calendar 值）的特定偏移量的时间点。  
  
## <a name="syntax"></a>语法  
  
```  
  
public static DateTimeOffset valueOf(java.sql.Timestamp timestamp, java.util.Calendar calendar)  
```  
  
#### <a name="parameters"></a>参数  
 *timestamp*  
  
 java.sql.Timestamp值。  
  
 calendar  
  
 偏移值。  将根据 timestamp 值设置 calendar 的日期和时间组件。  
  
## <a name="return-value"></a>返回值  
 返回一个 DateTimeOffset 对象，该对象表示在给定 java.util.Calendar 对象的时区由 java.sql.Timestamp 对象给定的时间点。  
  
## <a name="remarks"></a>注解  
 此方法还会将 java.util.Calendar 对象设置为由 java.sql.Timestamp 对象给定的时间点。  
  
## <a name="see-also"></a>另请参阅  
 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [DateTimeOffset 成员](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
