---
description: isClosed 方法 (SQLServerResultSet)
title: isClosed 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 6081aa34-fc88-4dd0-9a3f-05e8488219dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4733b1877a0621f2f8974e3f3b221d745f3bfdcb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177436"
---
# <a name="isclosed-method-sqlserverresultset"></a>isClosed 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  指示此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象是否已关闭。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean isClosed()  
```  
  
## <a name="return-value"></a>返回值  
 如果此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象已关闭，则为 true，如果仍打开，则为 false。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 isClosed 方法是由 java.sql.ResultSet 接口中的 isClosed 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
