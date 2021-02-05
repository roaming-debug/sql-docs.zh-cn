---
description: getMaxColumnsInGroupBy 方法 (SQLServerDatabaseMetaData)
title: getMaxColumnsInGroupBy 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxColumnsInGroupBy
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: a59cfe98-c0f4-46ad-9243-62aa56855f1a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03078948707d7aaa97ceb7cd6833165ac2ce0690
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175582"
---
# <a name="getmaxcolumnsingroupby-method-sqlserverdatabasemetadata"></a>getMaxColumnsInGroupBy 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此数据库在 GROUP BY 子句中允许的最大列数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getMaxColumnsInGroupBy()  
```  
  
## <a name="return-value"></a>返回值  
 指示允许的最大列数的 int。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getMaxColumnsInGroupBy 方法是由 java.sql.DatabaseMetaData 接口中的 getMaxColumnsInGroupBy 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
