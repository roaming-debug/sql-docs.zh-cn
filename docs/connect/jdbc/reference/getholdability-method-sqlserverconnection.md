---
description: getHoldability 方法 (SQLServerConnection)
title: getHoldability 方法 (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getHoldability
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b1644791-c36a-4837-86c4-9299537ee1c2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab0e93dabbd5f4d8ce27be2381629760afb89ca7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162999"
---
# <a name="getholdability-method-sqlserverconnection"></a>getHoldability 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索使用此 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象创建的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前可保持性。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getHoldability()  
```  
  
## <a name="return-value"></a>返回值  
 包含下列可保持性级别之一的 int 值：  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getHoldability 方法是由 java.sql.Connection 接口中的 getHoldability 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
