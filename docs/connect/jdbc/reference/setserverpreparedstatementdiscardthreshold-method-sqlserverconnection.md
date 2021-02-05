---
description: setServerPreparedStatementDiscardThreshold 方法 (SQLServerConnection)
title: setServerPreparedStatementDiscardThreshold 方法 (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setServerPreparedStatementDiscardThreshold
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9c881ba19f22927bcfad281f2fa78d0059c77be7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178634"
---
# <a name="setserverpreparedstatementdiscardthreshold-method-sqlserverconnection"></a>setServerPreparedStatementDiscardThreshold 方法 (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 指定特定连接实例的行为。 此设置控制在执行调用以清除服务器上未完成的句柄之前，每个连接可以有多少个未完成的预定义语句放弃操作 (sp_unprepare) 处于未处理状态。 如果设置 <= 1，则在预定义语句关闭时将立即执行 un-prepare 操作。 如果将该值设置为 > 1，则这些调用将一起进行批处理，以避免过于频繁地调用 sp_unprepare 的开销。


## <a name="syntax"></a>语法  
  
```  
  
public void setServerPreparedStatementDiscardThreshold(boolean thresholdValue)  
```  

#### <a name="parameters"></a>参数  
 thresholdValue  
 
 serverPreparedStatementDiscardThreshold 连接属性的新值。  
 
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>备注  
 JDBC 驱动程序版本 6.4 及其后续版本中提供此方法。
 
## <a name="see-also"></a>另请参阅  
 [SQLServerConnection 成员](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection 类](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
