---
description: getMaxIndexLength 方法 (SQLServerDatabaseMetaData)
title: getMaxIndexLength 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxIndexLength
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7c85d021-d466-4732-85f9-53903d297041
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2b6b66f9f8c9add68fa9e1f5f1869d84e805d477
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175531"
---
# <a name="getmaxindexlength-method-sqlserverdatabasemetadata"></a>getMaxIndexLength 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此数据库允许的最大索引（包括索引的所有部分）字节数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getMaxIndexLength()  
```  
  
## <a name="return-value"></a>返回值  
 指示允许的最大字节数的 int。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getMaxIndexLength 方法是由 java.sql.DatabaseMetaData 接口中的 getMaxIndexLength 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
