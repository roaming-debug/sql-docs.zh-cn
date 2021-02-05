---
description: executeUpdate 方法 (SQLServerStatement)
title: executeUpdate 方法 (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.executeUpdate
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 10ae662a-ce3c-4b24-875c-5c2df319d93b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8134bd0c4523da21bd46ebea7547c6ca85e978e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168521"
---
# <a name="executeupdate-method-sqlserverstatement"></a>executeUpdate 方法 (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  运行给定的 SQL 语句，可以是 INSERT、UPDATE 或 DELETE 语句；或不返回任何内容的 SQL 语句，例如 SQL DDL 语句。 从 [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 开始，executeUpdate 将返回在 MERGE 操作中更新的正确行数。  
  
## <a name="overload-list"></a>重载列表  
  
|名称|说明|  
|----------|-----------------|  
|[executeUpdate (java.lang.String)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-sqlserverstatement.md)|运行给定的 SQL 语句，可以是 INSERT、UPDATE、DELETE 或 MERGE 语句；或不返回任何内容的 SQL 语句，例如 SQL DDL 语句。|  
|[executeUpdate (java.lang.String, int)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-int.md)|运行给定的 SQL 语句，并通过给定的标记向 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 发出信号，指示由此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象自动生成的键是否应对检索可用。|  
|[executeUpdate (java.lang.String, int&#91;&#93;)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string.md)|运行给定的 SQL 语句，并向 JDBC 驱动程序发出信号，指示给定数组中指示的自动生成的键应对检索可用。|  
|[executeUpdate (java.lang.String, java.lang.String&#91;&#93;)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-java-lang-string.md)|运行给定的 SQL 语句，并向 JDBC 驱动程序发出信号，指示给定数组中指示的自动生成的键应对检索可用。|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
