---
description: supportsGroupByBeyondSelect 方法 (SQLServerDatabaseMetaData)
title: supportsGroupByBeyondSelect 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsGroupByBeyondSelect
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: eadd2c37-d9ec-4b47-a91e-ed90b1eaf4b4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42864e6acc746e656a16b8f9f34c8975bf6450b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185206"
---
# <a name="supportsgroupbybeyondselect-method-sqlserverdatabasemetadata"></a>supportsGroupByBeyondSelect 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  在 SELECT 语句中的所有列均包含在 GROUP BY 子句中的情况下，检索此数据库是否支持使用 GROUP BY 子句中的 SELECT 语句不包含的列。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean supportsGroupByBeyondSelect()  
```  
  
## <a name="return-value"></a>返回值  
 如果支持，则值为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 supportsGroupByBeyondSelect 方法是由 java.sql.DatabaseMetaData 接口中的 supportsGroupByBeyondSelect 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
