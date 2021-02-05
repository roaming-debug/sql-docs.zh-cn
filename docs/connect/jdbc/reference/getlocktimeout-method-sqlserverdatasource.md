---
description: getLockTimeout 方法 (SQLServerDataSource)
title: getLockTimeout 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getLockTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 676094e9-ec18-4524-9b21-1f9c5b16dd52
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94d36653ea854f1a7c9b5d122343fcd524cab55f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175625"
---
# <a name="getlocktimeout-method-sqlserverdatasource"></a>getLockTimeout 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回指示数据库在报告锁定超时之前要等待的毫秒数的 int 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getLockTimeout()  
```  
  
## <a name="return-value"></a>返回值  
 包含数据库要等待的毫秒数的 int 值。  
  
## <a name="remarks"></a>注解  
 锁定超时是指在等待多少毫秒后数据库报告锁定超时。默认值 -1 表示无限期等待。 如果指定，该值将成为此连接上所有语句的默认值。  
  
> [!NOTE]  
>  值为 0 表示不等待。 如果未设置 lockTimeout 属性，getLockTimeout 方法将返回默认值 -1。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
