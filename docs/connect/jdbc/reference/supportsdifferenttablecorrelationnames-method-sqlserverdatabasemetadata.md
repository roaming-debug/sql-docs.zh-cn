---
description: supportsDifferentTableCorrelationNames 方法 (SQLServerDatabaseMetaData)
title: supportsDifferentTableCorrelationNames 方法 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsDifferentTableCorrelationNames
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b4f8db0c-2eaf-476b-b916-3e83355778f7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5943beb8ae6ade712a9f301bec4dcf38723425d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159085"
---
# <a name="supportsdifferenttablecorrelationnames-method-sqlserverdatabasemetadata"></a>supportsDifferentTableCorrelationNames 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索在支持表相关名称时，这些名称是否必须与表名不同。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean supportsDifferentTableCorrelationNames()  
```  
  
## <a name="return-value"></a>返回值  
 如果支持，则值为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 supportsDifferentTableCorrelationNames 方法是由 java.sql.DatabaseMetaData 接口中的 supportsDifferentTableCorrelationNames 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
