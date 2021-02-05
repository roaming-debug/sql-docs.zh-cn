---
description: cancelRowUpdates 方法 (SQLServerResultSet)
title: cancelRowUpdates 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.cancelRowUpdates
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2ecacca4-f7bc-4f5d-886a-da7747fdccae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5629e6ea7cb25331cbdeedd1440826c3687caf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168661"
---
# <a name="cancelrowupdates-method-sqlserverresultset"></a>cancelRowUpdates 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  取消对此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的当前行所做的更新。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void cancelRowUpdates()  
```  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 cancelRowUpdates 方法是由 java.sql.ResultSet 接口中的 cancelRowUpdates 方法指定的。  
  
 可以在调用 updater 方法后且在调用 [updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) 方法前调用此方法，以回滚对行所做的更新。 如果未进行更新或已调用 updateRow，则此方法不起作用。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
