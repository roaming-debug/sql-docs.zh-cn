---
description: finalize 方法 (SQLServerResultSet)
title: finalize 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.finalize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 49bc879d-822b-42da-bc20-2394865f1f0f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71dc58e3172657a480aa5465e48010953c32fc83
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168438"
---
# <a name="finalize-method-sqlserverresultset"></a>finalize 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  显式关闭此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void finalize()  
```  
  
## <a name="remarks"></a>备注  
 如果应用程序未关闭结果集，则关闭它。 提供此方法只是为了符合 JDBC 规范。 由于 Java 虚拟机 (JVM) 不保证何时可以运行终结器，因此忽略显式关闭其结果集的应用程序对于正在使用同一连接并因为公共服务器资源（如行锁）而阻塞的另一语句可能造成死锁。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
