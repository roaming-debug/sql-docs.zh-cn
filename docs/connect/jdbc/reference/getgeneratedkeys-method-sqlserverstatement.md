---
description: getGeneratedKeys 方法 (SQLServerStatement)
title: getGeneratedKeys 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getGeneratedKeys
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: a3325950-0e81-4ae8-aa0c-e1f6d371adcd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c55ef370ebc1413411dee4bfc951d6ce4c91cba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175714"
---
# <a name="getgeneratedkeys-method-sqlserverstatement"></a>getGeneratedKeys 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索因运行此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象而创建的任何自动生成的键。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final java.sql.ResultSet getGeneratedKeys()  
```  
  
## <a name="return-value"></a>返回值  
 ResultSet 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getGeneratedKeys 方法是由 java.sql.Statement 接口中的 getGeneratedKeys 方法指定的。  
  
 有关如何使用此方法的更多信息，请参阅[使用自动生成的键](../../../connect/jdbc/using-auto-generated-keys.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
