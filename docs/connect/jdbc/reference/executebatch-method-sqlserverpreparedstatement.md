---
description: executeBatch 方法 (SQLServerPreparedStatement)
title: executeBatch 方法 (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeBatch
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8418167e-cbd2-464d-b118-73cdd76080ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3e31d995f65a5f1a9e5ff9d3ff88bf3965e8061d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165926"
---
# <a name="executebatch-method-sqlserverpreparedstatement"></a>executeBatch 方法 (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  向数据库提交要运行的命令批。 如果所有命令都成功运行，则返回一个更新计数数组。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int[] executeBatch()  
```  
  
## <a name="return-value"></a>返回值  
 一个包含更新计数的整数数组。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
 java.sql.BatchUpdateException  
  
## <a name="remarks"></a>注解  
 此 executeBatch 方法是由 java.sql.Statement 接口中的 executeBatch 方法指定的。  
    
 此方法将重写 [SQLServerStatement.executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverstatement.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement 类](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
