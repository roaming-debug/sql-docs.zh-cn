---
description: rollback 方法 (java.sql.Savepoint)
title: rollback 方法 (java.sql.Savepoint) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.rollback (java.sql.Savepoint)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d5dbd9ef-194f-4130-bfcc-7901a4fa8ded
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 97f6ad14c17ea7c42b79823a16efcae0615a9d6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174145"
---
# <a name="rollback-method-javasqlsavepoint"></a>rollback 方法 (java.sql.Savepoint)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  撤消在设置给定 [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) 对象后所做的所有更改。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void rollback(java.sql.Savepoint s)  
```  
  
#### <a name="parameters"></a>参数  
 *s*  
  
 要回滚到的 SavePoint 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 rollBack 方法是由 java.sql.Connection 接口中的 rollBack 方法指定的。  
  
 仅当已禁用自动提交模式时才应使用此方法。  
  
## <a name="see-also"></a>另请参阅  
 [rollback 方法 (SQLServerConnection)](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md)   
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
