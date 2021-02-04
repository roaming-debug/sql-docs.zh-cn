---
description: setDate 方法 (int, java.sql.Date, java.util.Calendar)
title: 设置日期和日历的 setDate 方法 - int | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setDate (int, java.sql.Date, java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2c46f694-6dc4-429f-a037-a3bad369a7c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad7d90a970fd0963fb037352219b96761780d66f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179066"
---
# <a name="setdate-method-int-javasqldate-javautilcalendar"></a>setDate 方法 (int, java.sql.Date, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为给定的日期和日历值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setDate(int n,  
                          java.sql.Date x,  
                          java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>参数  
 *n*  
  
 指示参数编号的 int。  
  
 *x*  
  
 Date 对象。  
  
 cal   
  
 Calendar 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setDate 方法是由 java.sql.PreparedStatement 接口中的 setDate 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [setDate 方法 &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setdate-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement 类](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
