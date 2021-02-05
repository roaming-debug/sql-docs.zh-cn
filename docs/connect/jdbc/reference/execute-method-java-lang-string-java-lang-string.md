---
description: execute 方法 (java.lang.String, java.lang.String)
title: execute 方法 (java.lang.String, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.execute (java.lang.String.java.lang.String[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9451c7c2-4c0d-4d1e-9b42-a26ff28e3f6a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4103e4a10aff5f16c68cfc584d31a1e6600e723
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168475"
---
# <a name="execute-method-javalangstring-javalangstring"></a>execute 方法 (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  运行可返回多项结果的给定的 SQL 语句，并向 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 发出信号以指明给定数组中指定的自动生成的键应对检索可用。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final boolean execute(java.lang.String sql,  
                             java.lang.String[] columnNames)  
```  
  
#### <a name="parameters"></a>参数  
 *sql*  
  
 包含 SQL 语句的 String。  
  
 columnNames  
  
 一个字符串数组，指示哪些自动生成的键的列名应可用。  
  
## <a name="return-value"></a>返回值  
 如果第一个结果为一个结果集，则为“true”。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此执行方法是由 java.sql.Statement 接口中的执行方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [execute 方法 (SQLServerStatement)](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)   
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
