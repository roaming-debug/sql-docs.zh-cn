---
description: setHoldability 方法 (SQLServerConnection)
title: setHoldability 方法 (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setHoldability
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 552eebd0-4c38-43f0-961f-35244f99109b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3f5b35e755449c61aa8e9b23bc05b1b71ab16318
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173418"
---
# <a name="setholdability-method-sqlserverconnection"></a>setHoldability 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将使用此 [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) 对象创建的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的可保持性更改为给定可保持性。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setHoldability(int nNewHold)  
```  
  
#### <a name="parameters"></a>参数  
 nNewHold  
  
 包含下列可保持性级别之一的 int 值：  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setHoldability 方法是由 java.sql.Connection 接口中的 setHoldability 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
