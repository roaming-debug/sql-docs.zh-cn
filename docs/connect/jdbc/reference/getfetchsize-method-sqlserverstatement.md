---
description: getFetchSize 方法 (SQLServerStatement)
title: getFetchSize 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getFetchSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8115ca58-8ae9-46ce-8515-7905d7bb25fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216ed0d1989a1646add0910fab4f40a929ecd0ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163036"
---
# <a name="getfetchsize-method-sqlserverstatement"></a>getFetchSize 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索结果集行数，该行数是由此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象生成的结果集对象的默认提取大小。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final int getFetchSize()  
```  
  
## <a name="return-value"></a>返回值  
 指示提取大小的 int，该提取大小由 [setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md) 方法指定。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getFetchSize 方法是由 java.sql.Statement 接口中的 getFetchSize 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
