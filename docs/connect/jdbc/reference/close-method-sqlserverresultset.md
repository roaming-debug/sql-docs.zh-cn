---
description: close 方法 (SQLServerResultSet)
title: close 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.close
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8f3adf5b-874e-4cf2-b4ef-672dda42d77a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0a6f5e311333da6c16015acd2fe6ab007204b8d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168594"
---
# <a name="close-method-sqlserverresultset"></a>close 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  立即释放此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的数据库和 JDBC 资源，而不是在此对象自动关闭时等待上述操作发生。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void close()  
```  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 close 方法是由 java.sql.ResultSet 接口中的 close 方法指定的。  
  
 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象会自动关闭其在 SQLServerStatement 对象关闭、重新运行或用于从一个多结果序列中检索下一个结果时生成的 SQLServerResultSet 对象。 对 SQLServerResultSet 对象进行垃圾收集处理后，此对象会自动关闭。  
  
 当执行可生成单个大型只进、只读结果集的语句时，您所感兴趣的可能只是返回的结果集中的一些初始行集。 在此情况下，应用程序可能会在关闭结果集之前调用关联语句对象的 [cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md) 方法，以最大程度地减少放弃余下不必要的行所需要的处理时间。 在决定是否应使用此方法时，建议对照可节约的处理时间与取消执行所需的时间及与服务器之间的附加往返，在两者之间进行折衷。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
