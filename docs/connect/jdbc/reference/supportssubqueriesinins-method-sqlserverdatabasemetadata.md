---
description: supportsSubqueriesInIns 方法 (SQLServerDatabaseMetaData)
title: supportsSubqueriesInIns 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSubqueriesInIns
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 77a0b5c0-0d8e-4e08-975f-4eeabb108ab1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 815430a48bdb36df48703f11899f5edbf3ab2de5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189121"
---
# <a name="supportssubqueriesinins-method-sqlserverdatabasemetadata"></a>supportsSubqueriesInIns 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此数据库是否支持 IN 语句中的子查询。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean supportsSubqueriesInIns()  
```  
  
## <a name="return-value"></a>返回值  
 如果支持，则值为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 supportsSubqueriesInIns 方法是由 java.sql.DatabaseMetaData 接口中的 supportsSubqueriesInIns 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
