---
description: getServerPreparedStatementDiscardThreshold 方法 (SQLServerDataSource)
title: getServerPreparedStatementDiscardThreshold 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4edb5a71177d46b3babd4ca4ef36ecc080624c9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162301"
---
# <a name="getserverpreparedstatementdiscardthreshold-method-sqlserverdatasource"></a>getServerPreparedStatementDiscardThreshold 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回 serverPreparedStatementDiscardThreshold  连接属性的值。 此设置控制在执行调用以清除服务器上未完成的句柄之前，每个连接可以有多少个未完成的预定义语句放弃操作 (sp_unprepare) 处于未处理状态。 如果设置 <= 1，则在预定义语句关闭时将立即执行 unprepare 操作。 如果此值设置为 > 1，则会对这些调用进行批处理，以避免过于频繁地调用 sp_unprepare 的开销。

  
## <a name="syntax"></a>语法  
  
```
public int getServerPreparedStatementDiscardThreshold();  
```  
  
## <a name="return-value"></a>返回值  
 返回 serverPreparedStatementDiscardThreshold 连接属性的 int 值。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>备注  
 JDBC 驱动程序版本 6.4 及其后续版本中提供此方法。
 
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
