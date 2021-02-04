---
description: getFetchDirection 方法 (SQLServerStatement)
title: getFetchDirection 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ceb4ae68-decc-46d3-83f1-0bbd23aaf58c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a2e23066c13ecba2034b79432fededbf88edaa07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163049"
---
# <a name="getfetchdirection-method-sqlserverstatement"></a>getFetchDirection 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索从数据库表中提取行的方向，这是通过此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象生成的结果集的默认方向。  
  
> [!NOTE]  
>  [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 目前未实现此方法。 因此，它将始终返回 FETCH_UNKNOWN。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final int getFetchDirection()  
```  
  
## <a name="return-value"></a>返回值  
 指示由 [setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md) 方法指定的提取方向的 int 值。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getFetchDirection 方法是由 java.sql.Statement 接口中的 getFetchDirection 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
