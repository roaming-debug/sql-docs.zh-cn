---
description: getMaxProcedureNameLength 方法 (SQLServerDatabaseMetaData)
title: getMaxProcedureNameLength 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxProcedureNameLength
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e1c05eb3-8465-46fd-99bc-5e8effcafee5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3020742322ae3b51e6e498674ad6058f85f559f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162801"
---
# <a name="getmaxprocedurenamelength-method-sqlserverdatabasemetadata"></a>getMaxProcedureNameLength 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此数据库在过程名称中允许的最大字符数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getMaxProcedureNameLength()  
```  
  
## <a name="return-value"></a>返回值  
 指示允许的最大字符数的 int。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getMaxProcedureNameLength 方法是由 java.sql.DatabaseMetaData 接口中的 getMaxProcedureNameLength 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
