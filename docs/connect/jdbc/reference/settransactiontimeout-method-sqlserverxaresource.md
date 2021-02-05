---
description: setTransactionTimeout 方法 (SQLServerXAResource)
title: setTransactionTimeout 方法 (SQLServerXAResource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerXAResource.setTransactionTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 38bf4a1a-6ad3-437c-b9ed-8792ab6dde7e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6cfd7379a9315910b0822072268791a6c37cbcf0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172979"
---
# <a name="settransactiontimeout-method-sqlserverxaresource"></a>setTransactionTimeout 方法 (SQLServerXAResource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  为此 [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) 对象设置当前事务超时值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean setTransactionTimeout(int seconds)  
```  
  
#### <a name="parameters"></a>参数  
 *seconds*  
  
 int 值。  
  
## <a name="return-value"></a>返回值  
 如果已成功设置超时值，则为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 javax.transaction.xa.XAException  
  
## <a name="remarks"></a>注解  
 此 setTransactionTimeout 方法是由 javax.transaction.xa.XAResource 接口中的 setTransactionTimeout 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerXAResource 方法](../../../connect/jdbc/reference/sqlserverxaresource-methods.md)   
 [SQLServerXAResource 成员](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [SQLServerXAResource 类](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
