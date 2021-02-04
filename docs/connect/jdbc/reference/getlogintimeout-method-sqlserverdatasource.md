---
description: getLoginTimeout 方法 (SQLServerDataSource)
title: getLoginTimeout 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getLoginTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 316f067c-9e08-456a-af19-b80b0bbd4a5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0748b46e99113e80266379361ed403815621feec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162886"
---
# <a name="getlogintimeout-method-sqlserverdatasource"></a>getLoginTimeout 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回此 [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) 对象在尝试建立连接时将等待的秒数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getLoginTimeout()  
```  
  
## <a name="return-value"></a>返回值  
 一个表示要等待的秒数的 int 值。  
  
## <a name="remarks"></a>注解  
 如果应用程序未明确指定超时值，则此方法会返回 15 秒的默认值。  
  
 此 getLoginTimeout 方法是由 javax.sql.DataSource 接口中的 getLoginTimeout 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
