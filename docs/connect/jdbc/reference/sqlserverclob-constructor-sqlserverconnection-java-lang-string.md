---
description: SQLServerClob 构造函数 (SQLServerConnection, java.lang.String)
title: SQLServerClob 构造函数 (SQLServerConnection, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.SQLServerClob (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7058f4f7-ef3e-4d62-90d1-79299708b1eb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1b682ecf3dfc85415d8ca505be95fc65856bb465
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178370"
---
# <a name="sqlserverclob-constructor-sqlserverconnection-javalangstring"></a>SQLServerClob 构造函数 (SQLServerConnection, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  在给定 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象和数据字符串时，初始化 [SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md) 类的新实例。  
  
> [!NOTE]  
>  JDBC 驱动程序 2.0 版已不推荐使用此方法， 而是使用 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 类的 [createClob](../../../connect/jdbc/reference/createclob-method-sqlserverconnection.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```  
  
public SQLServerClob(SQLServerConnection connection,  
                     java.lang.String data)  
```  
  
#### <a name="parameters"></a>参数  
 连接  
  
 SQLServerConnection 对象。  
  
 *data*  
  
 CLOB 数据。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerClob 构造函数](../../../connect/jdbc/reference/sqlserverclob-constructors.md)   
 [SQLServerClob 成员](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [SQLServerClob 类](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
