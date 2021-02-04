---
description: getMaxConnections 方法 (SQLServerDatabaseMetaData)
title: getMaxConnections 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxConnections
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 745410f7-e59b-4423-9728-c903adedc399
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dceb62cfa315646d4ca1c381cce433832e170072
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162813"
---
# <a name="getmaxconnections-method-sqlserverdatabasemetadata"></a>getMaxConnections 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索可能连接到此数据库的最大并发连接数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getMaxConnections()  
```  
  
## <a name="return-value"></a>返回值  
 指示允许的最大并发连接数的 int。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getMaxConnections 方法是由 java.sql.DatabaseMetaData 接口中的 getMaxConnections 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
