---
description: storesUpperCaseQuotedIdentifiers 方法 (SQLServerDatabaseMetaData)
title: storesUpperCaseQuotedIdentifiers 方法 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.storesUpperCaseQuotedIdentifiers
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 936ec140-2597-44e6-82d3-3994a676ee35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d67988b6bd13aa1e0c52eeabe767b4316d708b4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183458"
---
# <a name="storesuppercasequotedidentifiers-method-sqlserverdatabasemetadata"></a>storesUpperCaseQuotedIdentifiers 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索此数据库是否将用双引号引起来的大小写混合的 SQL 标识符视为不区分大小写，并以大写方式存储它们。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean storesUpperCaseQuotedIdentifiers()  
```  
  
## <a name="return-value"></a>返回值  
 如果以大写形式存储标识符，则为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 storesUpperCaseQuotedIdentifiers 方法是由 java.sql.DatabaseMetaData 接口中的 storesUpperCaseQuotedIdentifiers 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
